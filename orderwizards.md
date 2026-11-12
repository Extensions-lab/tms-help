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
2. Confirm or enter the Forwarding Order number, then choose **Next** to continue entering the order details.
3. Fill the ordering party.
4. Fill origin party details.
5. Fill consignee details.
6. Create and open the new Forwarding Order.

## Copy a Forwarding Order

Start **Copy** from the target Forwarding Order card to open the three-step **Copy Forwarding Order** wizard. It fills the current order from an existing order; it does not create another order number. The card is saved before the wizard opens and refreshed when it closes. The first step opens directly with **Source**, without a separate target section.

1. **Select the Source.** Choose **Forwarding Order** or **Posted Forwarding Order** in **Copy From**, then select the source. The posted lookup selects an exact order and **Posted Version**, which is shown in the wizard. An active order cannot be copied into itself; an archived version with the same number is allowed.
2. **What to Copy.** Use one block of independent switches for **Ordering Party**, **Shipper**, **Consignee**, **Content Lines**, **Instructions**, **Income Settlement Lines**, and **Cost Settlement Lines**. All are on by default except Cost Settlement Lines. Unchecked sections are cleared in the target order; its previous values are not preserved. Other header details and route stages are copied.
3. **Review.** This step contains one **Warnings and Notes** field. Read the information, use **Back** to change the options, or choose **Copy** to apply them. If there are no warnings, the same field confirms that. Back and Next do not copy data; closing the wizard before Copy leaves the target's contents unchanged.

On the first step, four independent search switches can be combined:

| Search switch | Match with the target order |
|---|---|
| **Same FWO Type** | Forwarding Order Type Code. |
| **Same Ordering Party** | Ordering Party type and number. |
| **Same Shipper** | Shipper type and number. |
| **Same Consignee** | Consignee type and number. |

On first use, a search switch is enabled when its corresponding target type or party number is filled. You can turn any switch off, including Same FWO Type. The wizard remembers all four search switches and all seven copy options separately for each user and company when it closes, including closing without Copy. On later openings, your saved choices replace the initial defaults. The source kind, source order, posted version, and wizard step are not saved; choose the source for each new copy. Changing the source kind or a search switch clears the selected source so that it can be selected with the new filters. Active order searches also respect the user's existing Responsibility Center filter.

Warnings are shown **before** copying. With Content Lines on, they identify blocked Items or Products and explain an empty cargo source; blocked cargo is still copied with its descriptions, quantities, and measurement data. With Content Lines off, the review explains that the target will have no content lines. Other notes cover excluded parties or instructions, route stages retained without copied parties, empty stages, and parallel stage consolidation. No extra confirmation dialog is required. The source and options are checked again at Copy; if the reviewed information has changed, the review is refreshed before copying can continue. Invalid stage combinations or a required Settlement amount-model conversion stop the operation before replacing the target.

If the **target** already contains Settlement linked to documents or Freight Orders, or other Settlement that cannot be deleted, the Review step lists the affected lines and their reasons. **Copy is disabled**. Use another target order or resolve its Settlement links and restrictions first. This check includes all existing target Settlement even when Income or Cost copying is off, because those switches do not preserve the target's old lines. The checks run again before any target content is replaced.

The options have the following effects:

| Option | When turned off |
|---|---|
| **Ordering Party** | Clears the party details, linked agreement, currency, salesperson, responsibility center, and party system ID. Copied Income Settlement has no customer until one is selected. |
| **Shipper** | Clears the shipper details and loading address, contact, map location, VAT number, and notes. |
| **Consignee** | Clears the consignee details and unloading address, contact, map location, VAT number, and notes. |
| **Content Lines** | Removes the target's old cargo and copies no source cargo lines. |
| **Instructions** | Removes the target's old instructions and copies no source instructions. |
| **Income Settlement Lines** | Copies no Income Settlement. |
| **Cost Settlement Lines** | Copies no Cost Settlement. |

Route stage From/To locations remain a separate copied route template when Shipper or Consignee is off. Review those locations before planning the new job. Shipment references and other shared header values retain their normal copy behavior.

The target keeps its own order number, number series, creation date and creator, and status. Its actual start and end dates are cleared. The modification date and user reflect the copy operation, and the display style follows the target's retained status.

