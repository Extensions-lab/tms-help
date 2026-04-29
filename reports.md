---
title: "Reports and Documents"
description: "Print or email Forwarding Order, Freight Order, settlement, cargo, and carrier documents."
---

# Reports and Documents

Use **Reports and Documents** to produce customer, carrier, cargo, and internal documents from TMS.

Documents should be printed only after the required order, stage, carrier, cargo, and settlement data has been reviewed.

## Before you start

Make sure that:

- report selections are configured in [TMS Setup](setup.md),
- customer and carrier contact details are filled,
- Freight Orders contain carrier, route, and cargo data,
- stage entries are reviewed when reports include execution event details,
- settlement lines are reviewed before financial documents are created.

## Common documents

| Document | Use it for |
|---|---|
| **Forwarding Order Confirmation** | Customer-facing confirmation of the transportation job. |
| **Freight Order** | Carrier instruction for execution work. |
| **Freight Order Details** | Detailed cargo and stage information. |
| **Load Summary** | Compact overview of content assigned to a Freight Order. |
| **Packing List** | Cargo packing and handling information. |
| **Bill of Lading** | Transport handover documentation when required. |
| **Booking Confirmation** | Carrier or customer booking confirmation. |
| **Settlement Summary** | Internal review of income, cost, and profit. |

## How to print or email documents

1. Open the Forwarding Order or Freight Order.
2. Review required fields and lines.
3. Choose the document action.
4. Preview the document when the output is new or changed.
5. Print, download, or email according to your process.
6. Attach the final file to the order when document control requires it.

## Good to know

- Report output depends on the data available on the source document.
- Email actions depend on Business Central email setup.
- If the wrong report opens, run **Set default reports** in [TMS Setup](setup.md) or review report selections.

## Troubleshooting

| Problem | What to check |
|---|---|
| Report action is missing | Check permissions, document status, and page actions. |
| Report prints blank sections | Review carrier, party, cargo, stage, and settlement data. |
| Email does not send | Check Business Central email setup and recipient address. |
| Wrong report layout is used | Review report selections and custom report extensions. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Stage Entries](stageentries.md)
- [Settlement](settlement.md)
- [TMS Setup](setup.md)
