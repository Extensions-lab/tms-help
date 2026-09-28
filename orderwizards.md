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

- use **Create Freight Order** on the Forwarding Order card to create one new Freight Order for several unassigned stages,
- create a new Freight Order from a Forwarding Order stage,
- create several Freight Orders from selected content or stages,
- review stage selection before creating carrier execution,
- run an optimization wizard when route or sequence data is available.

### Create Freight Order

The **Create Freight Order** wizard always creates a new Freight Order.

1. Review the introduction.
2. Select one or more unassigned stages. All available stages are selected by default.
   Stages with blocking problems are highlighted in red. A multiline problem summary under the list explains what must be corrected before Freight Order creation.
   Vehicle Unit Type, Transport Regulation, and Transport Profile can be corrected directly in the temporary stage list. These corrections are written to the source FWO stages only when **OK** creates the Freight Order; closing the wizard first discards them.
   Stage Priority is not shown or edited: the wizard follows the immutable Forwarding Order stage sequence.
3. Review Mode of Transport, Package Type, Transport Regulation, Stage Vehicle Unit Type, Transport Profile, and the capacity limits of every selected stage.
4. Review the Carrier and select the summary Vehicle Unit Type stored on the Freight Order header. The Pay-to Vendor is applied automatically from the selected stages and can be changed later on the created Freight Order.
5. Choose **OK** to create the Freight Order and assign every selected stage in full.

The wizard assigns the same complete cargo set to every selected stage. It does not split weight, volume, footage, logistic units, or unit types.
The created Freight Order stage priorities are assigned as 10, 20, 30, and so on in Forwarding Order line order.

When exactly one stage is selected, the Freight Order Header Vehicle Unit Type is required and defaults from that stage. When multiple stages are selected, the Header Vehicle Unit Type is forced to blank; each Stage Vehicle Unit Type remains authoritative.

The new Freight Order inherits its initial loading and unloading dates from the stages assigned by the wizard, as described below.

### Assign Freight Order

Use **Assign Freight Order** to distribute a stage's selected cargo to an existing Freight Order or create a new Freight Order for that assignment.

In step 4, **Create New Freight Order** is off by default, so you can select an existing Freight Order.

To create an order within the wizard:

1. Turn on **Create New Freight Order**.
2. Review **Carrier No.** and **Vehicle Unit Type Code**, which default from the selected stage. Carrier is optional; Vehicle Unit Type Code is required. The Pay-to Vendor is transferred automatically and can be changed later on the Freight Order.
3. Continue through the wizard and choose **Assign**. The new Freight Order is created at this point, and its initial planning dates come from the assigned stage.

Selecting an existing Freight Order in step 4 leaves its dates unchanged until you choose **Assign**. After successful assignment, the wizard recalculates loading and unloading dates for either an existing or a new Freight Order using all stages assigned to it. This includes previously assigned stages and newly assigned stages, even when they belong to different Forwarding Orders.

Manual card creation, including **New** in the Freight Order lookup, uses normal card defaults. If you later select that order and complete **Assign**, its planning dates are recalculated by the same rules.

### Planning dates from assigned stages

The **Create Freight Order** wizard and the batch wizard's **Create New Freight Order** mode fill the new order's planning dates from its assigned stages. **Assign Freight Order** applies the same calculation to both new and existing orders after assignment:

- **Loading Date Time** uses the earliest nonblank start date and time among all stages assigned to that Freight Order.
- **Unloading Date Time** uses the latest nonblank end date and time among all those stages.

Each available boundary replaces the corresponding date and time on the Freight Order, even if it was entered manually. The calculation uses stage dates only; previous header dates do not participate in the minimum or maximum. Stages outside this Freight Order do not affect the result.

The two boundaries are handled independently. If no assigned stage has a start date, the current Loading Date Time is retained; if none has an end date, the current Unloading Date Time is retained. For a new order, this preserves the corresponding creation default.

The separate Date and Time fields and each Window End synchronize automatically. Loading Window and Unloading Window durations remain unchanged.

The batch wizard's mode for adding to an existing Freight Order continues to preserve that order's planning dates.

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
