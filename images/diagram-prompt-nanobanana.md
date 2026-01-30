# Prompt for Limits overview diagram (use with Nanobanana or similar)

Use this prompt to generate a diagram image in the style of [Buster’s overview](https://docs.buster.so/overview): clean, minimal flow with two main sections and a feedback loop. Use **Limits brand colors** only.

---

## Image prompt (copy-paste)

```
Minimalist flow diagram for a documentation page. Clean, flat design, no 3D or shadows.

**Layout:**
- Two main sections side by side, connected by a horizontal double-headed arrow.
- Left section: One tall rounded rectangle (light grey #E5E5E5 or #F0F0F0). Title at top: "Request sources:". Below it, a vertical bullet list:
  - API call
  - SDK call
  - Agent action
  - Webhook
  - Scheduled / Cron
  - Manual
- Right section: One wider rounded rectangle (same light grey). Inside it, two smaller rounded boxes side by side:
  - Left box: "Policy evaluation"
  - Right box: "Outcome"
  - A circular feedback loop between them: arrow from "Policy evaluation" to "Outcome" labeled "request", arrow from "Outcome" back to "Policy evaluation" labeled "allow / block / escalate".
- Thick double-headed arrow (black #000000) connecting the right edge of the left section to the left edge of the right section.

**Colors (Limits brand – use only these):**
- Box fill: light grey #E5E5E5 or #F0F0F0 (so it works on white background).
- All text: black #000000.
- Arrows and borders: black #000000.
- Optional accent for arrow labels or key text: dark grey #1F1F23.
- Background: pure white #FFFFFF.

**Style:** Simple, professional, like Buster docs overview diagram. Rounded corners on all boxes. No gradients, no icons, no decorative elements. Clear sans-serif typography. Diagram should read left-to-right: requests come from sources into Limits, which evaluates and returns an outcome.
```

---

## Alternative: dark theme version

If you need a version for dark mode (background #1F1F23 or #000000):

```
Same diagram as above, but:
- Background: #1F1F23 (dark grey) or #000000 (black).
- Box fill: #2D2D2D or #3A3A3A (slightly lighter grey).
- All text and arrows: #FFFFFF (white).
- Optional: arrow labels in #A0A0A0 for softer contrast.
```

---

## After you generate the image

1. Save the image (e.g. `overview-flow.png`) in `docs/images/`.
2. In `overview.mdx`, you can replace or supplement the Mermaid block with:

```mdx
<img src="/images/overview-flow.png" alt="Request sources connect to Limits policy layer; policy evaluation and outcome form a loop" />
```

3. For responsive images, use the path that Mintlify expects (e.g. `/images/overview-flow.png` from the docs root).
