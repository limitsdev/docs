# Agents as Main Cycle — Plan Verification

This checklist confirms the implementation follows [AGENTS-AS-MAIN-CYCLE-IMPLEMENTATION-PLAN.md](AGENTS-AS-MAIN-CYCLE-IMPLEMENTATION-PLAN.md).

---

## Step 1 — Database

| Plan requirement | Status | Notes |
|------------------|--------|--------|
| `agents`: id (UUID), organization_id (FK), slug, name, description (optional), status, created_at, updated_at | Done | [backend/prisma/schema.prisma](backend/prisma/schema.prisma) |
| Unique `(organization_id, slug)` | Done | `@@unique([organization_id, slug])` |
| `agent_policies`: id, agent_id (FK CASCADE), policy_id (FK CASCADE), mode (CONDITIONS/INSTRUCTIONS/GUARDRAIL), created_at | Done | Uses existing `policy_mode` enum |
| Unique `(agent_id, policy_id, mode)` | Done | `@@unique([agent_id, policy_id, mode])` |
| Relations: organizations.agents, agents.agent_policies, policies.agent_policies | Done | |
| Migration created and run | Done / Pending | Migration file exists; run `npx prisma migrate dev` when DB is ready |

---

## Step 2 — Backend: Agents CRUD

| Plan requirement | Status | Notes |
|------------------|--------|--------|
| Module distinct from existing LLM “agent” service | Done | New module `backend/src/app-agents` at path **/app-agents** (plan text says “/agents”; we use /app-agents to avoid clashing with existing `POST /agents/generate-policy`, `GET /agents/messages/:threadId`) |
| Create DTO: name, description?, slug? | Done | [app-agents/dto/create-app-agent.dto.ts](backend/src/app-agents/dto/create-app-agent.dto.ts) |
| Update DTO: name?, description?, slug?, status? | Done | [app-agents/dto/update-app-agent.dto.ts](backend/src/app-agents/dto/update-app-agent.dto.ts) |
| Response: id, organizationId, slug, name, description, status, createdAt, updatedAt, policyCount? | Done | policyCount in list/detail; lastActivity optional in plan, not implemented |
| GET list (page, limit, status) | Done | `GET /app-agents` |
| POST create (slugify from name if no slug) | Done | `POST /app-agents` |
| GET by id | Done | `GET /app-agents/:id` |
| GET by slug | Done | `GET /app-agents/by-slug/:slug` |
| PATCH update | Done | `PATCH /app-agents/:id` |
| DELETE (cascade agent_policies) | Done | `DELETE /app-agents/:id` |
| Auth + org: ClerkAuthGuard, req.organization.id | Done | All routes org-scoped |

---

## Step 3 — Backend: Agent-Scoped Policies

| Plan requirement | Status | Notes |
|------------------|--------|--------|
| GET list attached: `{ policyId, policyKey, mode }[]` | Done | `GET /app-agents/:id/policies` |
| PUT full list (replace) | Done | `PUT /app-agents/:id/policies` with `{ attachments: [{ policyId, mode }] }` |
| Policy same org as agent | Done | Validated in service |
| Mode validated (CONDITIONS / INSTRUCTIONS / GUARDRAIL) | Done | DTO + enum |

Plan also allows POST attach + DELETE detach; we implemented **PUT replace** only, as per “Alternatively, a single PUT … replaces the set.”

---

## Step 4 — Frontend API Routes

| Plan requirement | Status | Notes |
|------------------|--------|--------|
| GET /api/agents | Done | Proxies to GET /app-agents |
| POST /api/agents | Done | Proxies to POST /app-agents |
| GET /api/agents/[id] | Done | Proxies to GET /app-agents/:id |
| GET /api/agents/by-slug/[slug] | Done | Proxies to GET /app-agents/by-slug/:slug |
| PATCH /api/agents/[id] | Done | Proxies to PATCH /app-agents/:id |
| DELETE /api/agents/[id] | Done | Proxies to DELETE /app-agents/:id |
| GET /api/agents/[id]/policies | Done | Proxies to GET /app-agents/:id/policies |
| PUT /api/agents/[id]/policies | Done | Proxies to PUT /app-agents/:id/policies |
| Clerk auth, proxy to backend | Done | Same pattern as /api/policies |

---

## Step 5 — Frontend Types and Hooks

| Plan requirement | Status | Notes |
|------------------|--------|--------|
| Agent type (id, slug, name, description, status, organizationId?, createdAt?, updatedAt?, policyCount?, lastActivity?) | Done | [limitsfrontend/src/lib/agents.ts](limitsfrontend/src/lib/agents.ts); display status derived from API |
| AgentPolicyAttachment: policyId, mode, policyKey? | Done | In lib/agents and use-agent-policies |
| useAgents(params?) | Done | [limitsfrontend/src/hooks/use-agents.ts](limitsfrontend/src/hooks/use-agents.ts) |
| useAgent by id or slug | Done | useAgentById(id), useAgentBySlug(slug) |
| useCreateAgent() | Done | |
| useUpdateAgent(id) | Done | |
| useDeleteAgent() | Done | |
| useAgentPolicies(agentId) | Done | [limitsfrontend/src/hooks/use-agent-policies.ts](limitsfrontend/src/hooks/use-agent-policies.ts) |
| useSetAgentPolicies(agentId) | Done | PUT full list |

