# Overview Tab — Images to Add

This guide lists **what images you need** and **where to put them** for the **Introduction** group (Overview tab) in the docs.

---

## 1. **overview.mdx** (Overview page)

| # | Where to put | Image filename (suggested) | What the image should show |
|---|--------------|-----------------------------|----------------------------|
| 1 | **After the opening paragraph** (before "Why Limits?") | `overview-hero.png` or use existing `hero-light.png` / `hero-dark.png` | **Hero/heroine:** Product shot or diagram showing Limits as a layer between "your app/agents" and "allow/block/escalate". Optional: reuse `hero-light.png` and `hero-dark.png` if they already show this. |
| 2 | **After "How it works"** (after the mermaid diagram, before "You define policies") | `overview-flow.png` (optional) | **Optional:** A polished version of the "request sources → Limits → outcome" flow (you already have a mermaid diagram; only add if you want a custom graphic). |
| 3 | **After "You stay in control"** (before "Policies do what you tell them") | `limitsplatform.png` | **Platform screenshot:** Dashboard or Policies + Approvals + Logs in one view (or a composite). Shows "Dashboard, Policies, Logs, Approvals" so users see where they stay in control. |
| 4 | **After "Policies do what you tell them"** (before "Next steps" cards) | (none required) | No image required; the examples are clear. |

**Summary for overview.mdx:**  
- **Required:** 1 hero (or reuse hero-light/hero-dark) + 1 platform screenshot (`limitsplatform.png`).  
- **Optional:** 1 flow diagram (`overview-flow.png`) if you want a custom graphic instead of/in addition to mermaid.

---

## 2. **index.mdx** (Introduction / Home)

| # | Where to put | Image filename (suggested) | What the image should show |
|---|--------------|-----------------------------|----------------------------|
| 1 | **After the opening sentence** (before the first CardGroup) | `index-hero.png` or reuse `hero-light.png` | **Hero:** Same as overview—Limits as the policy layer (edge, policies, deterministic rules). Can reuse `hero-light.png` / `hero-dark.png`. |
| 2 | **After "Key Features" CardGroup** | (none required) | Cards are enough; optional: one image that represents "deterministic + approvals + logs + edge" if you have a single graphic. |

**Summary for index.mdx:**  
- **Required:** 1 hero (reuse hero or add `index-hero.png`).  
- **Optional:** 1 “Key Features” visual if you have it.

---

## 3. **quickstart.mdx** (Quickstart)

| # | Where to put | Image filename (suggested) | What the image should show |
|---|--------------|-----------------------------|----------------------------|
| 1 | **After "Install the SDK"** (before "Initialize the Client") | `quickstart-terminal.png` (optional) | **Optional:** Terminal showing `npm install limit-js` and success output. |
| 2 | **After "Complete Example"** (before "Next steps") | `quickstart-flow.png` (optional) | **Optional:** Simple flow: "Your app → limits.check() → allow/block/escalate → proceed or stop." |

**Summary for quickstart.mdx:**  
- **Optional only:** Terminal screenshot and/or small “check → outcome” flow. No image is required for quickstart to work.

---

## 4. **policies/instructions-vs-conditions.mdx** (Instructions vs Conditions)

| # | Where to put | Image filename (suggested) | What the image should show |
|---|--------------|-----------------------------|----------------------------|
| 1 | **After "Quick difference between the two modes" table** (before "Conditions mode") | `instructions-vs-conditions-modes.png` | **Screenshot:** Policy edit UI showing **Conditions** tab vs **Instructions** tab (or two side-by-side screenshots). So users see "Conditions = structured rules" vs "Instructions = natural language" in the product. |
| 2 | **After "Conditions mode" example** (before "Instructions mode") | `conditions-builder.png` (optional) | **Optional:** Condition builder UI (field, operator, value) for one rule. |
| 3 | **After "Instructions mode" example** (before "Summary") | `instructions-editor.png` (optional) | **Optional:** Instructions editor with natural-language text. |

**Summary for instructions-vs-conditions.mdx:**  
- **Required:** 1 screenshot showing **Conditions vs Instructions** in the platform (`instructions-vs-conditions-modes.png`).  
- **Optional:** Condition builder screenshot, Instructions editor screenshot.

---

## File locations

- Put all images in: **`c:\limits\docs\images\`**
- In MDX, reference them as: **`/images/filename.png`**
- Use **hero-light.png** and **hero-dark.png** for dark-mode support where you use a hero image.

---

## Checklist (Overview tab only)

| Page | Required images | Optional images |
|------|-----------------|-----------------|
| **overview.mdx** | Hero (or reuse hero-light/hero-dark), `limitsplatform.png` | `overview-flow.png` |
| **index.mdx** | Hero (or reuse) | Key features visual |
| **quickstart.mdx** | — | `quickstart-terminal.png`, `quickstart-flow.png` |
| **instructions-vs-conditions.mdx** | `instructions-vs-conditions-modes.png` | `conditions-builder.png`, `instructions-editor.png` |

I can next add the actual `<img>` or Mintlify image tags in each MDX file so the paths are ready when you add the image files.
