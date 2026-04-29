---
title: "Purchase Assignment"
description: "Assign purchase invoice or credit memo lines to Freight Order charges and settlement costs."
---

# Purchase Assignment

Use **Purchase Assignment** to connect vendor purchase document lines to Freight Order charges and allocated Forwarding Order costs.

This is the finance workflow for matching carrier or vendor invoices to the transportation work they belong to.

## Before you start

Make sure that:

- the Freight Order exists,
- the carrier or pay-to vendor is correct,
- purchase invoices or credit memos exist for that vendor,
- [Charges](charges.md) are configured,
- settlement allows cost lines and allocation.

## How to assign purchase lines

1. Open the Freight Order or the related cost workflow.
2. Open **Purchase Document Assignment**.
3. Review available purchase document lines for the vendor.
4. Select the lines that belong to the Freight Order.
5. Choose the allocation option when prompted.
6. Confirm that freight charge lines and settlement cost lines are created or updated.
7. Review [Settlement](settlement.md) totals.

## What the page shows

| Field | Why it matters |
|---|---|
| **Document Type** | Shows whether the source is an invoice, order, or credit memo. |
| **Document No.** | Identifies the purchase document. |
| **Vendor Invoice No.** | Helps match the carrier invoice reference. |
| **Type / No.** | Shows the purchase line posting object. |
| **Description** | Explains the vendor cost line. |
| **Quantity / Direct Unit Cost / Line Amount** | Drives the assigned cost value. |
| **Currency Code** | Shows the purchase document currency. |

## Key actions

| Action | Use it for |
|---|---|
| **Charges Setup** | Open charge master data when a line needs mapping review. |
| **Delete Link To Freight Order** | Remove an existing assignment and related allocated cost lines. |

## Good to know

- Purchase Assignment is cost-side. It does not create customer invoices.
- A purchase line can be assigned only once when the system detects an existing allocation.
- Deleting a link can remove the freight charge line and allocated Forwarding Order cost line.
- Review settlement after every assignment.

## Troubleshooting

| Problem | What to check |
|---|---|
| Purchase line is not listed | Check vendor, document type, company, and filters. |
| Line is already assigned | Open the existing assignment or remove the link if it was wrong. |
| Cost does not appear in settlement | Check charge setup, allocation choice, and settlement recalculation. |
| Delete link is risky | Review the Freight Order and settlement cost line before confirming. |

## Related

- [Freight Order](freightorder.md)
- [Settlement](settlement.md)
- [Charges](charges.md)
- [Freight Agreements](freightagreement.md)
