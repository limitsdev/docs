# Agents as Main Cycle — Implementation Plan (Backend + DB + Frontend)

This document is the **step-by-step plan** to make agents **dynamic** (non-static, non-mock) and align the database, backend, and frontend with the [Agents as Main Cycle](../limitsfrontend/docs/AGENTS-AS-MAIN-CYCLE-CHANGELOG.md) UI.

**Current state:** Agents list and agent detail pages use `MOCK_AGENTS` and in-memory create; agent dashboard uses real policies from the API but attached policies are local state only (lost on refresh).

**Goal:** Agents are stored in the DB, scoped by organization; create/list/get-by-slug go through the API; agent dashboard loads and saves attached policies (conditions, instructions, guardrails) via the backend.

---

## Execution Plan (do this now)

Use this order. Each phase blocks the next.

| Phase | Scope | Do first → last |
|-------|--------|------------------|
| **A** | DB only | 1) Edit `schema.prisma` (agents + agent_policies). 2) Create migration. 3) Run `npx prisma migrate dev`. |
| **B** | Backend only | 4) Implement agents CRUD (controller, service, DTOs, org-scoped). 5) Implement agent-policies (list, attach, detach or PUT). 6) Register routes and test with API client. |
| **C** | Frontend API | 7) Add `/api/agents` (GET, POST), `/api/agents/[id]` (GET, PATCH, DELETE), `/api/agents/by-slug/[slug]` (GET), `/api/agents/[id]/policies` (GET, PUT or POST/DELETE). |
| **D** | Frontend hooks + types | 8) Add types (`Agent`, `AgentPolicyAttachment`) and hooks: `useAgents`, `useAgentBySlug`, `useCreateAgent`, `useAgentPolicies`, `useSetAgentPolicies`. |
| **E** | Frontend pages | 9) Agents list page: useAgents + useCreateAgent. 10) Agent layout: useAgentBySlug. 11) Agent dashboard: useAgentPolicies + persist attach/detach. |
| **F** | Cleanup | 12) Remove or gate mock data (`agents-mock.ts`); verify no static agent references. |

**Quick checklist (copy and tick as you go):**

- [ ] **A1** Prisma: add `agents` and `agent_policies` models + relations
- [ ] **A2** Create migration `add_agents_and_agent_policies`
- [ ] **A3** Run migration
- [ ] **B1** Backend: agents module (controller, service, DTOs)
- [ ] **B2** Backend: GET/POST agents, GET/PATCH/DELETE by id, GET by slug
- [ ] **B3** Backend: GET/POST/DELETE (or PUT) agent policies
- [ ] **C1** Frontend: `/api/agents` and `/api/agents/[id]` and `/api/agents/by-slug/[slug]`
- [ ] **C2** Frontend: `/api/agents/[id]/policies`
- [ ] **D1** Frontend: Agent types + useAgents, useAgentBySlug, useCreateAgent
- [ ] **D2** Frontend: useAgentPolicies, useSetAgentPolicies
- [ ] **E1** Agents list page: fetch + create from API
- [ ] **E2** Agent layout: resolve agent by slug from API
- [ ] **E3** Agent dashboard: load/save attached policies
- [ ] **F1** Remove or reduce mock; verify flows

---

## Overview of Steps

| Step | Area | What |
|------|------|------|
| 1 | Database | Add `agents` table and `agent_policies` junction table; migration |
| 2 | Backend | Agents CRUD API (list, create, get by slug, update, delete) |
| 3 | Backend | Agent-scoped policies API (list attached, attach, detach) |
| 4 | Frontend API | Next.js API routes for agents and agent policies |
| 5 | Frontend | Types and React Query hooks (useAgents, useAgent, useCreateAgent, useAgentPolicies) |
| 6 | Frontend | Wire agents list page (fetch list, create agent via API) |
| 7 | Frontend | Wire agent layout and detail (resolve agent by slug from API) |
| 8 | Frontend | Wire agent dashboard (load/save attached policies) |
| 9 | Optional | Last activity, agent-scoped logs/escalations/integrations |

---

## Step 1 — Database: Agents and Agent–Policy Link

