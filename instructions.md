---
title: "Instructions"
description: "Set up reusable instructions, adapt them for customers and addresses, and control their use in orders and printed documents."
---

# Instructions

Use **Instructions** to keep reusable operational guidance in TMS and adapt it for a specific customer, Ship-to Address, Sales Order, Forwarding Order (FWO), or Freight Order (FO). This reduces repeated typing and helps dispatchers, warehouse staff, and carriers receive the right requirements for each job.

Typical examples include booking a delivery slot, calling before arrival, using a particular gate, packing requirements, and carrier handling notes. Keep a common instruction in the master list, adapt it for a customer's site, and make a further change on an individual order when necessary.

Instruction content uses the Business Central rich-content editor. It can contain formatted text, tables, links, and embedded images. **The bundled reports that print Instructions use plain text, without the embedded pictures.** Write every requirement needed on paper as text; see [General recommendations](#general-recommendations).

On this page:

1. [Set up types and master instructions](#create-instruction-types).
2. [Override instructions for a customer or address](#add-entity-specific-instructions).
3. [Control instructions in FWO and FO](#priority-and-propagation).
4. [Control instructions in reports](#instructions-in-reports).
5. [General recommendations](#general-recommendations).

## Before you start

Make sure that:

- **Instructions Enabled** is selected on [TMS Setup](setup.md),
- an administrator has created the required instruction types and master instructions,
- users have **TMS User** or **TMS Administrator** permissions,
- the business has decided which instructions belong in Forwarding Orders, Freight Orders, Bill of Lading datasets, and Load Summary datasets,
- the business has defined how report comments should be grouped by audience or logistics context.

When the feature is disabled, entity and document instruction sections are hidden. Administrators can still prepare **Instruction Types** and **Instructions** before enabling the feature.

Disabling the feature does not remove saved instruction rows or exclude them from reports. Use the document instruction settings and report layout to control printed output.

## Enable instructions

1. Open **TMS Setup**.
2. Select **Instructions Enabled**.
3. Reopen any Customer, Ship-to Address, Sales Order, Forwarding Order Type, Forwarding Order, or Freight Order page that was already open.
4. Confirm that the **TMS Instructions** or **Instructions** section is visible.

New companies start with instructions enabled. Existing companies can remain disabled until an administrator enables the feature.

## Create instruction types

Use **Instruction Types** to define reusable categories and their defaults, for example carrier guidance, internal handling, forwarding process, or unloading requirements.

1. Open **Instruction Types** from TMS Setup or a TMS role center.
2. Enter a code and description.
3. Select the default **Comment Group**.
4. Select the default applicability fields.
5. Create the record.

The Comment Group and applicability fields are copied as defaults when **Instruction Type Code** is selected on a master instruction. Editing the type's defaults later does not rewrite existing instructions. Selecting another type on a master instruction copies that type's defaults again, so review the resulting group and flags.

Keep these three settings separate:

| Setting | Question it answers | Example |
|---|---|---|
| **Instruction Type Code** | Which reusable business category and defaults should this instruction use? | A type you create named `SITE-ACCESS`. |
| **Comment Group** | Who is the instruction for, or which report section should contain it? | **Carrier**, **Shipper**, or **Pick-Pack Instructions**. |
| **For ...** fields | Which automatic document flow or report dataset should include it? | **For Freight Order** and **For Load Summary**. |

Comment Groups are predefined choices; create your own instruction types when you need more business categories. A type named `DRIVER` does not by itself select the **Driver** group or make the text print in a carrier section.

## Create master instructions

1. Open **Instructions**.
2. Choose **New**.
3. Enter the code and description.
4. Select an **Instruction Type Code**.
5. Review the Comment Group and applicability fields copied from the type.
6. Enter the formatted instruction content.
7. Save the instruction.

Operational users can view master instructions. Maintaining instruction types and master instructions requires **TMS Administrator** permissions.

Creating a master instruction alone does not add it to orders. Assign it to a Customer, Ship-to Address, Sales Order, or Forwarding Order Type, or add it manually on a document. On the master instruction, **No. of Assignments** opens the list of these source assignments; use **Open Source** to inspect a local variant. This count does not include FWO or FO instruction copies.

## Applicability fields

Applicability fields control automatic propagation and report dataset selection. They do not prevent a manager from deliberately adding a master instruction manually to a Forwarding Order or Freight Order.

| Field | Use it for |
|---|---|
| **For Forwarding Order** | Include a customer, address, or Sales Order instruction in the FWO, even when it is not needed downstream. |
| **For Freight Order** | Copy the instruction automatically from the final FWO snapshot into a related Freight Order. |
| **For Bill of Lading** | Carry the instruction into the Freight Order and make it available to the Bill of Lading report dataset. |
| **For Load Summary** | Carry the instruction into the Freight Order and make it available to the Load Summary report dataset. |

One instruction can have any combination of these fields.

For Customer, Ship-to Address, and Sales Order sources, **at least one** of the four fields must be selected for automatic inclusion in an FWO. **For Forwarding Order** does not have to be selected if one of the other three is selected. Instructions assigned to the **Forwarding Order Type** are included in the FWO regardless of these flags. Further automatic copying from FWO to FO requires **For Freight Order**, **For Bill of Lading**, or **For Load Summary**.

## Comment groups

**Comment Group** classifies an instruction by its intended audience or logistics context. It is stored on every master, entity, active document, and posted instruction snapshot so report layouts can group comments without interpreting their text.

| Comment Group | Typical use |
|---|---|
| **Blank (Unspecified)** | Existing or not-yet-classified instructions. The field is blank in the interface. |
| **General** | General external or document-wide guidance. |
| **Shipper** | Instructions intended for the shipper. |
| **Consignee** | Instructions intended for the consignee. |
| **Additional Consignee** | Instructions for an additional receiving party. |
| **Pickup** | Pickup, loading-site, or origin handling guidance. |
| **Pick-Pack Instructions** | Warehouse picking, packing, and shipment-preparation guidance. |
| **Delivery** | Delivery, unloading-site, or destination handling guidance. |
| **Carrier** | Instructions intended for the carrier organization. |
| **Driver** | Driver-specific execution guidance. |
| **Customs** | Customs, border, or clearance guidance. |
| **Internal** | Internal operational notes. |

Comment Group does not change applicability automatically. For example, **Internal** with **For Bill of Lading** selected is still available in the general Bill of Lading dataset, even if the bundled layout does not display it. A custom layout could print it. Review both the group and applicability fields; the group is not an access restriction.

## Add entity-specific instructions

The reusable **TMS Instructions** section is available on:

- Customer Card,
- Ship-to Address Card,
- Sales Order,
- Forwarding Order Type.

To create a local variant:

1. Open the source record.
2. Expand **TMS Instructions**.
3. Select an **Instruction Code**.
4. Review the copied description, type, Comment Group, applicability, and content.
5. Change the description, Comment Group, or applicability on that local row when the source needs special handling.
6. Open **Content**, choose **Edit**, update the text, and choose **Save and Close**.

You can open **Content** or choose **Edit** immediately after selecting the instruction code; there is no need to leave the row first. The instruction row and any pending field changes are saved before the editor opens. Closing the editor without **Save and Close** keeps that row but discards unsaved content edits. This also applies to the master instruction list and the FWO and FO instruction lists.

The copied instruction is independent of the master. Use **Reset from Master** when the local variant must be replaced with the current master description, type, Comment Group, applicability, and content. This discards local changes; it restores the master, not the customer's variant.

### Example: different arrival rules for one address

1. Create an instruction type `SITE-ACCESS`, set **Comment Group = Carrier**, and select **For Freight Order** and **For Load Summary**.
2. Create master instruction `ARRIVAL` with that type and the content "Call the site before arrival."
3. Add `ARRIVAL` to the Customer's **TMS Instructions**. Change its content to "Call the site 30 minutes before arrival."
4. Open that Customer's specific **Ship-to Address**, add the **same Instruction Code** `ARRIVAL`, and change its content to "Call 60 minutes before arrival and use Gate 2." Keep its applicability fields selected.
5. Create an FWO with that Customer as **Consignee** and that Ship-to Address as **Unload Address Code**. Review the address-specific text in **Instructions**.
6. Assign an FWO stage to an FO. The instruction is copied into the FO; its **Carrier** group makes it printable in the Freight Order report, and **For Load Summary** makes it printable in the Load Summary carrier section.

Using the same code creates an override. Using a different code adds a separate instruction, so both texts can reach the order. An address variant starts from the master; include any customer requirements that should remain in its replacement text.

## Priority and propagation

When the same **Instruction Code** exists at several eligible levels, the more specific source wins. The lists below run from lowest to highest priority.

For Sales Orders, the priority is:

1. Customer,
2. Ship-to Address,
3. manual Sales Order instruction.

For Forwarding Orders, the priority is:

1. Forwarding Order Type,
2. consignee Customer,
3. consignee Ship-to Address,
4. Sales Order instructions imported when creating the FWO from a Sales Order or adding its lines,
5. manual Forwarding Order instruction.

The winning variant supplies its description, type, Comment Group, applicability, and complete rich-content value. Manual document instructions are preserved when automatic instructions are refreshed.

For the FWO customer/address route, TMS uses **Consignee Type = Customer**, **Consignee No.**, and **Unload Address Code**. Setting a Sell-to Customer or Shipper alone does not select that customer's instructions through this route.

Clearing all four applicability flags on an address variant is not a way to suppress a lower-priority instruction. That address variant is skipped, and an eligible Customer or Forwarding Order Type variant with the same code can still apply.

## Copy a Sales Order with instructions

Use the standard **Copy Document** action with **Include Header** selected to copy the source document's saved instructions into the destination Sales Order.

The copy replaces the complete instruction list. Existing destination instructions are deleted, including manual, edited and automatically added rows. Each source instruction retains its description, type, Comment Group, applicability, complete rich text and customization/automatic status. The source document is unchanged. The saved document variants are used, so later changes to the master instructions do not replace the copied text.

If the source has no instructions, the destination's instruction list becomes empty. With **Include Header** cleared, only the document lines are copied and the destination instructions are preserved. **Recalculate Lines** does not change instruction copying.

Posted and archived documents currently do not retain an instruction snapshot. Copying their header therefore leaves the destination without instructions; the current open order's instructions are not treated as historical instructions. Review the destination's **TMS Instructions** after copying from these sources.

## Work with Forwarding Order instructions

The **Instructions** section appears immediately after General on the Forwarding Order.

TMS builds the automatic FWO snapshot (a saved copy) from the Forwarding Order Type, consignee, and delivery address. Sales Order instructions are imported when creating the FWO from the Sales Order or adding its lines. Both routes use the priority and applicability rules above.

1. Set the FWO type, consignee, and unloading address as appropriate for the job. For Sales Order work, use the supported create-FWO or add-lines flow to import its instructions.
2. Review the automatically added rows in **Instructions**, including their text, **Comment Group**, and all four applicability fields.
3. For a one-off requirement, add a row and select an **Instruction Code**. Manual selection accepts any master instruction, regardless of its applicability fields.
4. Open **Content** to preview the text, choose **Edit**, and use **Save and Close** to keep an order-specific change when status and permissions allow it.
5. To pass a row to an FO, select **For Freight Order**, **For Bill of Lading**, or **For Load Summary** on the FWO row before assigning its stage.

For an instruction needed only by the FWO team, use **Comment Group = Internal**, select **For Forwarding Order**, and clear the other three fields. The row remains in the FWO and is not automatically copied to the FO.

The **FWO Instructions** FactBox in [Forwarding Control Tower](forwardingcontroltower.md) shows the code and description for the selected order. Choose **Open** for a read-only formatted preview.

## Work with Freight Order instructions

The **Instructions** section appears immediately after General on the Freight Order.

Automatic FO instructions are created from the already materialized FWO snapshots assigned through stages. TMS does not reread current Customer, Ship-to Address, Sales Order, or Forwarding Order Type master data at this point.

Instructions marked **For Freight Order**, **For Bill of Lading**, or **For Load Summary** are copied automatically. Managers can add any master instruction manually, regardless of its applicability fields.

When one Freight Order contains stages from several Forwarding Orders, TMS keeps source-specific variants of the same instruction code separately.

1. Assign the required FWO stages to the FO.
2. Review **Instructions** on the FO, especially any different site requirements coming from several FWOs. Personalize the list to show **Source Forwarding Order No.** when you need to identify their origins.
3. Adjust the FO row's text, **Comment Group**, **For Bill of Lading**, and **For Load Summary** for this execution job.
4. Add another **Instruction Code** manually when an instruction applies directly to the FO. Such a row has no **Source Forwarding Order No.**
5. Preview the required report and check its instructions before sending it.

During automatic rebuilding, a manual FO instruction with no **Source Forwarding Order No.** takes priority over the same code from all source FWOs. An edited source-specific row takes priority for its particular FWO. Review existing rows as well; adding a manual row does not immediately rebuild the automatic copies.

### Changing instructions after orders already exist

Instruction copies are not live links to their masters. Configure recurring requirements before creating and planning orders.

| Change | What happens to existing instructions |
|---|---|
| Edit a master, Customer, or Ship-to Address instruction | Existing FWO and FO copies are not updated automatically. |
| Create an FWO or change its type, consignee, or unloading address | Automatic instructions from the FWO Type, Customer, and address are rebuilt. Previously imported Sales Order instructions and manual or edited rows are retained. |
| Create an FO through planning, or add, move, or remove assigned stages | Automatic FO instructions are rebuilt from the saved FWO copies; manual or edited FO rows are retained. |
| Edit an instruction on an existing FWO | The FWO changes; an already created FO does not immediately receive the edit. |
| Print a report | The report reads the saved FO instructions without refreshing them. |

There is no separate **Refresh Instructions** action. For an urgent change to an existing job, review and edit the current FWO and its FO instructions directly. Changing text, description, group, or applicability marks a document row as a manual customization, so later automatic rebuilding preserves it.

Deleting an automatic row is not a permanent exclusion: a later rebuild can add it again. To keep an instruction only on the FWO, clear **For Freight Order**, **For Bill of Lading**, and **For Load Summary** on its FWO row, then review and remove any unwanted copy already saved on the FO separately.

## Preview and edit content

- Choose **Content** or **Open** to view formatted content.
- Choose **Edit** in an editable instruction window to use the visual rich-content editor.
- Choose **Preview** to review unsaved formatting before saving.
- Choose **Save and Close** to store the complete content.
- Closing without saving leaves the stored content unchanged.

Posted instructions and the Control Tower FactBox open in preview-only mode.

## Instructions in reports

Printed output depends on the instructions **saved on the FO**, their **Comment Group**, their report flags, and the selected layout. Setting a flag alone does not guarantee that the bundled layout has a section for that group.

| Report | Required settings on the saved FO instruction | Output in the bundled layout |
|---|---|---|
| **Freight Order** | **Comment Group = Carrier**. No applicability flag is required. | **Carrier Instructions & Notes** below the carrier details. |
| **Load Summary** | **For Load Summary**, with **Shipper**, **Pick-Pack Instructions**, or **Carrier** as the group. | Shipper/Pick-Pack text within each relevant Drop; Carrier text after all Drops. |
| **Dangerous Goods Bill of Lading** | **For Bill of Lading** and **Comment Group = Carrier**. Both are required. | **Carrier Instructions** for the FO. |
| **Bill of Lading** | **For Bill of Lading**, with any group. | Available in the report dataset, but not printed by the bundled Word layout. A custom layout must map the instructions. |
| **Bill of Lading by Drop** | **For Bill of Lading**, with any group. | Available in the report dataset, but not printed by the bundled Word layout. A custom layout must map the instructions. |

To control printing:

1. Open **Instructions** on the current FO and check the saved text.
2. Set **Comment Group** for the intended report section. For example, **Driver** does not appear in the bundled **Carrier** section; choose **Carrier** when that is where the text belongs.
3. Select or clear **For Bill of Lading** and **For Load Summary** as required. Clearing them does not hide a **Carrier** instruction from the **Freight Order** report, which selects by group alone.
4. Check the report selected for the document in **Report Selection - Transportation Management System**, then check its active layout in **Report Layout Selection**. [TMS Setup](setup.md) describes the initial report defaults. Custom layouts may use different sections.
5. Preview the document. Verify the text and the recipient before printing or emailing.

For example, a **Carrier** instruction with **For Bill of Lading** cleared and **For Load Summary** selected prints in **Freight Order** and **Load Summary**, but not in **Dangerous Goods Bill of Lading**.

### Load Summary placement

The bundled Load Summary Word layout prints **Shippers / Pick-Pack Instructions** inside each Drop and **Carrier instructions** after all Drops. Only saved Freight Order instructions with **For Load Summary** selected are included. The Drop block combines the **Shipper** and **Pick-Pack Instructions** groups for the source FWOs in that Drop; manually added FO instructions without a **Source Forwarding Order No.** apply to every Drop. The Carrier block includes the **Carrier** group for the whole FO.

Other Comment Groups remain available in the general dataset but are not printed by the bundled layout. Identical text is printed once within each block. When converted content is empty, the instruction description is used.

### Instructions versus document comments

The FO **Comment** box is separate from the Instructions mechanism. It prints as **General Notes / Comments** in Freight Order, as **General Instructions** after **DELIVERY NOTES:** in Dangerous Goods Bill of Lading, and as general instructions in Packing List by Container. These fields do not use Instruction Types, Comment Groups, or the instruction applicability flags.

The Dangerous Goods Bill of Lading carrier block uses eligible instructions from the whole FO, including copies from FWOs. It does not further limit those instruction sources to the FWOs represented by the loaded cargo.

There is no automatic Instructions block in Forwarding Order Confirmation or the packing-list layouts. For **Packing List by Container**, use the separate FO **Comment** for its general instruction text; adding an Instructions row does not populate that field.

### Custom report layouts

The ordinary Bill of Lading, Bill of Lading by Drop, and Load Summary datasets expose Source FWO No., Instruction Code, Comment Group, the formatted Comment Group caption, Description, and the complete HTML content. Instruction rows are sorted by Comment Group before Source FWO No. and Instruction Code so layouts can render contiguous sections. HTML in the dataset does not mean that a Word layout renders formatted content or pictures automatically.

For **Bill of Lading by Drop**, the general instruction dataset contains eligible instructions for the whole FO. It does not filter them by each Drop's Source FWO No.; a custom layout must account for the intended scope.

The Load Summary dataset also exposes three independently placeable repeaters for Word layouts:

| Repeater | Comment Group |
|---|---|
| **ShipperInstructions** | **Shipper** |
| **PickPackInstructions** | **Pick-Pack Instructions** |
| **CarrierInstructions** | **Carrier** |

Each named repeater provides its section caption, Source FWO No., Instruction Code, Description, and plain-text instruction content. These repeaters remain available for custom layouts.

## Posted instructions

**Freight Order Post** first creates the read-only instruction snapshot for the complete Posted Freight Order. When the trip covers several Forwarding Orders, all of its saved instruction rows remain together with that trip.

Later, **Forwarding Order Post** creates the instruction snapshot for that customer job's Posted Forwarding Order. It retains the links to the Freight Orders already posted and does not recreate or change their instruction snapshots.

The snapshot preserves:

- instruction code and description,
- instruction type,
- Comment Group,
- applicability fields,
- source identity,
- formatted content and embedded images,
- whether the instruction was automatic or customized.

Later changes to master or entity-specific instructions do not change posted history.

## Permissions

| Permission set | Instruction access |
|---|---|
| **TMS User** | Read master instructions and maintain Customer, Ship-to Address, Sales Order, FWO, and FO instruction variants. |
| **TMS Administrator** | Includes operational access and can maintain Instruction Types and master Instructions. |

The internal **TMA Instructions** and **TMA Instructions Admin** capabilities are included automatically in the standard TMS roles. Normally, do not assign internal permission sets directly.

## Good to know

- Changing a master instruction does not rewrite existing entity, active document, or posted snapshots.
- Use **Reset from Master** when an entity-specific variant must receive the latest master values.
- Applicability fields control automatic propagation. Manual FWO and FO selection accepts any master instruction.
- Comment Group is a snapshot value and can be overridden independently on Entity, FWO, and FO instructions.
- Rich-content images are stored inside the instruction content and copied with the snapshot.
- Posted instructions are intentionally read-only.

## General recommendations

- **Write printable requirements as text.** The bundled Freight Order, Load Summary, and Dangerous Goods Bill of Lading instruction blocks print plain text. Embedded pictures are not printed and text inside pictures is not recognized. An image may be replaced by its alternative text or an **[Image omitted]** marker.
- **Use simple formatting.** Paragraph breaks, lists, table text, and links can survive text conversion, but colors, fonts, picture placement, and the original table appearance do not. Preview the report, not just the rich-content editor.
- **Keep one reusable purpose per code.** Use the same code for a customer/address override and a different code for an additional independent requirement. Write the full replacement requirement in an override; texts from different levels are not merged.
- **Review confidentiality through the actual output settings.** Internal FWO-only guidance should normally use **Internal**, **For Forwarding Order**, and no downstream flags. Check any manual FO copy and custom layout separately.
- **Maintain the source and the current job deliberately.** Update Customer or Ship-to Address variants for future jobs. Review existing FWO and FO copies separately when a requirement changes during execution.
- **Preview each affected form after changing the setup.** A group can print in one report and be omitted from another. Check the placement for each Drop when an FO serves several destinations.

## Troubleshooting

| Problem | What to check |
|---|---|
| The instruction section is not visible | Confirm **Instructions Enabled** in TMS Setup, then reopen the page. |
| A user cannot edit master instructions | Assign **TMS Administrator** when the user maintains instruction setup. |
| The wrong text appears in a Sales Order or FWO | Check whether a more specific Ship-to Address, Sales Order, or manual variant has the same Instruction Code. |
| An instruction appears under the wrong report section | Check Comment Group on the document instruction. Automatic snapshots inherit it from the winning entity/master variant; manual changes mark the document instruction as customized. |
| An instruction is not copied automatically to FWO | Assign the master to a source first. Check its applicability fields and the FWO's consignee, unloading address, type, or imported Sales Order. Existing copies are not updated by source edits alone. |
| An instruction is not copied automatically to FO | Check **For Freight Order**, **For Bill of Lading**, or **For Load Summary**, and confirm the source FWO stage is assigned to the FO. |
| Instruction text does not appear in a report | Check the saved FO row against the report table above. Ordinary Bill of Lading layouts require custom instruction mapping; DG Bill of Lading requires both Carrier and For Bill of Lading. |
| Text still prints in Freight Order after report flags are cleared | Freight Order prints saved Carrier instructions by group alone. The BoL and Load Summary flags do not control this report. |
| Changed customer/address text is missing from an existing FO | Review the saved FWO and FO copies separately. Editing a source or printing does not update the existing FO. |
| A deleted instruction returns | A later automatic rebuild can restore it from its source. Review source assignments and downstream flags; deletion is not a permanent exclusion rule. |
| A picture is missing from the printed instruction | Bundled instruction blocks print text only. Put essential requirements into the text itself. |
| Content cannot be edited | Check document status, user permissions, and whether the record is posted. |
| Posted content differs from the current master | This is expected. Posted documents keep the snapshot created during posting. |

## Related

- [TMS Setup](setup.md)
- [Forwarding Order Types](forwardingordertype.md)
- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Forwarding Control Tower](forwardingcontroltower.md)
- [Reports and Documents](reports.md)
- [Posted History](postedhistory.md)
- [Assign Permission Sets](assignpermissionsets.md)
