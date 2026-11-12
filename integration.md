---
title: "Business Central Integration"
description: "Understand how TMS uses standard Business Central customers, vendors, sales documents, purchase documents, and posted documents."
---

# Business Central Integration

TMS for Logistics Service Providers runs inside Microsoft Dynamics 365 Business Central and uses standard Business Central records for customers, vendors, contacts, financial documents, permissions, and reporting.

Use this article to understand which standard records TMS expects users to maintain correctly.

## Integration points

| Business Central area | How TMS uses it |
|---|---|
| **Customers** | Ordering party, invoice customer, settlement income documents, customer or Ship-to Address [instruction variants](instructions.md), and **Default FWO Type** for creation from source documents. |
| **Vendors** | Carriers, pay-to vendors, settlement cost documents, and vendor or Order Address **Default FWO Type** for creation from source documents. |
| **Contacts** | Party contact details and operational communication. |
| **Sales documents** | Customer invoices and credit memos created from settlement. Sales Orders can also hold instruction variants that override Customer and Ship-to defaults before an FWO snapshot is created. |
| **Purchase documents** | Vendor costs and carrier invoice support. |
| **Posted documents** | Audit trail for invoicing, credit memos, and cost posting. |
| **Permission sets** | Controls access to TMS pages and API entities. |

## Transport and delivery menu actions

The legacy Shipper menu actions are hidden in the standard Business Central pages extended by TMS for Logistics Service Providers. This includes **Create Transport Request**, **Transport Requests**, **Transport Request Planning**, **Create Delivery Order**, **Delivery Orders**, and **Update Transportation Status**, together with their promoted action links.

Standard Business Central Role Centers also hide the legacy **Transport Requests**, **Delivery Orders**, **Scheduler**, **Load Management**, and **Posted Deliveries** entries. Forwarding Order, Freight Order, Fulfillment Overview, and Freight Load Management actions remain available.

This step changes menu visibility only. Existing documents and processing remain in place. **Transportation Documents** field drilldowns and access through search, direct links, and APIs are outside this change.

## Open assigned Freight Orders from a Sales Order

On a **Sales Order**, open **TMS**. **Forwarding Orders** appears directly in this menu, immediately before **Assigned Freight Orders**.

Choose **Assigned Freight Orders** to open the unposted [Freight Orders](freightorder.md) assigned to the stages of the Sales Order's linked Forwarding Orders. If there is exactly one matching Freight Order, its card opens directly. If there are several, a filtered list opens. Assignments across all linked Forwarding Orders are included, and each Freight Order counts once even when it serves several stages or Forwarding Orders.

The action is disabled when no linked unposted Freight Orders exist. Posted Freight Orders are not included. Use **Forwarding Orders** to review the related customer orders and their stage assignments.

## Default FWO Type from source documents

Set **Default FWO Type** on a Customer or Vendor card to suggest its usual transportation process. The same field on a Ship-to Address or Order Address card overrides that party's default for the selected address.

Sales and purchase documents store the suggestion in **FWO Type**. Select or change the customer/vendor or address to populate it, then adjust the type manually on the document if needed. Sales use **Sell-to Customer No.** and **Ship-to Code**; purchases use **Buy-from Vendor No.** and **Order Address Code**. Bill-to Customer and Pay-to Vendor do not determine the suggestion.

The address is looked up for the document's operational customer or vendor. A blank address code, a missing address record, or a blank address default falls back to the party default. If that is also blank, **FWO Type** is cleared. Manually changing address text does not select a different type.

On unposted sales and purchase documents, changing the customer/vendor or selected address refreshes **FWO Type**, including after an FWO is linked. Other edits preserve a manual choice. Changing defaults on the master-data cards does not recalculate existing documents. The FWO creation action reads the stored document field before creating the new order, so the selected type controls its numbering, stages, statuses, and other process defaults from the start.

| Source used to create the FWO | Type used |
|---|---|
| Sales Order, Sales Return Order, sales creation as LSP, and combined shipments | Stored **FWO Type** on the sales document; if blank, existing source-specific Setup/selection behavior. |
| Posted Sales Shipment and Posted Return Receipt | **FWO Type** copied from the source sales document during posting; if blank, **Default Sales FWO Type** in TMS Setup. |
| Purchase Order and Purchase Return Order | Stored **FWO Type** on the purchase document; if blank, **Default Purchase FWO Type** in TMS Setup. |
| Posted Purchase Receipt and Posted Return Shipment | **FWO Type** copied from the source purchase document during posting; if blank, **Default Purchase FWO Type** in TMS Setup. |
| Inventory Document | Current **Default FWO Type** on the document's customer → **Default Sales FWO Type** in TMS Setup. This source has no new document-level FWO Type or address default. |

Sales and purchase posting preserves the stored field in shipments, receipts, invoices, and credit memos. It is also preserved in archived headers for archive/restore workflows. Posted document cards show the copied type as read-only. Creation from posted documents uses that stored value rather than current customer or address defaults.

Documents created before this field was added remain blank; no automatic update is performed. Set the type manually on an editable source document or change its customer/vendor/address to apply the defaults. A blank field uses the existing source-specific fallback. On unposted sales and purchase documents, **FWO Type** remains an editable default even after an FWO is linked or posted; changing it does not change any existing FWO. Posted document fields remain read-only. Reopening the linked FWO keeps its original type.

Return documents use the same defaulting rules and support a manual type on the source header. The selected type's stage profile is not automatically reversed; choose a type suitable for the return process when needed.