### 1.1 New tables

**`agents`**

- `id` (UUID, PK)
- `organization_id` (FK → organizations, required)
- `slug` (string, unique per org) — used in URLs, e.g. `/agents/support-bot`
- `name` (string)
- `description` (string, optional)
- `status` (e.g. `ACTIVE` | `INACTIVE` or keep minimal and derive from policies)
- `created_at`, `updated_at`
- Unique constraint: `(organization_id, slug)`

**`agent_policies`** (which policies are attached to which agent, and in which mode)

- `id` (UUID, PK)
- `agent_id` (FK → agents, CASCADE)
- `policy_id` (FK → policies, CASCADE)
- `mode` (enum or string: `CONDITIONS` | `INSTRUCTIONS` | `GUARDRAIL`)
- `created_at` (optional)
- Unique constraint: `(agent_id, policy_id, mode)` or `(agent_id, policy_id)` if one policy can only be in one mode per agent (decide per product: one policy in multiple sections vs one section only).

### 1.2 Prisma schema changes

- Add model `agents` with relation to `organizations` and `agent_policies`.
- Add model `agent_policies` with relations to `agents` and `policies`.
- Add `agents` relation on `organizations` and `agent_policies` on `policies`.

### 1.3 Migration

- Create migration (e.g. `add_agents_and_agent_policies`).
- Run migration against dev DB.

**Deliverable:** Migration runs cleanly; Prisma Client includes `agents` and `agent_policies`.

---

## Step 2 — Backend: Agents CRUD API

### 2.1 NestJS module and DTOs

- **Module:** Reuse or create an `agents` resource module (e.g. under `backend/src/agents` for “application agents” — distinct from existing `agent` LLM/generation service if needed). Ensure it has access to Prisma (or TypeORM) and org context.
- **DTOs:**  
  - Create: `name`, `description`, optional `slug` (default: slugify from name).  
  - Update: optional `name`, `description`, `slug`, `status`.  
  - Response: `id`, `organizationId`, `slug`, `name`, `description`, `status`, `createdAt`, `updatedAt`, and optionally `policyCount`, `lastActivity` if you want to avoid extra queries in list.

### 2.2 Endpoints (all org-scoped via auth)

- `GET /agents` — List agents for the current organization. Query: optional `page`, `limit`, `status`. Return array and total/cursor as needed.
- `POST /agents` — Create agent. Body: `name`, `description?`, `slug?`. Generate slug from name if not provided; ensure uniqueness per org.
- `GET /agents/:id` — Get one agent by ID (ensure org).
- `GET /agents/by-slug/:slug` — Get one agent by slug for current org (for frontend routing).
- `PATCH /agents/:id` — Update agent (name, description, slug, status).
- `DELETE /agents/:id` — Delete agent (and cascade `agent_policies`).

### 2.3 Auth and org context

- Resolve organization from authenticated user (e.g. JWT/Clerk token, membership table). All agents must be filtered/created under that `organization_id`.

**Deliverable:** Backend exposes agents CRUD; list/create/get-by-id/get-by-slug/update/delete work with org scope.

---

## Step 3 — Backend: Agent-Scoped Policies API

### 3.1 Endpoints

- `GET /agents/:agentId/policies` — List policies attached to this agent. Response: array of `{ policyId, policyKey, mode }` or full policy summary. Ensure `agentId` belongs to current org.
- `POST /agents/:agentId/policies` — Attach a policy. Body: `policyId`, `mode` (`CONDITIONS` | `INSTRUCTIONS` | `GUARDRAIL`). Idempotent if you use unique `(agent_id, policy_id, mode)`.
- `DELETE /agents/:agentId/policies/:policyId` — Detach policy (optionally with `mode` in query if same policy can be in multiple modes).

Alternatively, a single **PUT /agents/:agentId/policies** that accepts the full list of `{ policyId, mode }[]` and replaces the set (simpler for dashboard “save flow”).

### 3.2 Business rules

- Policy must belong to the same organization as the agent.
- Validate `mode` against allowed enum.

**Deliverable:** Backend can list, attach, and detach (or replace) agent policies; all scoped to org.

