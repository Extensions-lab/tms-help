---
title: "AI and Copilot Features"
description: "Use AI-assisted drafting to create or enrich Forwarding Order data from natural language input."
---

# AI and Copilot Features

Use AI-assisted features to speed up Forwarding Order drafting when users receive transportation requests as text.

AI can help turn unstructured request text into a draft. Users remain responsible for reviewing and correcting the result.

## Before you start

Make sure that:

- AI or Copilot setup is enabled in the environment,
- the user has permission to use the feature,
- Forwarding Order setup, statuses, and stages are configured,
- your company policy allows AI-assisted processing of customer request text.

## How to draft a Forwarding Order with AI

1. Open the AI draft action from the Forwarding Order area.
2. Paste or enter the customer request text.
3. Start draft generation.
4. Review extracted parties, dates, route, cargo, and references.
5. Correct missing or uncertain values.
6. Create or update the Forwarding Order only after review.

## What to review carefully

| Area | Why it matters |
|---|---|
| **Ordering party** | Drives billing and commercial responsibility. |
| **shipper and consignee** | Drive origin and destination details. |
| **Dates and times** | Drive planning and commitments. |
| **Cargo description** | Affects content, weight, volume, and documents. |
| **References** | Help users trace the customer request. |
| **Services and charges** | Affect financial result. |

## Good to know

- AI output should be treated as a draft, not as approved operational data.
- Users should keep customer request text in the document only when company policy allows it.
- AI is most useful for first-pass data entry. It does not replace status, posting, or finance controls.

## Troubleshooting

| Problem | What to check |
|---|---|
| AI action is not visible | Check environment setup, feature enablement, permissions, and license. |
| Draft misses important values | Improve the request text and review unsupported formats. |
| Created order has wrong defaults | Review Forwarding Order Type, stage profile, and setup defaults. |
| User is unsure about a result | Do not post or invoice until a business user has reviewed the draft. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Forwarding Order Types](forwardingordertype.md)
- [TMS Setup](setup.md)
