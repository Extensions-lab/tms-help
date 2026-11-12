---
title: "Pricing"
description: "Use pricing setup to create or suggest settlement income lines for customer billing."
---

# Pricing

Use **Pricing** when your company wants TMS to support repeatable customer billing rules.

Pricing helps users add settlement income lines with less manual calculation.

## Before you start

Make sure that:

- [Services](services.md) are configured,
- customers and forwarding agreements are available when used,
- price list behavior is enabled in [TMS Setup](setup.md) if your process requires it,
- users know when prices are automatic and when they must be reviewed manually.

## What pricing can depend on

| Factor | Typical use |
|---|---|
| **Customer** | Customer-specific commercial terms. |
| **Forwarding Agreement** | Contract or lane-specific terms. |
| **Service** | Which billable service is priced. |
| **Route or lane** | Different prices for different geography. |
| **Currency** | Pricing in the customer or contract currency. |
| **Quantity or weight** | Unit-based, weight-based, or volume-based price logic. |

## How users work with pricing

1. Open the Forwarding Order.
2. Confirm customer, agreement, route, cargo, and dates.
3. Open [Settlement](settlement.md).
4. Add or calculate income lines.
5. Review **Financial Effect**, **Pricing Mode**, **Unit of Measure Code**, quantity, price, amount, currency, and description.
6. For an editable manual line, adjust the input that controls its pricing mode: **Quantity** and **Price** for **Per Unit**, or **Amount** for **Fixed Amount**. Follow [Add an Income line](settlement.md#add-an-income-line).
7. For a normal customer invoice, confirm **Financial Effect = Standard** and fill **Unit of Measure Code** on every Income line, including **Fixed Amount** lines. Create the invoice when the order is ready.

## Pricing Mode in Settlement

Pricing rules can produce different settlement pricing modes. Review the resulting mode before changing a line.

| Pricing Mode | What controls the total |
|---|---|
| **Per Unit** | **Quantity × Price** determines **Amount** before line discount. Changing quantity changes the total. |
| **Fixed Amount** | **Amount** is the agreed document total before line discount. **Price** is derived from Amount and Quantity when quantity is nonzero. Changing quantity preserves Amount. |

Enter quantity, price, and amount as they should appear on the target document. An ordinary **Cost / Standard** line uses positive document values: quantity `1` and price `700` give **Amount = 700** and **P/L Amount = -700** before discounts. Settlement applies **Type** and **Financial Effect** to P/L automatically. Deliberately negative quantities, prices, and amounts remain available.

**Financial Effect** has a separate purpose. Use **Standard** for ordinary billing and **Credit** for a customer credit. Do not select a credit workflow just because a value is negative. See [Quantity, price, and amount signs](settlement.md#quantity-price-and-amount-signs).

## Pricing after splitting a stage

When one Forwarding Order stage is split across several Freight Orders, **Get Price** calculates each **Per Unit** service once for that stage and logistic unit type. Splitting the cargo between carriers or vehicles does not multiply the customer charge. Separate stages are still priced separately, even when they use the same Stage Code.

For example, 10 pallets at 10 per pallet produce a total of **100**, whether the stage is assigned to one Freight Order or split into loads of 5 + 5 or 2 + 3 + 5 pallets.

The calculation uses the Forwarding Order's total quantity, weight, chargeable weight, or volume for the relevant logistic unit type. **Rate Table** thresholds also use this total, so a split does not move each branch into a smaller rate band.

Split branches must have consistent stage codes and applicable pricing rules for **Per Unit** pricing. If these stage codes or pricing rules differ, **Get Price** stops and keeps the existing settlement lines. Review the stage codes, route details, and price list rules before trying again.

**Fixed Amount** prices retain their existing behavior: the applicable fixed charge is generated for each physical branch. Review these charges separately when splitting a stage.

## Good to know

- Pricing is a starting point for billing. Users still need to review settlement before invoicing.
- Keep pricing rules simple enough for users to understand.
- Use forwarding agreements when the same customer has multiple commercial arrangements.

## Troubleshooting

| Problem | What to check |
|---|---|
| No price is found | Check service, customer, agreement, currency, route, validity dates, and setup. |
| Wrong price is used | Review priority rules and whether the Forwarding Order has the expected agreement. |
| User cannot change price | Check status controls and settlement permissions. |
| Invoice total differs from expected value | Review **Pricing Mode**, **Financial Effect**, quantity, currency, tax, discounts, and rounding. |
| Quantity changes but the total stays the same | Check whether **Pricing Mode = Fixed Amount**. In this mode, quantity changes the derived price, not the agreed amount. |

## Related

- [Settlement](settlement.md)
- [Services](services.md)
- [Forwarding Agreements](forwardingagreement.md)
- [TMS Setup](setup.md)
