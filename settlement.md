---
title: "Settlement"
description: "Manage income, costs, allocation, invoicing, credit memos, and profit for a Forwarding Order."
---

# Settlement

Use **Settlement** to control the financial result of a Forwarding Order.

Settlement brings together customer income, carrier and vendor costs, allocation, posted document links, and profit review.

## Before you start

Make sure that:

- services exist for customer billing,
- charges exist for carrier and vendor costs,
- [Freight Agreements](freightagreement.md) exist when carrier rates are maintained as agreements,
- customers, vendors, posting groups, tax setup, and currencies are ready,
- status controls allow settlement changes,
- posting controls are configured in [TMS Setup](setup.md).

## What lives in settlement

| Area | Use it for |
|---|---|
| **Income lines** | Billable services for the customer. |
| **Cost lines** | Carrier, vendor, or internal cost entries. |
| **Purchase assignment** | Links vendor purchase lines to Freight Order charges and settlement cost. |
| **Allocation** | Distribution of cost across the Forwarding Order or related lines. |
| **Sales documents** | Customer invoices and credit memos created from income lines. |
| **Purchase references** | Vendor cost support and carrier invoice references. |
| **Totals** | Income, cost, profit, margin, and currency values. |

## How to work in settlement

1. Open the Forwarding Order.
2. Open **Settlement**.
3. Add or calculate income lines.
4. Add or import cost lines.
5. Use [Purchase Assignment](purchaseassignment.md) when a vendor purchase line must be matched to a Freight Order charge.
6. Review currency, quantity, price, tax, and posting fields.
7. Allocate costs when required.
8. Create customer invoice or credit memo when the order is ready.
9. Review posted document links.
10. Confirm totals before posting the Forwarding Order.

## Key actions

| Action | Use it for |
|---|---|
| **Create Sales Invoice** | Create a customer invoice from income lines. |
| **Create Credit Memo** | Create a customer credit memo from negative income lines. |
| **Sync with Sales** | Update linked sales document lines from settlement. |
| **Allocate Costs** | Distribute carrier or vendor costs. |
| **Purchase Assignment** | Link vendor purchase document lines to Freight Order and settlement cost. |
| **Recalculate** | Refresh totals after line changes. |
| **Open Related Document** | Trace posted or unposted Business Central documents. |

## Fields that matter most

| Field | Why it matters |
|---|---|
| **Service Code** | Defines what is billed to the customer. |
| **Charge Code** | Defines the cost type. |
| **Customer / Vendor** | Controls document creation and posting context. |
| **Currency Code** | Controls financial values and reporting. |
| **Quantity / Unit Price / Amount** | Drives line value. |
| **Document Status** | Shows whether financial documents are created or posted. |
| **Profit and Margin** | Shows job result. |

## Posting controls

Forwarding Order posting can require settlement documents to be posted first.

If **Posted Documents Control** or **FWO Posting Date Control** is enabled in TMS Setup, users must complete financial document posting and date checks before the Forwarding Order can be posted.

## Good to know

- Settlement is where operations and finance meet. Review it before creating customer documents.
- Use services for income and charges for cost.
- Posted document links are part of the audit trail.
- Purchase Assignment is the safest way to match vendor invoices to Freight Order cost.
- Margin is only meaningful when income and cost lines are complete.

## Troubleshooting

| Problem | What to check |
|---|---|
| Invoice action is disabled | Check status rules, permissions, customer, and setup. |
| Invoice creation fails | Check service mapping, posting groups, tax setup, currency, and required fields. |
| Cost cannot be allocated | Check charge setup, cost line status, and allocation target. |
| Purchase line is already assigned | Open [Purchase Assignment](purchaseassignment.md) and review the existing link before deleting it. |
| Forwarding Order posting is blocked | Review posted document control and settlement posting date control. |
| Profit looks wrong | Recalculate settlement and review line amounts, currencies, and allocations. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Services](services.md)
- [Charges](charges.md)
- [Freight Agreements](freightagreement.md)
- [Purchase Assignment](purchaseassignment.md)
- [Pricing](pricing.md)
- [TMS Setup](setup.md)
