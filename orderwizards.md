---
title: "Order Wizards"
description: "Use guided pages to create Forwarding Orders and Freight Orders with the required type, parties, stages, and content."
---

# Order Wizards

Use **Order Wizards** when users need a guided way to create Forwarding Orders or Freight Orders.

Wizards help new users enter the required values in the right order and reduce missing setup errors.

## Before you start

Make sure that:

- [Forwarding Order Types](forwardingordertype.md) are configured,
- [Freight Order Types](freightordertype.md) are configured,
- number series exist,
- default party, status, and stage setup is ready,
- users know which order type to choose.

## Forwarding Order Wizard

Use the **New Forwarding Order Wizard** to create a Forwarding Order step by step.

Typical flow:

1. Select the Forwarding Order Type.
2. Confirm or enter the Forwarding Order number.
3. Fill the ordering party.
4. Fill origin party details.
5. Fill consignee details.
6. Create and open the new Forwarding Order.

## Freight Order Wizards

Use Freight Order wizards to create or optimize Freight Orders from stages, content, unit types, or selected operational data.

Typical use:

- create a new Freight Order from a Forwarding Order stage,
- create several Freight Orders from selected content or stages,
- review stage selection before creating carrier execution,
- run an optimization wizard when route or sequence data is available.

## Good to know

- A wizard creates or updates normal TMS documents. After creation, users continue work on the regular card pages.
- If a wizard stops, the most common cause is missing type setup or number series setup.
- Wizard defaults come from setup and document types.
- Use wizards for onboarding and controlled entry. Experienced users can still work directly on document pages when allowed.

## Troubleshooting

| Problem | What to check |
|---|---|
| Wizard cannot continue | Check required type, number series, and mandatory party fields. |
| Created order has wrong defaults | Review Forwarding Order Type, Freight Order Type, and TMS Setup. |
| Freight Order is not created | Check stage selection, status controls, and content assignment. |
| Open button is not available | The wizard may not have created the document yet or was opened in a restricted flow. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Forwarding Order Types](forwardingordertype.md)
- [Freight Order](freightorder.md)
- [Freight Order Types](freightordertype.md)
- [Stages](stages.md)
