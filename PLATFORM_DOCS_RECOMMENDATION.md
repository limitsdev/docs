# Platform Documentation Integration Recommendation

## Analysis Summary

The `USER_DOCUMENTATION.md` file contains comprehensive platform/UI documentation that complements the existing SDK documentation. The current docs focus on **developer integration** (SDK), while the user docs focus on **platform usage** (web UI).

## Recommended Structure

### New Section: "Platform Guide" or "User Guide"

Add a new tab/section in `docs.json` for platform documentation, separate from SDK docs.

## Priority Sections to Include

### High Priority (Essential for users)

1. **Getting Started with Platform**
   - Account setup and authentication
   - Navigation overview
   - First steps guide
   - *Source: Lines 24-100*

2. **Dashboard**
   - Overview statistics explanation
   - Charts and visualizations
   - Real-time updates
   - *Source: Lines 102-148*

3. **Policy Management (UI)**
   - Creating policies via AI Assistant
   - Policy editing (Conditions vs Instructions mode)
   - Policy structure and concepts
   - Policy deployment
   - *Source: Lines 151-269*
   - *Note: Different from SDK policies - this is UI-focused*

4. **AI Assistant**
   - Natural language to policy conversion
   - Two creation modes (Instructions vs Conditions)
   - Chat interface for refinement
   - *Source: Lines 272-346*

5. **Integrations**
   - Slack setup and configuration
   - Webhook configuration
   - *Source: Lines 726-787*

6. **Approvals (Human Review)**
   - Viewing pending approvals
   - Approving/declining requests
   - Approval workflow
   - *Source: Lines 790-857*
   - *Note: Complements SDK human-approvals.mdx*

### Medium Priority (Important but can be condensed)

7. **Policy Conditions & Logic**
   - Operators reference
   - Field paths
   - CEL expressions
   - *Source: Lines 349-426*
   - *Note: Can be condensed into Policy Management*

8. **Policy Locks**
   - Limit locks (count/budget)
   - Time window locks
   - Velocity locks
   - *Source: Lines 429-504*
   - *Note: Can be part of Policy Management*

9. **Notifications**
   - Slack notifications
   - Webhook notifications
   - Per-action configuration
   - *Source: Lines 507-568*
   - *Note: Can be merged with Integrations*

10. **Policy Simulation & Testing**
    - Testing with sample payloads
    - Evaluation trace
    - *Source: Lines 571-639*
    - *Note: Important for users to test before deployment*

11. **Logs**
    - Viewing logs
    - Filtering (date range, action type)
    - Log details
    - *Source: Lines 642-723*

12. **API Keys**
    - Generating keys
    - Managing keys
    - Security best practices
    - *Source: Lines 860-922*

### Lower Priority (Can be simplified or moved)

13. **Billing & Subscriptions**
    - Plan overview
    - Upgrading/downgrading
    - *Source: Lines 925-993*
    - *Note: May be better in a separate "Account" section or support docs*

14. **Authentication & User Management**
    - Sign up/sign in
    - Profile management
    - *Source: Lines 997-1045*
    - *Note: Can be simplified to just sign up/sign in basics*

15. **Advanced Features**
    - Policy validation
    - CEL advanced usage
    - Error handling
    - *Source: Lines 1048-1134*
    - *Note: Can be merged into relevant sections*

16. **Glossary**
    - Key terms definitions
    - *Source: Lines 1137-1170*
    - *Note: Can be a separate glossary page or inline definitions*

## Recommended File Structure

```
docs/
├── platform/
│   ├── getting-started.mdx          # Account setup, navigation, first steps
│   ├── dashboard.mdx                 # Dashboard overview
│   ├── policies.mdx                  # Policy management UI (create, edit, deploy)
│   ├── ai-assistant.mdx             # AI Assistant usage
│   ├── policy-conditions.mdx        # Conditions, operators, CEL (condensed)
│   ├── policy-locks.mdx             # Locks (condensed)
│   ├── notifications.mdx             # Notifications (merged with integrations)
│   ├── simulation.mdx                # Policy testing
│   ├── logs.mdx                      # Viewing and filtering logs
│   ├── integrations.mdx              # Slack, webhooks setup
│   ├── approvals.mdx                 # Human approval workflow (UI)
│   ├── api-keys.mdx                 # API key management
│   └── glossary.mdx                 # Terms and definitions
```

## Content Adaptation Notes

1. **Convert to Mintlify MDX format**
   - Add frontmatter with title/description
   - Use Mintlify components (Card, CardGroup, CodeGroup, etc.)
   - Add proper code blocks with syntax highlighting
   - Use proper heading hierarchy

2. **Remove redundancy**
   - Some sections overlap (e.g., notifications appear in both Integrations and Policy Management)
   - Consolidate related topics

3. **Add visual elements**
   - Screenshots or diagrams where helpful
   - Code examples for webhook payloads
   - Step-by-step guides with visual indicators

4. **Cross-reference with SDK docs**
   - Link between platform docs and SDK docs where relevant
   - Example: Platform "Approvals" links to SDK "Human Approvals"

5. **Simplify technical details**
   - The user docs are very detailed - some can be condensed
   - Focus on "how to use" rather than "how it works internally"

## Suggested docs.json Update

Add a new tab for Platform documentation:

```json
{
  "tab": "Platform Guide",
  "icon": "layout",
  "groups": [
    {
      "group": "Getting Started",
      "pages": [
        "platform/getting-started"
      ]
    },
    {
      "group": "Core Features",
      "pages": [
        "platform/dashboard",
        "platform/policies",
        "platform/ai-assistant"
      ]
    },
    {
      "group": "Policy Configuration",
      "pages": [
        "platform/policy-conditions",
        "platform/policy-locks",
        "platform/simulation"
      ]
    },
    {
      "group": "Workflows",
      "pages": [
        "platform/approvals",
        "platform/logs"
      ]
    },
    {
      "group": "Integrations",
      "pages": [
        "platform/integrations",
        "platform/notifications",
        "platform/api-keys"
      ]
    }
  ]
}
```

## Next Steps

1. Create the `platform/` directory
2. Start with high-priority sections
3. Convert content to Mintlify MDX format
4. Add cross-references between platform and SDK docs
5. Test navigation and user flow