Both **Profit % (LCY)** and **Profit % (ACY)** are reset to 0 when copying from either an active or a posted Forwarding Order. The source's percentage may no longer match the Settlement selected for the new job. After reviewing the copied Settlement, choose **Profit Calculation** in the active Forwarding Order list to calculate and save the target's percentages.

Shipment references, including external tracking, waybill, bill of lading, consolidation, and flight numbers, are retained. When Content Lines is on, cargo descriptions and selected logistic units are retained for reuse.

Each copied stage is non-parallel and has no Freight Order link. When one source stage is split across several Freight Orders, its branches become one stage in the new order. Separate source stage lines remain separate. Planned details are taken from the branch with the first Freight Order number in the source sort order; old cargo reservations, execution status, actual date/time, vehicle, and driver assignments are cleared. Stage priority and simultaneous loading are reset for the new planning process. Stage order is established when the copied stages are assigned to a Freight Order. If branches of the same stage have different routes or transport requirements, copying stops before replacing the target order. These rules apply to both active and posted sources.

On the second step, choose which Settlement lines to copy from the selected active or posted order. **Income Settlement Lines** is on by default; **Cost Settlement Lines** is off by default. The options work independently:

| Income Settlement Lines | Cost Settlement Lines | Settlement copied from the source |
|---|---|---|
| On | Off | Income only (default). |
| Off | On | Cost only. |
| On | On | Income and Cost. |
| Off | Off | No Settlement lines. |

The wizard still replaces the target order's existing Settlement for every combination. Turning both options off leaves the target without Settlement lines after copying; it does not preserve the target's old lines.

Each copied Settlement line becomes a new editable, unlinked manual draft. The source can contain linked, posted, generated, Legacy, or Needs Review lines; copying does not change or verify the original lines. The selected source Settlement lines must already use the current amount model (version 2). If copying reports an **Amount Model Version** error, complete the Settlement amount-model conversion before trying again.

The new drafts retain the line Type, service or charge, description, Planned Document Type, quantity, unit of measure, currency, exchange-rate date and factor, Location Code, dimensions, and distance/weight calculation options. Pricing and discounts follow these rules:

- **Per Unit** retains the stored Price, including its sign, and recalculates Amount from quantity and price. If those values do not reproduce the source Amount at the currency's rounding precision, the draft uses **Fixed Amount** to preserve the source Amount.
- **Fixed Amount** retains Amount before line discount and derives Price using the copied quantity. A zero quantity is supported.
- Line Discount % and Line Discount Amount are retained for ordinary lines. If the source has prices including VAT or an invoice discount, the draft instead uses **Fixed Amount** based on the source's net P/L value and Planned Document Type. Line and invoice discounts are cleared, prices exclude VAT, and a zero quantity becomes 1. This preserves the net commercial value without carrying the old document's VAT or invoice discount.
- With Ordering Party on, the Income customer comes from the target FWO's Ordering Party when its type is Customer. For other party types, the source line's customer is retained and its name is refreshed. With Ordering Party off, copied Income has no customer. For Cost, the source line's vendor is always retained independently of the Ordering Party option.
- Posting Date comes from the target FWO, or the work date when the header date is blank. A missing exchange-rate date uses that new draft date; a missing foreign-currency factor is obtained from the exchange-rate setup. A blank source unit uses the current service or charge default when available.
- The full line dimension set is retained when present. Older lines containing only the two shortcut dimensions apply those values to the new defaults.

Source document links, freight links, allocation, posting and correction history, generation markers, verification status, and legacy metadata are not inherited. A copied Cost line is not assigned to the original purchase document or Freight Order. Amounts in local and reporting currencies are recalculated. The normal validations for services, charges, customers, vendors, units, dimensions, locations, exchange rates, and target status still apply.

Copying does not convert legacy signed prices. After the required amount-model conversion, stored quantity and price signs are preserved, and Type and Planned Document Type determine the new draft's P/L direction. Review the drafts before creating financial documents.

## Freight Order Wizards

Use Freight Order wizards to create or optimize Freight Orders from stages, content, unit types, or selected operational data.

