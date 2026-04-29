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
5. Review price, currency, quantity, and description.
6. Adjust only when the status and company policy allow it.
7. Create the customer invoice when the order is ready.

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
| Invoice total differs from expected value | Review settlement quantity, currency, tax, discounts, and rounding. |

## Related

- [Settlement](settlement.md)
- [Services](services.md)
- [Forwarding Agreements](forwardingagreement.md)
- [TMS Setup](setup.md)