When creating an FWO from a Purchase Return Order, the company is the shipper, the header **Location Code** is the load address, and the **Buy-from Vendor** is the consignee. The document's **Ship-to** details become the unload address, including an alternate vendor address or a custom address. A vendor or Order Address map location is retained only when its address matches the document; otherwise the delivery map location is cleared for planning.

For this return flow, **Expected Receipt Date** supplies the requested and planned pickup date. Delivery dates remain blank until planned in the FWO. Item lines with negative quantities represent goods moving in the opposite direction; use **Move Negative Lines** before creating the outbound FWO.

Transfers, manual FWO creation, the FWO API, and order wizards keep their existing type selection. For the blank-field fallback, see [TMS Setup](setup.md#default-forwarding-order-type-by-customer-vendor-and-address).

## Copy sales and purchase documents

Use the standard **Copy Document** action to reuse sales or purchase document details. After copying a header, its Forwarding Order number, status, posting date, posted version where applicable, and related FWO/FO lists are cleared. This applies to all source document types offered by the action, including posted and archived documents.

The header references are cleared even if the destination had them before copying. This does not delete or reassign existing FWO, FO or Settlement records. Copying only lines leaves the destination's FWO/FO header references unchanged. **Recalculate Lines** does not change this rule, and **FWO Type** keeps its normal behavior as a default for creating a new FWO.

With **Include Header** selected, copying also replaces the destination's entire set of document instructions, including manual and automatically added instructions. Stored instructions from an unposted source are copied with their complete rich text, local changes, grouping and applicability. If the source has no instructions, the destination is left without instructions. Copying only lines preserves the destination's instructions.

Posted and archived documents currently have no saved document-instruction snapshot. Copying their header clears the destination's instructions; instructions from a current open document with the same number are not substituted. See [Copy a Sales Order with instructions](instructions.md#copy-a-sales-order-with-instructions).

**Correct**, **Cancel**, and **Create Corrective Credit Memo** continue to manage their own correction operations and Settlement relationships. Those deliberate financial relationships are separate from copied header references; see [Invoice corrections](invoice-corrections.md).

Documents copied before this change are not updated automatically.

## Load logistic units from source documents

Use **Load Logistic Units to Freight Order** from a **Sales Order**, **Sales Return Order**, **Purchase Order**, **Purchase Return Order**, **Transfer Order**, **Warehouse Shipment**, or **Warehouse Receipt** to assign the document's logistic units to an existing [Freight Order](freightorder.md).

1. Select the logistic units to load.
2. If more than one Forwarding Order matches all selected units, choose the required Forwarding Order.
3. Select an available Freight Order assigned to that Forwarding Order's stages.

The action adds missing assignments for every selected unit on **all stages of the selected Forwarding Order in the selected Freight Order**. Existing assignments on those stages are kept without duplication. Stages belonging to other Forwarding Orders in a shared Freight Order are unchanged.

For example, a Forwarding Order can have **Precarriage** in one Freight Order and **Main carriage** plus **Delivery** in another. Load the container into the first Freight Order, then run the action again and select the second. The container is assigned to both stages of the second Freight Order. A Freight Order with partially assigned units remains available when the action can complete its missing assignments.

A unit can travel on successive stages, but the same leg cannot carry that unit in two parallel Freight Orders. This restriction also applies when the other parallel assignment has already been posted. Freight Orders with no remaining eligible assignments are excluded from selection.

Messages distinguish a source document without linked logistic units from a selection with no available Freight Order. Assignment to a Freight Order does not post the source document or the logistic unit. Canceling a selection leaves assignments unchanged.

To undo a loading assignment, open the Freight Order and use **Remove from Freight Order** in **Logistic Units**. Selecting one physical unit removes its assignments from every stage and Forwarding Order in that Freight Order, including rows hidden by filters. Review the confirmation before continuing. The unit and its source-document links remain available, and assignments to other Freight Orders are retained. See [Remove cargo from a Freight Order](freightorder.md#remove-cargo-from-a-freight-order) for selection rules and planned reservations.

## Recommended setup sequence

1. Confirm customers and vendors are clean.
2. Create carriers and link vendors where needed.
3. Configure services for customer billing.
4. Configure charges for vendor and carrier costs.
5. Configure settlement posting and document controls in [TMS Setup](setup.md).
6. Test one Forwarding Order from draft to invoice and posted history.

## Good to know

- TMS does not replace Business Central financial setup.
- Posting behavior depends on standard Business Central customer, vendor, posting group, currency, and tax setup.
- Permission issues often come from a missing Business Central permission set, not from TMS setup alone.

## Troubleshooting

| Problem | What to check |
|---|---|
| Invoice cannot be created | Check customer, posting groups, service mapping, currency, and settlement status. |
| Cost document cannot be created | Check vendor, charge mapping, posting groups, and purchase permissions. |
| User cannot open a TMS page | Check license, Business Central user setup, and TMS permission sets. |
| Posted document is not linked | Review settlement line references and posting result messages. |
| Assigned Freight Orders is disabled on a Sales Order | Check that a linked Forwarding Order has a stage assigned to an unposted Freight Order. Posted Freight Orders are excluded. Refresh the Sales Order after changing assignments on another page. |

## Related

- [Settlement](settlement.md)
- [Services](services.md)
- [Charges](charges.md)
- [Assign Permission Sets](assignpermissionsets.md)
- [Instructions](instructions.md)
- [Public API](api.md)
