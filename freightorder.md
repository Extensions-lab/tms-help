---
title: "Freight Order"
description: "Manage carrier execution for Forwarding Order stages, including carrier, vehicle, content, charges, documents, and route."
---

# Freight Order

Use a **Freight Order** to manage the carrier or internal resource that executes one or more transportation stages.

A Freight Order defines who performs the work, what content is carried, which stages are covered, and which carrier costs are expected or recorded.

![Freight Order card](resources/freightorder/pics/freightorder.png)

## Before you start

Make sure that:

- a Forwarding Order with stages exists,
- a [Freight Order Type](freightordertype.md) exists,
- a carrier or internal executor exists,
- optional vehicle and driver records are ready,
- content or logistic units are available to assign,
- charges are configured if carrier costs are tracked.

## What lives on a Freight Order

| Part | Use it for |
|---|---|
| **General** | Freight Order number, type, document dates, status, and route summary. |
| **Date and Time Planning** | Loading and unloading dates, times, allowed windows, bays, and priorities. |
| **Carrier** | Actual carrier and pay-to vendor or agent. |
| **Vehicle and Driver** | Resource assignment for execution. |
| **Content** | Cargo, logistic units, or unit types assigned to the Freight Order. |
| **Stages** | Forwarding Order stages covered by this Freight Order. |
| **Charges** | Expected or actual carrier cost lines. |
| **Stage Entries** | Execution events, statuses, coordinates, images, and attachments. |
| **Attachments** | Carrier documents, confirmations, and execution files. |
| **Instructions** | Guidance copied from assigned FWO snapshots or added manually. See [Instructions](instructions.md). |

## How to work in this page

The **Freight Orders (TMS)** list starts with **No.**, **Document Date**, **Posting Date**, **External Tracking Number**, **Freight Order Type Code**, and **Status Code**, following the main details on the card. **Load Date And Time**, carrier, mode of transport, vehicle, and driver follow. Payment, cost, transport reference, and loading bay details appear farther to the right. This default order also applies to the list shown by **Assigned Freight Orders** when a Sales Order has several matching Freight Orders.

The **Freight Orders (TMS)** list does not allow creating new Freight Orders. Open an existing order by selecting its **No.** link or choosing **Open Freight Order**. Create orders from a Forwarding Order using **Create Freight Order**, or use the existing creation actions in your Role Center.

1. Create the Freight Order from a Forwarding Order stage or open an existing Freight Order.
2. Select the carrier.
3. Fill pay-to vendor details if the invoice vendor differs from the actual carrier.
4. Add vehicle and driver if they are tracked.
5. Review content and stage lines.
6. Add or review freight charges.
7. Assign purchase lines when carrier invoices must be matched to costs.
8. Print or email the required carrier documents.
9. Update execution information as the work progresses.
10. When the carrier work is complete, choose **Post** before posting any related Forwarding Order.

## Key actions

