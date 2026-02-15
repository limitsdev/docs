# Overview Tab — How Each Image Should Look

Visual descriptions for each image so you (or a designer) can create or source them. Each section describes **composition**, **content**, **style**, and **purpose**.

---

## 1. **overview-hero.png** (Overview page — hero)

**Where it appears:** Right after the opening paragraph on the Overview page, before "Why Limits?"

**Purpose:** Communicate in one glance that Limits sits between "your app/agents" and "allow / block / escalate."

**How it should look:**

- **Layout:** Horizontal or wide rectangle. Left side: sources of requests (e.g. icons or labels for "API", "SDK", "Agent", "Webhook"). Center: a clear "Limits" or policy-layer block. Right side: three outcomes — "Allow", "Block", "Escalate" (with simple icons or badges).
- **Content:**
  - **Left:** 4–6 request sources (API call, SDK call, Agent action, Webhook, etc.) with small icons or pills.
  - **Center:** A single "Limits" or "Policy layer" box; optional small lock/shield icon to suggest governance.
  - **Right:** Three distinct outcomes: green/check for Allow, red/X for Block, yellow/clock for Escalate.
  - Arrows: requests flowing **into** Limits, then outcomes flowing **out** (or one arrow in, three arrows out).
- **Style:** Clean, minimal. Same tone as the docs: light background, dark text, simple shapes. No heavy 3D or gradients. Optional: match Limits brand (black/white/gray).
- **Dimensions:** Wide works best (e.g. 1200×500 px or 16:9). Will scale down in the doc.

**One-line brief:** "Diagram: request sources → Limits (policy layer) → Allow / Block / Escalate."

---

## 2. **limitsplatform.png** (Overview page — "You stay in control")

**Where it appears:** After the bullet list (Dashboard, Policies, Logs, Approvals), before the Info box.

**Purpose:** Show the real platform so users see where they manage policies and approvals.

**How it should look:**

- **Layout:** Single screenshot or a clean composite of 2–3 UI areas. Full-width in the doc; no tiny thumbnails.
- **Content (pick one approach):**
  - **Option A — One screenshot:** Limits app with sidebar visible: Dashboard (or Policies) in the main area, and sidebar showing "Dashboard", "Policies", "Logs", "Approvals", "API Keys". So "You stay in control" = this one screen.
  - **Option B — Composite:** Two or three cropped screens side by side: (1) Dashboard with counts/cards, (2) Policies list or policy editor, (3) Approvals queue or Logs. Labels underneath: "Dashboard", "Policies", "Approvals" (or "Logs").
- **Style:** Real product UI (app.limits.dev). Light theme is fine; avoid clutter (collapse sidebars if needed, hide sensitive data). Add a subtle border or shadow so it reads as "screenshot" not floating clip art.
- **Dimensions:** e.g. 1200×700 px (or 16:9). Enough height to show navigation + one main view.

**One-line brief:** "Screenshot(s) of Limits platform showing Dashboard, Policies, Logs, and Approvals so users see where they stay in control."

---

## 3. **overview-flow.png** (Overview page — optional)

**Where it appears:** After the "How it works" mermaid diagram, before "You define policies."

**Purpose:** Optional polished version of "request sources → Limits → outcome" if you want a custom graphic instead of (or in addition to) the mermaid diagram.

**How it should look:**

- **Layout:** Simple horizontal flow: Sources → Limits → Outcomes. Same idea as the mermaid but with custom art (icons, product-style boxes).
- **Content:** Match the mermaid: API, SDK, Agent, Webhook, Scheduled, Manual on the left; "Policy evaluation" / "Limits" in the middle; Allow / Block / Escalate on the right. Arrows between stages.
- **Style:** Same minimal, doc-style look as overview-hero (light bg, dark text, simple shapes). Can re-use the same icon set as overview-hero for consistency.
- **Dimensions:** Wide (e.g. 1000×350 px).

**One-line brief:** "Custom diagram of the same flow as the mermaid: sources → Limits → allow/block/escalate."

