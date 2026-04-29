---
title: "Freight Agreements"
description: "Create carrier agreements that define purchased transportation charges, validity, currency, and default charge lines."
---

# Freight Agreements

Use **Freight Agreements** to store the commercial terms your company has with carriers.

A freight agreement is the cost-side contract. It helps users apply expected carrier charges to Freight Orders and settlement.

## Before you start

Make sure that:

- the carrier exists,
- [Charges](charges.md) are configured,
- a Freight Agreement Type exists if you want default charge lines,
- currency and unit of measure setup is complete.

## How to create a freight agreement

1. Search for **Freight Agreements**.
2. Choose **New**.
3. Select the carrier.
4. Enter the agreement number and description.
5. Select the agreement type if default lines should be copied.
6. Fill currency, start date, end date, and **Active**.
7. Review the agreement lines.
8. Attach the carrier contract or rate sheet if your process requires it.

## Freight Agreement Types

Use **Freight Agreement Types** to define reusable charge templates.

When a type is selected on an agreement, its type lines can populate the agreement lines. This is useful when several carriers use the same cost structure.

## Fields that matter most

| Field | Why it matters |
|---|---|
| **Carrier No.** | Connects the agreement to the carrier. |
| **No.** | Identifies the carrier agreement. |
| **Type Code** | Copies or groups the expected charge structure. |
| **Currency Code** | Defines the agreement currency. |
| **Start Date / End Date** | Controls when the agreement should be used. |
| **Active** | Shows whether the agreement is available for operational use. |
| **Charge Code** | Defines each cost line on the agreement. |
| **Calc. Qty. as Distance / Weight** | Lets the charge quantity depend on distance or weight. |

## Good to know

- Freight Agreements support carrier cost control. They are different from [Forwarding Agreements](forwardingagreement.md), which support customer commercial context.
- Keep carrier agreements active only while they are valid.
- Use attachments for signed contracts and rate sheets.
- Do not delete old agreements that were used for historical cost review.

## Troubleshooting

| Problem | What to check |
|---|---|
| Agreement is not available | Check carrier, active flag, validity dates, and filters. |
| Default lines are missing | Check the Freight Agreement Type and its type lines. |
| Cost looks wrong | Review charge code, quantity, price, currency, and distance or weight calculation flags. |
| User cannot attach a rate sheet | Check attachment permissions and storage setup. |

## Related

- [Carriers](carrier.md)
- [Charges](charges.md)
- [Freight Order](freightorder.md)
- [Purchase Assignment](purchaseassignment.md)
- [Settlement](settlement.md)
