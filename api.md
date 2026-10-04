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

### Settlement document amounts and P/L

The v1.0 `settlements` and `postedsettlements` entity sets expose separate document and financial amounts:

| Property | Meaning |
|---|---|
| `amount` | Document amount **before line discount**, retaining the source sign. |
| `lineDiscountPercent`, `lineDiscountAmount` | Line discount percentage and exact amount in the transaction currency. |
| `lineAmount` | `amount − lineDiscountAmount`, before invoice discount. |
| `invoiceDiscountAmount` | Exact invoice discount allocated to this line or Settlement share. |
| `netAmountExclVAT` | Document amount excluding VAT after all discounts, before the P/L financial direction. |
| `amountIncludingVAT` | Exact linked document total including VAT for this share. An unlinked plan uses its Line Amount without estimating VAT. |
| `quantityBase` | Exact document quantity in the base unit of measure for this Settlement share. |
| `amountLCY`, `lineAmountLCY` | Gross Amount and Line Amount converted to LCY; these do not apply Income/Cost financial direction. |
| `plAmount` | Net amount excluding VAT after applicable discounts, with Income/Cost and Standard/Credit direction, in the transaction currency. |
| `plAmountLCY` | Signed financial contribution used for Forwarding Order profitability across currencies. |
| `plAmountACY` | `plAmountLCY` converted to the TMS additional reporting currency; equals `plAmountLCY` when none is configured. |
| `postedSourceDeleted` | Read-only flag on active and posted Settlement: deletion of the linked posted invoice, credit memo, or source line was recorded. Stored financial amounts and document references are retained. |
| `amountModelVersion` | `2` for canonical document and P/L fields; `0` and `1` require the manual conversion wizard. |

For a normal purchase line with quantity 1, price 100, and a 20% discount, `amount` is 100, `lineAmount` is 80, and `plAmount` is −80. A positive vendor credit uses the same document values with positive P/L. Negative source quantities and amounts retain their signs.