---

## Step 4 — Frontend: Next.js API Routes for Agents

### 4.1 Routes (proxy to backend with Clerk auth)

- `GET /api/agents` — Proxy to `GET /agents` (with pagination params if needed).
- `POST /api/agents` — Proxy to `POST /agents` (body: name, description?, slug?).
- `GET /api/agents/[id]` — Proxy to `GET /agents/:id` (by id).
- `GET /api/agents/by-slug/[slug]` — Proxy to `GET /agents/by-slug/:slug`.
- `PATCH /api/agents/[id]` — Proxy to `PATCH /agents/:id`.
- `DELETE /api/agents/[id]` — Proxy to `DELETE /agents/:id`.
- `GET /api/agents/[id]/policies` — Proxy to `GET /agents/:agentId/policies`.
- `POST /api/agents/[id]/policies` or `PUT /api/agents/[id]/policies` — Proxy to attach or replace agent policies.

Use same pattern as existing `/api/policies`: get Bearer token from Clerk, forward to `NEXT_PUBLIC_BACKEND_URL`, return JSON.

**Deliverable:** Frontend can call agents and agent-policies through Next.js API routes with auth.

---

## Step 5 — Frontend: Types and Hooks

### 5.1 Types

- Define or reuse `Agent` type matching backend: `id`, `slug`, `name`, `description`, `status`, `organizationId`, `createdAt`, `updatedAt`, and optionally `policyCount`, `lastActivity` if backend returns them.
- Define `AgentPolicyAttachment`: `policyId`, `mode`, and optionally `policyKey` for display.

### 5.2 Hooks (React Query)

- `useAgents(params?)` — GET `/api/agents`, returns list (and total if paginated).
- `useAgent(id | slug)` — GET by id or by slug (use `/api/agents/by-slug/[slug]` for slug).
- `useCreateAgent()` — mutation POST `/api/agents`; on success invalidate agents list and optionally redirect to new agent.
- `useUpdateAgent()` — mutation PATCH `/api/agents/[id]`.
- `useDeleteAgent()` — mutation DELETE `/api/agents/[id]`.
- `useAgentPolicies(agentId)` — GET `/api/agents/[id]/policies`, returns attached policies per mode.
- `useSetAgentPolicies(agentId)` or `useAttachAgentPolicy(agentId)` / `useDetachAgentPolicy(agentId)` — mutations to update attached policies (match backend: either PUT full list or attach/detach).

**Deliverable:** All agent and agent-policy reads/writes go through these hooks; types align with backend.

---

## Step 6 — Frontend: Wire Agents List Page

### 6.1 Replace mock data

- In `src/app/(app)/agents/page.tsx`, remove `MOCK_AGENTS` and local `useState(agents)`.
- Use `useAgents()` to fetch the list; show loading and error states.
- Create-agent dialog: use `useCreateAgent()`; on success invalidate `useAgents` and redirect to `/agents/[slug]` or close dialog and let list refresh.

### 6.2 Slug generation

- If backend accepts optional `slug` on create, frontend can send slug from `slugFromName(name)` or leave empty and let backend generate. Ensure backend guarantees unique slug per org.

**Deliverable:** Agents list is loaded from API; creating an agent persists to DB and appears in the list.

---

## Step 7 — Frontend: Wire Agent Layout and Detail

### 7.1 Resolve agent by slug from API

- In `src/app/(app)/agents/[agentSlug]/layout.tsx`, replace `getAgentBySlug(param)` with a fetch: e.g. `useAgentBySlug(agentSlug)` that calls `GET /api/agents/by-slug/[slug]`.
- Keep fallback to resolve by id if you support both (e.g. redirect id → slug as today).
- Show loading state while resolving; 404 if no agent.

### 7.2 Agent context

- Keep `AgentProvider` with the agent object; now the object comes from API (same shape as current `Agent` type).

**Deliverable:** Opening `/agents/support-bot` (or by id) loads the agent from the backend; layout and children receive the same agent context shape.

---

## Step 8 — Frontend: Wire Agent Dashboard (Policies Flow)

### 8.1 Load attached policies