| Action | Use it for |
|---|---|
| **Show Route** | Display the route for this Freight Order. |
| **Route Optimization** | Choose the delivery sequence for independent orders loaded together at one pickup location. See [Route optimization](#route-optimization). |
| **Freight Order** | Print the carrier instruction document. |
| **Email Freight Order** | Send the Freight Order as a PDF. |
| **Details** | Print logistic unit or cargo details. |
| **Load Summary** | Print a compact load overview, including lot numbers and stage-specific vehicle, regulation, transport-profile, formalized cargo-description, and package conditions. |
| **Packing List** | Print all rows configured under Freight Order - Packing List, in Sequence order. Keep only the required standard, By Drop, By Container, or custom form in setup. |
| **Bill of Lading** | Print all rows configured under Freight Order - Bill of Lading, in Sequence order. Keep only the required standard, By Drop, Dangerous Goods, or custom form in setup. |
| **Check Vehicle Unit Type** | Check the related Stage profiles, cargo assignments, equipment rules, temperature boundaries, and Conditional instructions. |
| **Booking Confirmation** | Print confirmation for the carrier. |
| **Purchase Assignment** | Assign vendor purchase lines to Freight Order charges and settlement costs. |
| **Delete Assignment** (Charges) | Cancel the selected freight charge and delete its active Settlement allocations together. Purchase documents remain; archived allocations and correction dependencies prevent cancellation. See [Purchase Assignment](purchaseassignment.md#assign-a-carrier-invoice-through-a-freight-order). |
| **Stage Entries** | Review execution events, statuses, images, attachments, and map points. |
| **Attachments** | Add or review Freight Order files. |
| **Remove from Freight Order** | Remove selected cargo assignments from the Logistic Units section after reviewing the affected stages. See [Remove cargo](#remove-cargo-from-a-freight-order). |
| **Post** | Close the complete carrier execution document and create its Posted Freight Order. |

## Remove cargo from a Freight Order

In **Logistic Units**, select one or more rows and choose **Remove from Freight Order**. Standard **Delete Line** is disabled in this section.

The removal scope depends on the selected row type:

| Type | What is removed |
|---|---|
| **Unit** | Every assignment of that physical logistic unit in the current Freight Order, across all stages and Forwarding Orders. Selecting one row is sufficient; assignments hidden by filters are included. |
| **Unit Type** | Only the selected rows. Each row represents a planned quantity for its own stage, so other rows with the same unit type are retained. |

One confirmation lists the exact Forwarding Order and stage assignments to be removed. Review the complete list, especially when the Freight Order carries several Forwarding Orders. **No** is the default; choosing No or canceling leaves the assignments unchanged.

Removal preserves the physical logistic unit, its contents and source-document links, and its assignments to other Freight Orders. For example, removing a container from the Freight Order covering **Main carriage** and **Delivery** clears both assignments there while retaining its assignment to the separate **Precarriage** Freight Order.

Planned stage reservations are not recalculated automatically. Review the reserved weight and volume separately if the loading plan has changed. If the affected assignments change during confirmation, or the Freight Order is no longer the same active document, removal stops without a partial result; refresh the Freight Order, review the current rows, and retry.

## Route optimization

Use **Route Optimization** for a Freight Order that loads independent orders together at one location and then delivers them to several destinations. For example, stages **A → X**, **A → Y**, and **A → Z** can be planned as **A → Y → X → Z** to minimize distance or duration.

The supported Freight Order must have:

- one shared, nonblank **From Location** on every assigned stage and a valid map location for each pickup and delivery;
- one assigned, non-parallel stage from each Forwarding Order;
- a filled **Mode of Transport** on the header and every stage, with compatible map travel modes;
- no more than **8 different delivery locations**.

The wizard fills **Start MAP Location** from the shared pickup location; it is read-only. Choose **Distance** or **Duration**, then continue to **Result**. You do not need to turn on **Simultaneous Load** beforehand: the wizard sets it when applying a calculated route.

For a calculated route, the result states whether the delivery order changed or was retained. Equal-cost alternatives retain the current delivery order. The result shows distance with its configured unit or duration in a readable time format. If there is only one delivery location, there is no sequence to optimize; the wizard explains that no rearrangement is required and leaves the stages unchanged.

Different pickup locations, several assigned stages of the same Forwarding Order, parallel stages, incompatible transport modes, missing map data, or more than eight delivery locations stop optimization before stage changes. Correct the Freight Order or plan those routes separately. Using **Back** and calculating again starts from the current stages with fresh calculation state. See [Order Wizards](orderwizards.md#route-optimization).

## Fields that matter most

The print/email groups use standard Business Central Report Selections with the new document usages. Variant-specific buttons are hidden. To produce only one form, leave only its row in the corresponding Usage; Sequence is an output order, not an alternative selector. See [Report selections by document usage](reports.md#report-selections-by-document-usage).

| Field | Why it matters |
|---|---|
| **Carrier No.** | Identifies who executes the stage. |
| **Pay-to Vendor** | Identifies who is paid for the carrier service. |
| **Vehicle No. / Driver No.** | Supports planning, reporting, and execution documents. |
| **Vehicle Registration No.** | Stores the registration number and optional extra text printed in the Dangerous Goods Bill of Lading. |
| **Mode of Transport** | Helps classify execution and documents. |
| **Bill of Lading No.** | Stores the number used by Bill of Lading reports; it can be supplied manually or assigned automatically at creation. |
| **Voyage Number** | Records the shipping line or vessel operator's reference for a voyage. |
| **Content totals** | Show capacity and cargo values. |
| **Freight charges** | Feed cost control and settlement. |

Changing **Carrier No.** clears any selected **Vehicle No.** or **Driver No.** that no longer belongs to the new carrier, together with its display name. The new carrier's resource defaults are then applied; resources remain blank when no applicable default is available. Revalidating the same carrier keeps valid manual resource assignments. Clearing the carrier clears the resource numbers and names. Assigned stages receive the final carrier, vehicle, and driver together, including their names.

A different carrier also clears the previous **Freight Agreement No.** and applies the new carrier's default agreement when configured. The selected **Pay-to Vendor** is retained because payment and execution can use different parties. Shared Forwarding Order resource changes apply only to stages without a Freight Order; an assigned stage takes its execution resources from its Freight Order. New or changed resource assignments are checked on save, including API creation and updates.

Carrier and Driver lookups exclude blocked resources. A blocked carrier cannot be newly assigned, and a blocked driver cannot be newly assigned or retained when changing the carrier. This also applies to entering a carrier number or name, API assignments, and driver defaults from a carrier or vehicle. If a default driver is blocked, choose another default or remove that default before completing the assignment.

Blocking a resource later preserves its existing assignment. You can read the order, make unrelated edits, replace other resources, or clear the assignment without reselecting a blocked resource. Adding a new stage or transferring a stage to another Freight Order checks the destination order's carrier and driver again, even when their numbers are unchanged. Historical records remain available.

Enter the optional **Voyage Number** in **General**, using the reference from the booking confirmation or bill of lading. It accepts up to 50 text characters and preserves their case and format. Voyage Number identifies the voyage and is separate from the vessel's IMO Number, the carrier's SCAC, and Flight Number. Posting automatically saves it on the **Posted Freight Order**. Both the active and posted Freight Order APIs expose it as **voyageNumber**. **Voyage Number** is also available in the **Freight Orders** list; it is hidden by default and can be shown through personalization.

**Vehicle Registration No.** is in **Vehicle and Driver > Vehicle** on the card. It is hidden by default; use personalization to show it. Enter up to 50 text characters, including any additional description needed on the transport document. You can enter it independently, without selecting a **Vehicle No.** or **Carrier No.**, for example for a pickup truck that is not maintained in the vehicle register.

Selecting a different **Vehicle No.** copies its **Registration No.** into this field. You can then change or clear the copied value manually. Revalidating the same Vehicle retains that manual value. Clearing Vehicle or Carrier also retains it for independent use. Later changes on the Vehicle card do not update the number already saved on the Freight Order.

Posting saves **Vehicle Registration No.** on the **Posted Freight Order** as a read-only snapshot, also hidden by default in its **Vehicle** group. The **Dangerous Goods Bill of Lading** prints the saved Freight Order value in **VEHICLE/CAR NO**. Existing orders and posted versions are not filled automatically; an existing active order needs manual entry or selection of a different Vehicle before this value appears on the report. See [Reports and Documents](reports.md#dangerous-goods-bill-of-lading).

## Automatic Bill of Lading numbering

Configure **TMS Setup > Freight Order > Bill of Lading Numbering** to fill a blank **Bill of Lading No.** when creating a Freight Order:

- Leave **Numbering Method** blank to keep automatic numbering off.
- Choose **Prefix + Freight Order No.** to join **Prefix** directly to the Freight Order's own number. For example, **BOL-** + **FO-00001** becomes **BOL-FO-00001**. Include the separator in the prefix; a blank prefix uses the Freight Order number alone.
- Choose **No. Series** and fill **Bill of Lading Nos.** with a series that has **Default Nos.** enabled to obtain a number such as **BOL00001**. The series uses Document Date, falling back to the work date when Document Date is blank.

**Prefix** and **Bill of Lading Nos.** are editable only for their matching method, and switching methods retains both values. The complete generated number must fit the 20-character Bill of Lading No. field; an overlong prefix result causes an error without truncation. See [TMS Setup](setup.md#bill-of-lading-numbering) for the three settings and series requirements.

If creation stops because the Bill of Lading number series is missing, open **TMS Setup > Freight Order > Bill of Lading Numbering** and fill **Bill of Lading Nos.** To enter Bill of Lading numbers manually instead, leave **Numbering Method** blank. Then create the Freight Order again.

Generation happens after the Freight Order number exists and applies to the card, wizards, and API creation, including an explicitly supplied Freight Order number. A Bill of Lading No. already supplied manually or through the API is preserved. Temporary records do not consume Bill of Lading sequence numbers.

Existing Freight Orders are not filled or renumbered when this setting changes. Later edits, renaming, and printing do not recalculate the number. Reports use the stored Bill of Lading No., and posting automatically copies it to the Posted Freight Order.

## Plan loading and unloading

Open the **Date and Time Planning** FastTab. The **Loading** and **Unloading** groups use the same fields:

| Field | Use it for |
|---|---|
| **Date** | Choose the date when the allowed window starts. |
| **Time** | Choose the time when the allowed window starts. |
| **Date Time** | Enter the combined start date and time. |
| **Window** | Enter the duration during which loading or unloading is allowed, for example 2 hours. This is an allowed window, not the actual handling duration. |
| **Window End** | Review the calculated end of the window. This field is read-only. |
| **Bay** | Identify the bay or staging area. |
| **Priority** | Record the loading or unloading priority. |

Changing Date or Time updates Date Time. Changing Date Time updates both Date and Time. Changing any start field or Window recalculates Window End.

For example, unloading on September 8 at 23:00 with a 2-hour window ends on September 9 at 01:00.

- Entering a date with a blank time uses midnight.
- You can enter a time before the date; the combined start and end remain blank until a date is entered.
- Clearing Date clears Date Time and Window End, but retains Time for later entry.
- Clearing Date Time clears Date, Time, and Window End.
- Window must be zero or greater. A zero window makes Window End equal to Date Time.
- Loading and unloading are planned independently; these fields do not reschedule the assigned stage lines.
- Posting preserves these values on the same FastTab of the **Posted Freight Order** card.

### Planning dates from assigned stages

After a successful **Assign Freight Order** assignment, the Freight Order's planning dates are recalculated from all stages assigned to it, including stages already attached and the newly assigned stages. Stages from other Forwarding Orders are included when they belong to the same Freight Order.

**Loading Date Time** becomes the earliest nonblank stage start date and time. **Unloading Date Time** becomes the latest nonblank stage end date and time. Each available boundary replaces the corresponding header value, including a manually entered value; previous header dates do not participate in the calculation.

The two values are independent: if no assigned stage has a start or end, the corresponding header value is retained. The separate Date and Time fields and Window End values synchronize automatically; Loading Window and Unloading Window durations remain unchanged.

The **Create Freight Order** wizard and the batch wizard's creation mode use the same stage-date calculation for a new order, retaining a creation default when its corresponding boundary is missing. The batch wizard's mode for adding to an existing order preserves its planning dates.

In step 4 of **Assign Freight Order**, **Create New Freight Order** is off by default. Selecting an existing order does not write or recalculate its dates before **Assign** succeeds. Turning on the creation option lets you review the stage's default carrier and vehicle unit type before **Assign** creates the order. Carrier is optional and Vehicle Unit Type Code is required. See [Order Wizards](orderwizards.md).

Manual card creation, including **New** from a Freight Order lookup, uses normal creation defaults and does not itself inherit stage dates. Completing **Assign Freight Order** for that order later recalculates its dates from all its assigned stages.

## Check Transport Profiles and equipment before execution

Set up the reusable scenarios and cargo assignments in [Transport Profiles and Dangerous Goods](transportprofiles.md). Select a profile on each relevant [Forwarding Order Stage](stages.md#select-a-transport-profile-on-an-order-stage), then:

1. Review each Stage's **Vehicle Unit Type Code**. A filled Stage value is used for that leg; a blank Stage value uses the Freight Order header value.
2. Select **Vehicle Unit Type Code** on the Freight Order. Its lookup intersects the equipment rules of profiles on Stages with blank equipment. Explicit Stage equipment does not restrict that header lookup.
3. Check **Temperature Control** and **Temperature**. Selecting a Vehicle Unit Type can supply these defaults from the type. When Temperature Control is on, use a common temperature unit across the Stage profiles because Freight Order has no separate temperature-unit field.
4. Choose **Check Vehicle Unit Type**. The explicit check requires a header Vehicle Unit Type, at least one linked Stage, and a Transport Profile on every linked Stage, including Service Stages.
5. Read any Conditional equipment instruction and confirm only after checking that the requirement is met. A confirmation is not stored as a lasting approval.
6. Repeat the check after changing cargo, assignments, profile data, equipment, or temperature.

No equipment rules means a profile places no equipment restriction. Once a profile has one rule, every unlisted type is rejected. Temperature boundaries can apply to Preferred and Allowed rules as well as Conditional rules. The lookup alone does not prove that those conditions are met.

Automatic save and Stage-assignment checks permit incomplete planning: they can skip Stages with no profile and skip equipment validation when the header Vehicle Unit Type is blank. They do not replace the explicit check. The feature does not add a separate transport-profile release/posting approval gate.

Before issuing **Dangerous Goods Bill of Lading**, fill **Bill of Lading No.**, load the actual logistic units, and review the first Stage's transport context. The report resolves a profile for each loaded Item/Product from cargo assignments; it does not use the selected Stage profile as a fallback. It also does not perform the full interactive equipment check. See [Reports and Documents](reports.md#dangerous-goods-bill-of-lading).

## Post a Freight Order

Post the Freight Order when all work covered by it is complete. Finish the related vendor purchases and distribute every Freight Order charge to the related Forwarding Orders' Settlement before posting. All linked purchase documents must be posted, and the Freight Order charges must be fully allocated. Review its stages, content, charges, instructions, and attachments before choosing **Post** on the card.

Posting creates one **Posted Freight Order** for the complete Freight Order. It saves the header, all assigned stages and content, charges, instructions, and Freight Order attachments together, then removes the active Freight Order. When the Freight Order combines several Forwarding Orders, its posted record includes all of them; it is not split into a separate archive for each customer job.

Posting refreshes stored record links from the existing order numbers and stage membership before checking the documents. Older orders with empty or stale stored links can therefore be posted without a separate link-repair operation. The operation preserves their saved execution and financial values; missing or ambiguous underlying records must still be resolved.

If posting reports that related Forwarding or Freight Orders changed while it was being prepared, refresh the order, review its assigned stages and charges, and retry. The unsuccessful attempt does not leave a partial posted trip. This can happen when another user changes assignments while the trip is being closed.

The related Forwarding Orders can remain active while customer income, sales invoicing, and other closing requirements are completed. Their stage lines retain the link to this exact Posted Freight Order. Its saved execution and carrier cost details are historical snapshots. Later Forwarding Order posting completes the job's historical links without rewriting those details. Each Forwarding Order can be posted only after every Freight Order assigned to its stages has been posted.

For example, **FO-100** carries jobs **FWO-A** and **FWO-B** and has a shared carrier confirmation. Post **FO-100** once after the trip. Its confirmation remains with that Posted Freight Order. Later, post **FWO-A** and **FWO-B** separately; both histories refer to the same completed trip, and neither creates another Posted Freight Order or takes its shared files. Attachments belonging directly to a Forwarding Order move with that Forwarding Order when it is posted.

## Good to know

- A Forwarding Order can have several Freight Orders.
- When configured on the Stage Profile, the Default Pay-to Vendor flows through the Forwarding Order stage into a newly created Freight Order. A Pay-to Vendor linked through the selected Carrier remains the fallback when the stage has no default.
- One Freight Order can cover content or stages from more than one Forwarding Order when the operation allows it.
- Stage Priority follows the source Forwarding Order sequence when all stages belong to one FWO. Users can edit Priority only when the Freight Order combines stages from different Forwarding Orders.
- In a combined multi-FWO Freight Order, Priority values may interleave different FWOs but cannot reverse or duplicate the original stage order inside the same Forwarding Order.
- Freight Order charges can be distributed back to settlement.
- Purchase Assignment connects vendor invoice lines to Freight Order charges.
- Stage Entries give operations a timeline of execution events.
- Print and email actions use Business Central report selection and email setup.

## Troubleshooting

| Problem | What to check |
|---|---|
| Freight Order cannot be created | Check Forwarding Order status and stage selection. |
| Carrier defaults are missing | Check the carrier card for default vehicle, driver, and unit type. |
| Route is not shown | Check map locations on related stages and map provider setup. |
| Report is missing data | Review carrier, vehicle, driver, content, and stage lines before printing. |
| Vehicle Unit Type is missing or rejected | Review profile equipment rules, the Stage/header equipment precedence, temperature boundaries, and Conditional instructions. Run the explicit check after completing all Stage profiles. |
| Charges do not appear in settlement | Review charge distribution and settlement recalculation. |
| Vendor invoice line cannot be assigned | Check vendor, purchase document status, charge setup, and whether the line is already assigned. |
| Freight Order posting is blocked by costs | Post its linked purchase documents and finish allocating every Freight Order charge to Forwarding Order Settlement before posting the trip. |
| A related Forwarding Order cannot be posted | Post every Freight Order assigned to its stages first. Posting refreshes stored links automatically, but the completed trip stage must exist and be uniquely identifiable in history. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Freight Load Management](freightloadmanagement.md)
- [Freight Order Types](freightordertype.md)
- [Purchase Assignment](purchaseassignment.md)
- [Stage Entries](stageentries.md)
- [Settlement](settlement.md)
- [Reports and Documents](reports.md)
- [Carriers](carrier.md)
- [Transport Profiles and Dangerous Goods](transportprofiles.md)