---

## 4. **hero-light.png** (index.mdx — Introduction hero)

**Where it appears:** Introduction page (index.mdx), right after: *"Limits intercepts every agentic action at the edge, centralizes and versions stateful policies, and replaces prompt-driven constraints with deterministic rules."* — before the first CardGroup.

**Purpose:** In one glance, show that Limits is the **AI policy layer**: it sits between your agents/app and the three outcomes (allow, block, escalate). Reinforce "edge," "policy layer," and "deterministic rules."

**How it should look:**

- **Layout:** Wide horizontal image (e.g. 1200×500 px or 16:9). Three clear zones left → center → right.
- **Left zone — Request sources:** Icons or small labels for where requests come from: e.g. **API**, **SDK**, **Agent**, **Webhook** (and optionally **Cron**, **Manual**). Simple icons (terminal, code bracket, bot, link). Can be pills, cards, or a small stack. Conveys: "every agentic action" / "your stack."
- **Center zone — Limits / policy layer:** One clear block with the word **"Limits"** or **"Policy layer"**. Optional: small shield or lock icon to suggest governance. Optional short line: "Edge · Deterministic rules." This is the central "intercepts at the edge" / "centralizes policies" moment.
- **Right zone — Outcomes:** Three distinct outcomes with icons and labels:
  - **Allow** — e.g. green check or "✓ Allow"
  - **Block** — e.g. red X or "✗ Block"
  - **Escalate** — e.g. yellow clock or "⏳ Escalate"
- **Flow:** Arrows from left (sources) **into** the center (Limits), then from center **out** to the three outcomes. Clean, minimal arrows (no decoration).
- **Style:** Light background (white or very light gray), dark text and icons. Minimal: flat shapes, no heavy 3D or gradients. Match Limits brand (black/white/gray). Sans-serif if any text. Feels like a product diagram, not a cartoon.
- **Dark mode:** If you use the same asset for dark mode, ensure contrast is good; or provide **hero-dark.png** with inverted colors (dark bg, light text/icons) and use it where the theme is dark.

**One-line brief:** "Wide diagram: your stack (API, SDK, Agent, Webhook) → Limits (policy layer) → Allow / Block / Escalate. Light, minimal, product-style."

---

## 5. **instructions-vs-conditions-modes.png** (Instructions vs Conditions page)

**Where it appears:** After the "Quick difference between the two modes" table, before "Conditions mode."

**Purpose:** Show in the real UI that a policy can be in **Conditions** mode or **Instructions** mode (two ways to define the same policy).

**How it should look:**

- **Layout:** One screenshot or two side-by-side. Full-width or two equal columns.
- **Content (pick one):**
  - **Option A — One screenshot:** Policy edit page with tabs or segments visible: one tab labeled "Conditions" (maybe with a rule like "Block when amount > 10000") and another "Instructions" (with natural-language text). User should see "I can pick one of these two."
  - **Option B — Two screenshots:** Left = policy in Conditions mode (condition builder, field/operator/value). Right = same policy in Instructions mode (text area with bullets or prose). Short labels above: "Conditions mode" and "Instructions mode."
- **Style:** Real Limits UI. Light theme, clean crop. Optional: subtle highlight or border around the active tab/mode so the difference is obvious.
- **Dimensions:** e.g. 1200×600 px (one wide screenshot) or 2× 600×500 px (two side by side).

**One-line brief:** "Screenshot(s) of the policy editor showing Conditions mode vs Instructions mode so users see the two ways to define a policy."

---

## 6. **conditions-builder.png** (Instructions vs Conditions — optional)

**Where it appears:** After the "Conditions mode" example, before "Instructions mode."

**Purpose:** Show what the condition builder looks like (field, operator, value).

**How it should look:**

