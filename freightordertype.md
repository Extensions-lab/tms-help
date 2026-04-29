---
title: "Freight Order Types"
description: "Configure Freight Order defaults for status profile, default status, charge calculation, cost distribution, and number series."
---

# Freight Order Types

Use **Freight Order Types** to control how Freight Orders are created and managed.

A type defines numbering, status behavior, and cost behavior for carrier execution documents.

## Before you start

Make sure that:

- a status profile exists for Freight Orders,
- number series are configured,
- charge and cost distribution rules are agreed with finance,
- [TMS Setup](setup.md) points to the correct default Freight Order Type.

## How to create a Freight Order Type

1. Search for **Freight Order Types**.
2. Choose **New**.
3. Enter a code and description.
4. Select the **Default Status Profile**.
5. Confirm the **Default Status Code**.
6. Turn on charge calculation or cost distribution when your process uses it.
7. Select the **No. Series**.
8. Save the type and test it by creating a Freight Order.

## Fields that matter most

| Field | Why it matters |
|---|---|
| **Code** | Identifies the type users and setup records select. |
| **Description** | Helps users choose the correct execution type. |
| **Default Status Profile** | Controls Freight Order workflow and allowed actions. |
| **Default Status Code** | Sets the first status on new Freight Orders. |
| **Enable Charge Calculation** | Allows calculated charges for Freight Orders of this type. |
| **Enable Cost Distribution** | Allows cost distribution across related lines or documents. |
| **No. Series** | Assigns Freight Order numbers. |

## Good to know

- Keep the number of Freight Order Types small.
- Use different types only when workflow, numbering, or cost behavior is truly different.
- A missing number series can block Freight Order creation.
- A wrong status profile can make actions disappear from the Freight Order.

## Troubleshooting

| Problem | What to check |
|---|---|
| New Freight Order has no number | Check **No. Series** on the type and number series setup. |
| Wrong status appears | Check **Default Status Profile** and **Default Status Code**. |
| Charge calculation does not run | Check **Enable Charge Calculation** and charge setup. |
| Cost distribution is unavailable | Check **Enable Cost Distribution**, permissions, and settlement setup. |

## Related

- [Freight Order](freightorder.md)
- [Statuses and Status Profiles](statuses.md)
- [Charges](charges.md)
- [Settlement](settlement.md)
- [TMS Setup](setup.md)
