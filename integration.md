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

## Default FWO Type from source documents

Set **Default FWO Type** on a Customer or Vendor card to suggest its usual transportation process. The same field on a Ship-to Address or Order Address card overrides that party's default for the selected address.

Sales and purchase documents store the suggestion in **FWO Type**. Select or change the customer/vendor or address to populate it, then adjust the type manually on the document if needed. Sales use **Sell-to Customer No.** and **Ship-to Code**; purchases use **Buy-from Vendor No.** and **Order Address Code**. Bill-to Customer and Pay-to Vendor do not determine the suggestion.

The address is looked up for the document's operational customer or vendor. A blank address code, a missing address record, or a blank address default falls back to the party default. If that is also blank, **FWO Type** is cleared. Manually changing address text does not select a different type.

Before an FWO is linked, changing the customer/vendor or selected address refreshes **FWO Type**. Other edits preserve a manual choice. Changing defaults on the master-data cards does not recalculate existing documents. The FWO creation action reads the stored document field before creating the new order, so the selected type controls its numbering, stages, statuses, and other process defaults from the start.

| Source used to create the FWO | Type used |
|---|---|
| Sales Order, Sales Return Order, sales creation as LSP, and combined shipments | Stored **FWO Type** on the sales document; if blank, existing source-specific Setup/selection behavior. |
| Posted Sales Shipment and Posted Return Receipt | **FWO Type** copied from the source sales document during posting; if blank, **Default Sales FWO Type** in TMS Setup. |
| Purchase Order and Purchase Return Order | Stored **FWO Type** on the purchase document; if blank, **Default Purchase FWO Type** in TMS Setup. |
| Posted Purchase Receipt and Posted Return Shipment | **FWO Type** copied from the source purchase document during posting; if blank, **Default Purchase FWO Type** in TMS Setup. |
| Inventory Document | Current **Default FWO Type** on the document's customer → **Default Sales FWO Type** in TMS Setup. This source has no new document-level FWO Type or address default. |

Sales and purchase posting preserves the stored field in shipments, receipts, invoices, and credit memos. It is also preserved in archived headers for archive/restore workflows. Posted document cards show the copied type as read-only. Creation from posted documents uses that stored value rather than current customer or address defaults.

Documents created before this field was added remain blank; no automatic update is performed. Set the type manually on an editable source document or change its customer/vendor/address to apply the defaults. A blank field uses the existing source-specific fallback. Once an FWO is linked, **FWO Type** is read-only on the source document and automatic suggestions stop changing it. This also applies after FWO posting. The check includes links stored in the header and in working or posted FWO source lines, so purchases are locked even when the header link is blank. Reopening the linked FWO keeps its original type.

Return documents use the same defaulting rules and support a manual type on the source header. The selected type's stage profile is not automatically reversed; choose a type suitable for the return process when needed.

Transfers, manual FWO creation, the FWO API, and order wizards keep their existing type selection. For the blank-field fallback, see [TMS Setup](setup.md#default-forwarding-order-type-by-customer-vendor-and-address).

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

## Related

- [Settlement](settlement.md)
- [Services](services.md)
- [Charges](charges.md)
- [Assign Permission Sets](assignpermissionsets.md)
- [Instructions](instructions.md)
- [Public API](api.md)
