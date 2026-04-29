---
title: "Public API"
description: "Use the TMS API to connect Forwarding Orders, Freight Orders, settlement, posted history, portal records, and reports to external systems."
---

# Public API

Use the **TMS API** when another system needs to read or update TMS data without opening the Business Central web client.

Typical consumers include customer portals, partner portals, Power BI, middleware, mobile apps, and finance integrations.

## Before you start

Make sure that:

- the integration user exists in Business Central,
- the user has the required TMS permission sets,
- the API is enabled for the target environment,
- the integration uses the correct company,
- field mapping and validation rules are agreed before write operations are enabled.

## Main API areas

| Area | Use it for |
|---|---|
| **Master data** | Carriers, drivers, vehicles, customers, contacts, map locations, and user setup. |
| **Forwarding workflow** | Forwarding Orders, content, stages, settlement, attachments, and status data. |
| **Freight workflow** | Freight Orders, freight lines, stage entries, carrier charges, and documents. |
| **Logistic units** | Logistic units, unit lines, posted unit records, and cargo structure. |
| **Posted history** | Posted Forwarding Orders, posted Freight Orders, posted settlement, and posted charges. |
| **Portal** | Customer portal tickets and notifications. |
| **Documents** | Attachments and generated PDF documents such as posted customer invoices or credit memos. |

## Recommended integration pattern

1. Create a dedicated integration user.
2. Assign only the permission sets the integration needs.
3. Start with read-only calls.
4. Confirm keys, filters, and company selection.
5. Add write operations only after validation is tested.
6. Store external document numbers in TMS reference fields.
7. Log every integration request outside Business Central if the external system owns retries.

## Common scenarios

| Scenario | Typical flow |
|---|---|
| Customer portal creates a job | Create or update a Forwarding Order draft, then notify operations. |
| Portal displays job status | Read Forwarding Order, Freight Order, and posted history status fields. |
| Power BI reports profitability | Read Forwarding Orders, settlement lines, costs, and posted history. |
| Operations dashboard tracks execution | Read Freight Orders and Stage Entries by date, carrier, status, or stage. |
| External document service stores files | Upload attachments and link them to the correct TMS document. |
| Finance system consumes invoices | Read settlement and posted customer document references. |

## Good to know

- API behavior follows Business Central company, user, and permission rules.
- Do not use a personal user account for integrations.
- Keep external IDs in dedicated reference fields so users can trace records across systems.
- Treat write operations as business actions, not database imports.

## Troubleshooting

| Problem | What to check |
|---|---|
| API returns no records | Check company, filters, user permissions, and whether records exist in that company. |
| API returns permission errors | Assign the correct TMS permission sets and verify the Business Central license. |
| Create or update fails | Check required fields, status rules, and document type setup. |
| Portal status looks stale | Confirm which document owns the status: Forwarding Order, Freight Order, or posted history. |

## Related

- [Assign Permission Sets](assignpermissionsets.md)
- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Settlement](settlement.md)
- [Customer Portal Entities](customerportal.md)
