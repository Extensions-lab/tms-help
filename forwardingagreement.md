---
title: "Forwarding Agreements"
description: "Use customer agreements to store commercial context for Forwarding Orders and settlement."
---

# Forwarding Agreements

Use **Forwarding Agreements** to store customer commercial agreements that support Forwarding Order and settlement work.

An agreement can help users identify the contract, customer relationship, pricing context, or operational rules connected to a transportation job.

Use [Freight Agreements](freightagreement.md) for carrier-side commercial terms.

## Before you start

Make sure that:

- customer records exist,
- services and price records are configured if the agreement affects billing,
- users know which agreement should be selected for each customer or lane.

## How to work with agreements

1. Search for **Forwarding Agreements**.
2. Create a new agreement.
3. Fill customer, description, validity, and reference details.
4. Link pricing or service rules when your process uses them.
5. Select the agreement on the Forwarding Order when it applies.

## Fields that matter most

| Field | Why it matters |
|---|---|
| **Code / No.** | Identifies the agreement. |
| **Customer No.** | Connects the agreement to the ordering party. |
| **Description** | Helps users select the correct agreement. |
| **Starting Date / Ending Date** | Controls validity. |
| **Reference** | Stores contract or customer reference information. |

## Good to know

- Use agreements to give users business context. Do not overload them with one-time notes.
- Expired agreements should remain available for history.
- Pricing still depends on the setup and settlement logic used by your company.
- Forwarding Agreements are customer commercial context. Freight Agreements are carrier-side cost context.

## Troubleshooting

| Problem | What to check |
|---|---|
| Agreement is not available | Check customer, validity dates, and filters. |
| Wrong prices are used | Review pricing setup, services, and the agreement selected on the order. |
| Users choose the wrong agreement | Improve descriptions and retire obsolete agreements by ending date. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Pricing](pricing.md)
- [Freight Agreements](freightagreement.md)
- [Services](services.md)
- [Settlement](settlement.md)