- **Layout:** Cropped screenshot of the condition builder: one or two rules visible (e.g. "request.amount", "greater than", "5000" and "user.risk_level", "equals", "high").
- **Content:** Dropdowns or inputs for field path, operator, value. Optional: "Block" or "Escalate" or "Allow" label so it's clear this is a rule that leads to an outcome.
- **Style:** Real UI, minimal crop, no sensitive data.
- **Dimensions:** e.g. 700×350 px.

**One-line brief:** "Close-up of the condition builder UI: field, operator, value for one rule."

---

## 7. **instructions-editor.png** (Instructions vs Conditions — optional)

**Where it appears:** After the "Instructions mode" example, before "Summary."

**Purpose:** Show the instructions editor (natural-language text).

**How it should look:**

- **Layout:** Cropped screenshot of the instructions text area with 2–3 bullet points or short sentences (e.g. "Block any payment over $5,000.", "Escalate when risk level is high.").
- **Content:** Plain text / rich text area; no code. Should feel "natural language" not "condition builder."
- **Style:** Real UI, same product look as conditions-builder.
- **Dimensions:** e.g. 700×280 px.

**One-line brief:** "Close-up of the instructions editor with natural-language policy text."

---

## 8. **quickstart-terminal.png** (Quickstart — optional)

**Where it appears:** After "Install the SDK," before "Initialize the Client."

**Purpose:** Reassure users that the install step worked.

**How it should look:**

- **Layout:** Terminal/console window: one command (`npm install @limits/js` or `yarn add @limits/js`) and the success output (package name, version, "added 1 package" or similar).
- **Content:** No sensitive paths; generic project folder name (e.g. `my-app`). Optional: cursor on next line to suggest "ready for next step."
- **Style:** Common terminal look (dark background, green/white text, or your doc’s code-block style). Rounded window frame is optional.
- **Dimensions:** e.g. 800×220 px.

**One-line brief:** "Terminal showing npm/yarn install @limits/js and success message."

---

## 9. **quickstart-flow.png** (Quickstart — optional)

**Where it appears:** After "Complete Example," before "Next steps."

**Purpose:** Summarize the flow: app calls SDK → check → allow/block/escalate → proceed or stop.

**How it should look:**

- **Layout:** Simple horizontal flow: 3–4 steps. E.g. "Your app" → "limits.check()" → "Allow / Block / Escalate" → "Proceed or stop."
- **Content:** Very simple: app icon or "App", then "check()" or "Limits SDK", then three outcomes (icons or words), then "Proceed" vs "Stop" (or "Review" for escalate).
- **Style:** Minimal, same as overview diagrams (light, simple shapes). Can reuse outcome icons from overview-hero.
- **Dimensions:** e.g. 900×250 px.

**One-line brief:** "Simple flow: Your app → limits.check() → allow/block/escalate → proceed or stop."

---

## Summary table

| Image | Type | Main content | Size (suggested) |
|-------|------|--------------|------------------|
| **overview-hero.png** | Diagram | Sources → Limits → Allow/Block/Escalate | 1200×500 |
| **limitsplatform.png** | Screenshot | Dashboard + Policies + Logs + Approvals | 1200×700 |
| **overview-flow.png** | Diagram (optional) | Same flow as mermaid, custom art | 1000×350 |
| **hero-light.png** / **index-hero** | Hero | Same as overview-hero (optional dark variant) | 1200×500 |
| **instructions-vs-conditions-modes.png** | Screenshot | Conditions tab vs Instructions tab | 1200×600 or 2×600×500 |
| **conditions-builder.png** | Screenshot (optional) | Field, operator, value for one rule | 700×350 |
| **instructions-editor.png** | Screenshot (optional) | Natural-language instructions text | 700×280 |
| **quickstart-terminal.png** | Screenshot (optional) | Terminal: install + success | 800×220 |
| **quickstart-flow.png** | Diagram (optional) | App → check() → outcomes → proceed/stop | 900×250 |

Use this doc as the creative brief when designing or commissioning each image. If you tell me which ones you want to create first (e.g. only required, or hero + platform + instructions-vs-conditions), I can turn this into a short checklist or copy for a designer.