---

## Step 6 — Agents List Page

| Plan requirement | Status | Notes |
|------------------|--------|--------|
| Remove MOCK_AGENTS and local state | Done | [limitsfrontend/src/app/(app)/agents/page.tsx](limitsfrontend/src/app/(app)/agents/page.tsx) |
| useAgents(), loading and error | Done | |
| useCreateAgent(); on success invalidate and redirect to /agents/[slug] | Done | |
| Slug: backend accepts optional slug or generates | Done | Backend slugifies name if slug omitted |

---

## Step 7 — Agent Layout and Detail

| Plan requirement | Status | Notes |
|------------------|--------|--------|
| Replace getAgentBySlug with useAgentBySlug(slug) → GET /api/agents/by-slug/[slug] | Done | [limitsfrontend/src/app/(app)/agents/[agentSlug]/layout.tsx](limitsfrontend/src/app/(app)/agents/[agentSlug]/layout.tsx) |
| Fallback: resolve by id and redirect to slug URL | Done | When param looks like UUID, useAgentById then router.replace to /agents/[slug] |
| Loading state; 404 if no agent | Done | |
| AgentProvider with agent from API | Done | |

---

## Step 8 — Agent Dashboard

| Plan requirement | Status | Notes |
|------------------|--------|--------|
| useAgentPolicies(agent.id) to load attached policies | Done | [limitsfrontend/src/app/(app)/agents/[agentSlug]/dashboard/page.tsx](limitsfrontend/src/app/(app)/agents/[agentSlug]/dashboard/page.tsx) |
| Initialize attachedPolicies from API | Done | useEffect syncs apiAttachments → local state |
| On attach/detach call backend (useSetAgentPolicies with full list) | Done | persistAttachments() on each attach; PUT replaces full set (detach = remove from list and PUT) |
| Invalidate / refetch on success | Done | Mutation invalidates agent policies and agents list |

Flow layout (node positions) is optional per plan; not implemented.

---

## Step 9 — Optional

| Plan requirement | Status | Notes |
|------------------|--------|--------|
| Last activity | Not done | Optional |
| Agent-scoped logs / escalations / integrations | Not done | Optional; placeholder pages use useAgent() from context |

---

## File Checklist (from plan)

| File | Status |
|------|--------|
| backend/prisma/schema.prisma (agents, agent_policies) | Done |
| backend/prisma/migrations/..._add_agents_and_agent_policies/migration.sql | Done |
| Backend: app-agents module (controller, service, DTOs) | Done (path app-agents) |
| Backend: list, create, get by id, get by slug, update, delete | Done |
| Backend: agent-policies GET, PUT | Done |
| Frontend: /api/agents, /api/agents/[id], /api/agents/by-slug/[slug] | Done |
| Frontend: /api/agents/[id]/policies | Done |
| Frontend: lib/agents.ts (Agent, AgentPolicyAttachment) | Done |
| Frontend: use-agents.ts (useAgents, useAgentBySlug, useCreateAgent, etc.) | Done |
| Frontend: use-agent-policies.ts (useAgentPolicies, useSetAgentPolicies) | Done |
| Frontend: agents/page.tsx (useAgents, useCreateAgent) | Done |
| Frontend: [agentSlug]/layout.tsx (useAgentBySlug + id→slug redirect) | Done |
| Frontend: dashboard/page.tsx (useAgentPolicies, persist attach) | Done |
| Remove/reduce MOCK_AGENTS, getAgentBySlug from agents-mock.ts | Done |
| No static agent refs in detail pages | Done (all use API or context) |

---

## Intentional difference

- **Backend path:** Plan document says “GET /agents” etc. Implementation uses **GET /app-agents** (and so on) so existing LLM routes (`POST /agents/generate-policy`, `GET /agents/messages/:threadId`) stay unchanged. Frontend still exposes `/api/agents` and proxies to `/app-agents`; from the product perspective “agents” is the resource.

---

## Summary

The implementation matches the plan with:

1. **Database:** agents + agent_policies as specified; migration file present (run `npx prisma migrate dev` when DB is ready).
2. **Backend:** Full CRUD and agent-policies (GET + PUT replace) at `/app-agents` with Clerk auth and org scoping.
3. **Frontend API:** All listed routes implemented with Clerk auth and proxy to backend.
4. **Frontend hooks/types:** Agent, AgentPolicyAttachment, and all listed hooks.
5. **Pages:** List and create from API; layout resolves by slug with id→slug redirect; dashboard loads and saves attached policies.
6. **Cleanup:** Mock agent data and getters removed; Agent type and usage switched to lib/agents.

Optional items (last activity, flow layout, agent-scoped logs/escalations) are left for later as in the plan.
