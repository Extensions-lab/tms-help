---
title: "Charges"
description: "Define carrier and vendor cost types used in settlement, Freight Orders, allocation, and purchase processing."
---

# Charges

Use **Charges** to define the cost types your company records for carrier, vendor, and operational expenses.

Charges are the cost side of settlement. Services are the customer billing side.

![Charges list](resources/charges/pics/charges1.png)

## How to open the list

Open the **Charges (TMS)** list from the navigation menu on your Role Center (home page):

- On a standard Role Center with the **TMS** menu, choose **TMS > Setup > Invoicing > Charges**.
- On the **TMS Dispatcher** Role Center, choose **Setup > Invoicing > Charges**.

You can also search for **Charges (TMS)**.

## Before you start

Make sure that:

- item charges or G/L accounts are available if costs are posted to Business Central financial documents,
- vendors and carriers are created,
- [Freight Agreements](freightagreement.md) are configured if charges are copied from carrier agreements,
- settlement rules are configured in [TMS Setup](setup.md),
- users understand which costs are estimated and which costs are actual.

## How to create a charge

1. Open the **Charges (TMS)** list using one of the options above.
2. Choose **New**.
3. Enter **No.** and **Description**.
4. Set **Purchase Line - Type** and **Purchase Line - No.** to the Business Central line type and record used for this cost.
5. Fill **Unit of Measure Code** and **Default Vendor No.** when the charge should supply these defaults.

## Fields that matter most

| Field | Why it matters |
|---|---|
| **No.** | The charge identifier selected in **Code** on settlement lines. |
| **Description** | Appears on cost lines, reports, and documents. |
| **Unit of Measure Code** | Supplies the default unit for the charge quantity. |
| **Default Vendor No.** | Supplies the default supplier for the cost. |
| **Purchase Line - Type** | Defines the Business Central purchase line type, such as **G/L Account**, **Charge (Item)**, or a service **Item**. |
| **Purchase Line - No.** | Identifies the account, item charge, or service item. The purchase line must use the matching type and number when its cost is imported. |
| **Default for Purchase Import** | Chooses this Charge when several Charges match the source **Purchase Line - Type / Purchase Line - No.** and TMS needs to choose a cost code automatically. It is enabled automatically when you configure the first mapping for that type and number. |

## Where charges are used

| Area | Use |
|---|---|
| **Freight Order** | Records expected or actual carrier cost. |
| **Freight Agreements** | Defines reusable carrier cost lines and calculation flags. |
| **Settlement** | Allocates cost to the Forwarding Order and calculates margin. |
| **Purchase Assignment** | Maps vendor purchase lines into Freight Order charges and settlement cost. |
| **Purchase processing** | Supports vendor invoice or cost document creation. |
| **Reporting** | Shows cost structure by charge type. |

## Choose a charge for purchase import

Several Charges can use the same Business Central item, item charge, or account. When importing a purchase line, TMS must choose which Charge its **Type / No.** represents.

When you select or change **Purchase Line - Type / Purchase Line - No.**, TMS enables **Default for Purchase Import** if the complete pair is not used by any other Charge in the company. A second Charge using the pair starts with its flag off, even if the existing Charge is not marked. Incomplete mappings also leave the flag off.

1. In **Charges (TMS)**, filter **Purchase Line - Type** and **Purchase Line - No.** to the source line's values.
2. Review **Default for Purchase Import**. Enable it on the Charge you want to prefer over other matches. Only one Charge may be marked for the same type and number in the company. Clear the old flag before choosing a different default. You may also leave all flags off to use the first matching Charge by **Code** in ascending order.
3. Confirm that the Charge is allowed by the Forwarding Order Type's **Allowed Service/Charges Set**, when configured.

If another matching Charge is already marked, TMS rejects enabling a second default and identifies the existing Charge. Clear that Charge's flag before marking the replacement; this also applies when another user has just selected the default.

You can clear the flag manually; saving the Charge or selecting the same mapping again preserves that choice. Changing the mapping to a different pair recalculates the flag: an unused complete pair enables it, while a pair used by another Charge or an incomplete pair clears it. This does not change flags on other Charges.

A single matching Charge is used even when the flag is off. With several matches, a marked default takes priority. If none is marked, TMS uses the first matching Charge by **Code** in ascending order, independently of the list's sorting, filters, or record creation order. This fallback does not enable a flag or change any Charge record. No matching Charge or multiple marked defaults stops automatic selection. The rule applies to imports of purchase orders, invoices, and credit memos, including posted invoices and credit memos, and automatic Charge selection for Freight Order assignment.

When [linking a purchase line to an existing Cost](purchaseassignment.md#link-one-purchase-line-to-an-existing-cost), a compatible **Code** already selected on that Cost takes priority over the default. If the Code is blank or does not match the source, TMS uses the automatic selection rule. Multi-FWO **Charge Allocation** continues to require compatible existing Cost codes; the default does not replace them.

Changing the default does not reclassify existing links or historical records. Automatic selection for the same type and number uses the default or first-code fallback, regardless of vendor or description. If the selected Charge is excluded by the FWO type's allowed set, import stops with an error; TMS does not try another matching Charge. Manual Charge selection and purchase creation from TMS remain available for all applicable Charges.

When upgrading from a version without this setting, existing Charges start with the flag off; TMS does not choose defaults for them automatically. Single mappings continue to work, and shared mappings use the first matching **Code** in ascending order without requiring setup changes. Mark a default when you want to choose a different Charge.

## Good to know

- Use short, stable charge codes. They become reporting dimensions for cost analysis.
- Keep service codes and charge codes separate. It helps users understand income versus cost.
- For an ordinary manual Settlement **Cost** line, use **Financial Effect = Standard**. Enter **Quantity** and **Price** as they should appear on the purchase document: with **Pricing Mode = Per Unit**, quantity `1` and price `700` produce **Amount = 700** and **P/L Amount = -700** before discounts.
- With **Pricing Mode = Fixed Amount**, enter the document amount before line discount, normally **Amount = 700** for a cost of `700`. Settlement applies the financial direction to P/L automatically. Deliberately negative quantities, prices, and amounts remain available. For supplier credits, use **Financial Effect = Credit** and review the [quantity, price, and amount signs](settlement.md#quantity-price-and-amount-signs).
- When a vendor invoice already exists, follow [Purchase Assignment](purchaseassignment.md) to import its cost. Check for a matching draft cost first so that the invoice does not duplicate an estimate.

## Troubleshooting

| Problem | What to check |
|---|---|
| Charge cannot be selected | Check the charge exists and is included in the Forwarding Order Type's allowed service/charge set when one is configured. |
| Cost does not post correctly | Review **Purchase Line - Type**, **Purchase Line - No.**, vendor, and Business Central posting setup. |
| Margin is wrong | Review **Financial Effect**, **Pricing Mode**, amount signs, currency, allocation, and duplicate costs. |
| Cost cannot be allocated | Check the settlement line status and linked Forwarding Order. |
| Purchase invoice line cannot be matched | Check charge setup and [Purchase Assignment](purchaseassignment.md). |
| Import selected a different Charge than expected | Mark the intended matching Charge as **Default for Purchase Import**. With all flags off, selection uses **Code** ascending, not the order shown in the list. A compatible Code already selected for single-line linking takes priority. |

## Related

- [Settlement](settlement.md)
- [Services](services.md)
- [Freight Agreements](freightagreement.md)
- [Purchase Assignment](purchaseassignment.md)
- [Freight Order](freightorder.md)
- [TMS Setup](setup.md)