Typical use:

- use **Create Freight Order** on the Forwarding Order card to create one new Freight Order for several unassigned stages,
- create a new Freight Order from a Forwarding Order stage,
- create several Freight Orders from selected content or stages,
- review stage selection before creating carrier execution,
- use **Route Optimization** for independent deliveries loaded together at one pickup location.

### Route Optimization

Open **Route Optimization** from the Freight Order card to choose the delivery order for a shared pickup. Every assigned stage must use the same **From Location**, with one non-parallel stage per Forwarding Order in this Freight Order. **Mode of Transport** must be filled on the header and every stage, with compatible map travel modes, and pickup and delivery map locations must exist. The wizard supports up to **8 different delivery locations**.

1. Review the introduction and continue to **Define conditions**.
2. Review **Start MAP Location**, which is filled from the shared pickup and is read-only.
3. Select **Distance** or **Duration**, then choose **Next** to calculate and apply the route.
4. Read **Result**: it tells you whether the delivery order changed or was retained and shows the calculated distance with its unit or the duration as a time value. Equal-cost alternatives retain the existing delivery order.
5. Choose **Finish** to close the wizard, or use **Back** to select another criterion and calculate afresh from the current stages.

**Simultaneous Load** does not need to be selected beforehand. The wizard sets it on the stages when applying a calculated route. With only one delivery location, the result explains that no rearrangement is required; stages remain unchanged and no calculated goal is shown.

The wizard stops unsupported input before changing stages: different pickups, several assigned stages from one Forwarding Order, parallel stages, incompatible transport modes, missing map data, or more than eight different destinations. For example, **A → X**, **A → Y**, and **A → Z** are supported; **A → X**, **B → Y**, and **C → Z** need separate planning. See [Freight Order](freightorder.md#route-optimization).

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
Concrete logistic units come from both **Content** lines with **Type = Unit** and the units containing linked goods. Available units are included automatically; use **Assign Freight Order** to select individual units.
The created Freight Order stage priorities are assigned as 10, 20, 30, and so on in Forwarding Order line order.

When exactly one stage is selected, the Freight Order Header Vehicle Unit Type is required and defaults from that stage. When multiple stages are selected, the Header Vehicle Unit Type is forced to blank; each Stage Vehicle Unit Type remains authoritative.

The new Freight Order inherits its initial loading and unloading dates from the stages assigned by the wizard, as described below.

### Assign Freight Order

Use **Assign Freight Order** to distribute a stage's selected cargo to an existing Freight Order or create a new Freight Order for that assignment.

In step 4, **Create New Freight Order** is off by default, so you can select an existing Freight Order.

If the selected stage has available concrete logistic units, the next step shows **Include Logistic Units** and a list with **Select** checkboxes. Turn on **Include Logistic Units** and leave only the required units selected; all candidates are selected initially. The list includes units specified directly in FWO **Content** with **Type = Unit**, as well as units found through linked goods. Units already assigned for the same FWO and Stage Code are excluded. Nested units are represented by their outermost parent. If no units are available, this step is skipped. **Include Logistic Unit Types** is a separate step for planned unit types.

For unit types, enter the **No. of Packages** to carry from each FWO content line. Set the quantity to **0** to exclude that line from this assignment.

Turning on **Include Logistic Units** or **Include Logistic Unit Types** makes the selected cargo determine the reserved weight and volume. The wizard replaces the values entered in step 3, including reducing them when less cargo is selected. When both options are on, their selected weight and volume are added together:

- **Unit:** uses the selected unit's current gross weight, including its tare, and current volume in the base units of measure.
- **Unit Type:** takes the selected quantity's share of the source FWO line's saved total gross weight and volume. For example, selecting 3 of 10 packages reserves 30% of that line's totals. This preserves manual adjustments to the line totals; the assigned Freight Order content uses the same calculation.

**Footage remains manual.** Enter the required footage in step 3; changing the cargo selection does not recalculate it. Turning off both **Include** options restores the manual weight and volume entered in step 3.

Review the resulting reserved weight, volume, and footage on **Additional Parameters** before choosing **Assign**. The wizard checks the final reservation against the applicable capacity limits when you choose **Assign**.

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
