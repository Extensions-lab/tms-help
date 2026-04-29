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
| **Customers** | Ordering party, invoice customer, and settlement income documents. |
| **Vendors** | Carriers, pay-to vendors, and settlement cost documents. |
| **Contacts** | Party contact details and operational communication. |
| **Sales documents** | Customer invoices and credit memos created from settlement. |
| **Purchase documents** | Vendor costs and carrier invoice support. |
| **Posted documents** | Audit trail for invoicing, credit memos, and cost posting. |
| **Permission sets** | Controls access to TMS pages and API entities. |

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
- [Public API](api.md)