- In `src/app/(app)/agents/[agentSlug]/dashboard/page.tsx`, use `useAgentPolicies(agent.id)` to load the initial set of attached policies by mode (conditions, instructions, guardrail).
- Initialize `attachedPolicies` state from this data instead of empty array.

### 8.2 Persist attached policies

- When user attaches/detaches a policy in the flow UI, call the backend (e.g. `useSetAgentPolicies(agent.id)` with the full list, or attach/detach mutations).
- On success, invalidate `useAgentPolicies(agent.id)` and optionally refetch or update local state.

### 8.3 Optional: persist flow layout

- If you want to save React Flow node positions, add an optional `flow_layout` JSON field on `agents` and endpoints to GET/PATCH it; then load/save in the dashboard page. Can be a later step.

**Deliverable:** Dashboard shows attached policies from DB; attaching/detaching policies persists and survives refresh.

---

## Step 9 — Optional: Enrichment and Agent-Scoped Features

- **Last activity:** Compute or store `last_activity` per agent (e.g. from `logs` or `agent_sessions` by agent or by policies attached to agent). Expose in list/detail and in GET agent response.
- **Agent-scoped logs / escalations / integrations:** If product requires “logs for this agent” or “escalations for this agent,” add agent_id to the relevant tables or derive via agent_policies (e.g. logs filtered by policies attached to agent). Then add agent-scoped API and wire the placeholder pages under `/agents/[agentSlug]/logs`, etc.

---

## File Checklist (for implementation)

### Database

- [ ] `backend/prisma/schema.prisma` — add `agents`, `agent_policies`, relations
- [ ] `backend/prisma/migrations/XXXXXX_add_agents_and_agent_policies/migration.sql`

### Backend

- [ ] Agents resource: controller, service, DTOs (create, update, response)
- [ ] Endpoints: list, create, get by id, get by slug, update, delete
- [ ] Agent-policies: controller/service methods and endpoints (list, attach, detach or PUT)
- [ ] Auth/org resolution in all agents routes

### Frontend API

- [ ] `src/app/api/agents/route.ts` (GET, POST)
- [ ] `src/app/api/agents/[id]/route.ts` (GET, PATCH, DELETE)
- [ ] `src/app/api/agents/by-slug/[slug]/route.ts` (GET)
- [ ] `src/app/api/agents/[id]/policies/route.ts` (GET, POST or PUT)

### Frontend hooks and types

- [ ] Shared type `Agent` (and `AgentPolicyAttachment`) e.g. in `src/lib/agents.ts` or next to hooks
- [ ] `src/hooks/use-agents.ts` (useAgents, useAgent, useAgentBySlug, useCreateAgent, useUpdateAgent, useDeleteAgent)
- [ ] `src/hooks/use-agent-policies.ts` (useAgentPolicies, useSetAgentPolicies or attach/detach)

### Frontend pages

- [ ] `src/app/(app)/agents/page.tsx` — useAgents, useCreateAgent
- [ ] `src/app/(app)/agents/[agentSlug]/layout.tsx` — useAgentBySlug (or useAgent)
- [ ] `src/app/(app)/agents/[agentSlug]/dashboard/page.tsx` — useAgentPolicies, persist attach/detach

### Cleanup

- [ ] Remove or reduce `MOCK_AGENTS` / `getAgentBySlug` from `src/lib/agents-mock.ts` (keep only if still needed for demos or delete)
- [ ] Ensure no remaining static agent data in agent detail pages

---

## Summary

1. **DB:** Add `agents` and `agent_policies`; run migration.  
2. **Backend:** Implement agents CRUD and agent-policies list/attach/detach (or replace) with org scoping.  
3. **Frontend API:** Add Next.js routes that proxy to backend with auth.  
4. **Frontend hooks:** useAgents, useAgent, useAgentBySlug, useCreateAgent, useAgentPolicies, useSetAgentPolicies.  
5. **Frontend pages:** Agents list and create from API; agent layout resolves by slug from API; dashboard loads and saves attached policies.

This plan keeps the existing “agents as main cycle” UI and makes it backed by the database and APIs step by step.