The optional **TMS Setup > Make Settlement Input Positive** setting is off by default. When enabled, new negative inputs to editable `quantity`, `price` and `amount` fields are converted to positive values. Use the normalized values returned in the API response; web-service sessions do not show the interactive explanation Message. The option does not change P/L signs or synchronized document values, and unrelated edits do not normalize an existing negative value. See [Settlement input signs](setup.md#settlement-input-signs).

**Integration change:** for converted rows, `amount`, `amountLCY`, and `additionalCurrencyAmount` now describe document values before discount. Existing profitability consumers must use `plAmount`, `plAmountLCY`, and `plAmountACY`. Adding the new fields does not automatically update existing reports. Do not combine old and new `amount` semantics: use `amountModelVersion` to detect incomplete migration. P/L fields and order totals are incomplete while version-0 or version-1 rows remain.

Document links, base quantity, invoice discount, net/VAT totals, and calculated P/L fields are read-only. Active API writes to discounts follow the same editable-draft rules as the page; linked values come from the financial source. Posted Settlement is read-only. Existing rows are converted through the [manual Settlement wizard](setup.md#upgrade-existing-settlement-amounts), not an API request or an automatic amount-model upgrade.

### Order instructions

The `forwardingorders`, `freightorders`, `postedforwardingorders`, and `postedfreightorders` APIs expose read-only instruction collections. The navigation property has the same name as the corresponding instruction entity set listed below. Request it with `$expand` or navigate to that collection for a selected order.

For example, relative to the `extensionsforce/tms/v1.0` API company path:

```http
GET forwardingorders(<order-guid>)?$expand=forwardingorderinstructions
GET freightorders(<order-guid>)/freightorderinstructions
GET postedforwardingorders(<posted-order-guid>)/postedforwardingorderinstructions
GET postedfreightorders(<posted-order-guid>)?$expand=postedfreightorderinstructions
```

Instructions are also available through dedicated read-only entity sets:

| Entity set | Owner fields |
|---|---|
| `forwardingorderinstructions` | `forwardingOrderNo` |
| `freightorderinstructions` | `freightOrderNo` |
| `postedforwardingorderinstructions` | `forwardingOrderNo`, `postedVersion` |
| `postedfreightorderinstructions` | `freightOrderNo`, `postedVersion` |

Use `id` to identify an instruction record. Freight Order instructions also expose `sourceForwardingOrderNo`: several customer jobs can supply different variants of the same `instructionCode`. A blank source identifies the Freight Order's own instruction.

Each record includes the saved description, type code, Comment Group, applicability flags, customization/provenance information, `contentHtml`, and `contentText`. Active instruction type descriptions come from the current type master; posted instructions retain the archived type description. `contentText` is a plain-text conversion for integrations and LLMs; it preserves supported textual structure but does not recognize text inside images. Use `contentHtml` when the original formatting or embedded content is needed. Comment Group is classification, not an access-control filter.

The nested collection automatically selects the correct order and, for posted records, its exact version. When reading a posted entity set directly, filter by both owner fields. A Posted Freight Order can have version `0`; its version is independent of the related Forwarding Order version.

Reads return saved document copies without regenerating instructions from current master data. The APIs reject create, update, and delete operations. Existing instruction-table permissions are inherited by the TMS API roles; MCP operations still depend on server configuration and the integration user's permissions.

### Planning and operational fields

The APIs include selected operational fields from current TMS master data, orders, cargo lines, and posted history:

| API area | Useful fields and meaning |
|---|---|
| Drivers, vehicles, carriers | `blockForScheduling` and `schedulerSortOrder` describe scheduler visibility and order. Vehicles include `defaultMapLocationCode`; carriers include `note` and the read-only `freightOrderCount`. These fields do not establish availability for a particular date. |
| Logistic Units | `comment` contains the unit-specific note, up to 250 characters; posted units preserve its historical value as read-only. `trackingNo`, `seals`, `shippingAgentCode`, and `reusable` identify tracking, security, and reuse information. Read-only warehouse/logistics location fields describe stored placement. Posted units also expose `parentUnitPostedVersion` for exact historical nesting. |
| Logistic Unit lines | `qtyPerUnitOfMeasure`, `inventoryQuantityBase`, and `expectedQuantityBase` distinguish packing quantities from signed posted and projected document-link balances. Expected quantity includes both posted and unposted calculation links. Location fields are read-only. |
| Forwarding Orders | Order and party notes, the selected customer agreement, route, estimated loading length, planned reservations, and `uninvoicedAmountLCY`. That amount combines signed income and cost settlement without a financial document number; it is not an unpaid customer balance. |
| Freight Orders | Loading/unloading dates, times, windows, bays and priority; booking/flight references; responsibility center and route; stored capacity and temperature context. Current FO APIs also expose reserved capacity and current charge amounts. |
| Forwarding Order cargo lines | `gtin`, the read-only source `itemReferenceNo`, `qtyPerUnitOfMeasure`, and `linearUnitOfMeasureCode`. Changing the linear unit does not convert the numeric dimensions; it reinterprets them and recalculates volume. |
| Order stages | `stageProfileCode`, `transportRegulationCode`, `transportProfileCode`, `vehicleUnitTypeCode`, `packageTypeCode`, and relevant reservation/status-profile context. These new stage fields are read-only through the API. |

Posted APIs expose stored counterparts only where the source tables preserve that information. Posted Freight Orders do not expose the live-cost or cross-version reservation FlowFields as historical totals. A transport-profile code in history is a saved reference, not a frozen copy of the entire current master profile.

Computed values, historical data, workflow-managed locations, and selected customer agreements are read-only. The new stage transport selectors are read-only because the current API insertion path does not perform the full equipment validation used by the planning workflow. Use the supported business processes to change these values. All added fields include AboutTitle/AboutText explaining their source, units, relationships, and limits.

### Transport Profiles and Vehicle Unit Types

The Stage API exposes `transportProfileCode` and `vehicleUnitTypeCode`. Resolve these references through the following read-only APIs under `extensionsforce/tms/v1.0`:

| Entity set | Purpose and business key |
|---|---|
| `transportprofiles` | Current Transport Profile master data. Match `code` to the stage `transportProfileCode`. |
| `vehicleunittypes` | Shared Logistic Units type master used for equipment and packaging. Match `code` to the stage or Freight Order `vehicleUnitTypeCode`, or a vehicle's `unitTypeCode`. |
| `transportprofiledetails` | Repeating facts and structured conditions, linked by `profileCode`. The business key includes `profileCode`, `detailType` and `lineNo`. |
| `profileunittyperules` | Profile-specific equipment rules, linked by `profileCode` and `vehicleUnitTypeCode`. |

Every record uses `id` (SystemId) as its OData key. Transport Profiles expose their classification, SDS references, transport context, required temperatures with explicit Defined flags, and handling requirements. They describe complete scenarios, including nonregulated or conditional cargo; they are not simply a list of hazard classes.

Profiles contain `transportprofiledetails` and `profileunittyperules` navigation collections. Vehicle Unit Types also expose `profileunittyperules`, filtered to that equipment type. For example, relative to the API company path:

```http
GET transportprofiles?$filter=code eq 'PROFILE-CODE'&$expand=transportprofiledetails,profileunittyperules
GET vehicleunittypes?$filter=code eq 'TYPE-CODE'&$expand=profileunittyperules
```

The Vehicle Unit Type API returns stored dimensions and units, capacities, strict-control flags, loading characteristics, reuse and legacy TMS attribute information. The underlying dictionary also contains packaging types; it has no separate vehicle-only filter or Blocked field. Type membership is not evidence that equipment is available or suitable for a shipment.

These APIs return current definitions, including blocked or unreviewed profiles for traceability. A profile reference from a posted stage does not reconstruct a historical master definition. Inspect `blocked`, `transportDataReviewed`, the full profile context, details and equipment rules when interpreting a reference.

Temperature zero is meaningful only when its corresponding Defined flag is set. A profile with no equipment rules imposes no restriction through that rule list; once any rule exists, an unlisted type is excluded. Conditional rules require shipment context and, where specified, explicit confirmation. Reading or expanding the records does not execute the TMS compatibility checks, approve classification, or assign equipment.

Create, update and delete operations are disabled. The current and legacy TMS API permission sets include execution of the new pages; existing transport-planner permissions supply the profile-table reads. Reading `vehicleunittypes` additionally requires Unit Type read permission from the Logistic Units app, for example **TMAC User** (caption **Logistic Units - User**) or an equivalent custom role. TMS does not redefine the dependency's licensing entitlements.

### Transport Regulations and Package Types

Use these read-only entity sets under `extensionsforce/tms/v1.0` to resolve `transportRegulationCode` and `packageTypeCode` from `stages`, `postedstages`, `postedfreightstages`, and `transportprofiles`. Match each reference to the corresponding dictionary's `code`.

| Entity set | Fields |
|---|---|
| `transportregulations` | `id`, `code`, `description`, `edition`, `active`, `notes` |
| `packagetypes` | `id`, `code`, `description` |

Both APIs use `id` (SystemId) as the OData key and expose `systemCreatedAt`, `systemCreatedBy`, `systemModifiedAt`, and `systemModifiedBy`. Create, update, and delete operations are disabled.

Transport Regulations include inactive records so that older references remain resolvable when those dictionary records still exist. To request only active regulations, apply the optional filter explicitly:

```http
GET transportregulations?$filter=active eq true
GET packagetypes?$filter=code eq 'PACKAGE-CODE'
```

Package Types are the separate TMS dictionary for the cargo's packaging context; `vehicleunittypes` describes Logistic Units equipment and unit types. Resolve `packageTypeCode` against `packagetypes`, even if an equipment or unit type happens to use the same code. These APIs return current dictionary values, not the definitions as they were when a stage was posted.

### General API behavior

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
