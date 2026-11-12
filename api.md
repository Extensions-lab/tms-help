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

## Posting and history

Post Freight Orders before their related Forwarding Orders. Each Freight Order produces one complete historical trip, including all of its stages, content, Charges, instructions, and Freight Order attachments. A Forwarding Order cannot be posted until every Freight Order assigned to its stages has a valid posted link. The Freight Order's linked purchases and charge allocation must be completed before its own posting; the Forwarding Order can finish its remaining customer work afterward.

Active stages expose read-only `postedFreightOrderId` and `postedFreightOrderStageId` references to their exact history. After Freight Order posting, use these references rather than looking for an active Freight Order by number. Posting a Forwarding Order links its exact history to the existing posted freight stages; it does not create another Posted Freight Order or transfer that Freight Order's attachments again. The Forwarding Order's own attachments are transferred by its own posting.

On `postedfreightstages`, `postedFOSystemID` identifies the trip, while `postedFWOSystemID` and `postedFWOVersion` identify the forwarding history linked later. A blank `postedFWOSystemID` means that the related Forwarding Order remains active. New whole-trip Posted Freight Orders have a blank `forwardingOrderNo`; read their stages for all related jobs. Older per-FWO freight snapshots keep their saved owner and version.

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

### Creating a Forwarding Order

Resolve `forwardingOrderTypeCode` through the read-only `forwardingordertypes`
catalogue described below before creating a job.

Send `POST forwardingorders` with `forwardingOrderTypeCode`. Omit `no` to use the
type's number series and omit `id` to use the generated identity:

```json
{
  "forwardingOrderTypeCode": "FWO",
  "externalTrackingNumber": "API-DEMO-001"
}
```

The API page controls creation in one transaction using the same lifecycle as
the FWO card: insert the header with its type defaults, initialize related
Settlement and instructions, then apply explicitly supplied values in dependency
order. The requested status is reapplied last, with its normal requirements.
This replay does not bypass the ordinary validation that already runs while
reading the request. Restrictions of the type's initial status still apply.

Existing Settlement follows the card's normal update rules, rather than being
rebuilt after all request values have been applied. Eligible unlinked Income
receives a changed customer, and editable drafts receive dimension changes;
protected rows keep their dimensions. Already populated Posting Date and
Exchange Rate Date remain unchanged, so changing the header's date does not
force the same date onto generated rows. A new customer's default agreement
does not replace existing Income. If there is no Income, normal customer
validation can generate that agreement's rows. Consignee/address validation
refreshes automatic instructions normally.

An omitted property keeps its default. An explicit empty property clears the
value where business rules allow. Customer/address defaults are applied before
explicit address overrides and shortcut dimensions; field validation can still
normalize values and reject conflicting inputs. Default cargo from the type is
created once; adding nested cargo does not replace it.

The original fields remain bound to the table, preserving GET filters and
ordering. Table field `OnValidate` and TableRelation checks run normally, before
the page captures each supplied value. Send the type before an explicit `no`,
party type before party number, party number before address code, and address
code before map location. Include the status profile when supplying a status.
Set the fields required by that status after their controlling/defaulting fields
and send the status last. An early status that requires missing values is rejected,
not deferred. If a status requires saved children, omit it from POST and set it
with a subsequent PATCH after creation. Replay order does not make arbitrary
JSON property order valid.

An explicit `no` requires manual numbering to be allowed. A duplicate number is
an error, not an update or a silently accepted retry. A failed request rolls back
the header and its dependent writes. `externalTrackingNumber` is a reference,
not an idempotency key. Store the returned `id` for GET/PATCH; creating the order
does not post it. Existing PATCH behavior is unchanged.

### Importing a Sales Order into Settlement

Use `importSalesOrder` on an existing Forwarding Order to import the lines of an
existing unposted Sales Order as new linked Income lines. The action uses the
same eligibility and mapping rules as **Import to Settlement** → **Import Sales
Document** → **Sales Order** on the Forwarding Order.

Send the Sales Order's `SystemId` as `salesOrderId`. The URL identifies the target
FWO by its `id` (SystemId); both records must exist in the selected company:

```http
POST /api/extensionsforce/tms/v1.0/companies(<companyId>)/forwardingorders(<fwoId>)/Microsoft.NAV.importSalesOrder
Content-Type: application/json

{
  "salesOrderId": "<salesOrderId>"
}
```

Replace all placeholders with GUIDs. The request accepts a Sales Order, not a
Sales Invoice or a posted document. A successful request returns **204 No
Content**. Read the resulting lines through the existing collection, relative
to the same company API path:

```http
GET forwardingorders(<fwoId>)/settlements
```

The imported lines have `type = Income`, `documentType = Order`, the source
`documentNo`, and the corresponding `documentLineNo`. The action creates these
links; those fields remain read-only through the Settlement API.

All non-comment source lines are checked before import. The configured **Sales
Document Linking Mode**, Service mappings, source-processing restrictions, and
existing Settlement links apply exactly as in the interactive import. A failed
check leaves no partial import. Repeating a successful import returns an error
because the source lines are already linked, including links in archived
Settlement. After an uncertain HTTP result, read Settlement before retrying.

The action does not approve, release, reopen, or post the Sales Order. If your
process requires approval before import, complete that workflow before calling
the action. The import preserves the source customer, currency, and financial
values and follows the existing synchronization rules for linked documents.

Existing planned Income remains: the action adds new rows rather than matching
or replacing plans. Review Income generated by the FWO type, Service Set, or
agreement to avoid counting the same income twice. See [Import an existing sales
document](settlement.md#import-an-existing-sales-document) for the full import
rules.

### Reading TMS Setup

Read the selected company's saved TMS configuration through `tmssetups` under
`extensionsforce/tms/v1.0`. For example, relative to the company API path:

```http
GET tmssetups
GET tmssetups(<id>)
GET tmssetups?$select=id,primaryKey,statusControl,systemModifiedAt
```

The collection contains at most one record: the canonical TMS Setup row with
`primaryKey` equal to `0`. Use its `id` (SystemId) for direct addressing. If setup
has not been created, the collection is empty; reading the API does not initialize
setup, run a wizard or create defaults.

The API exposes active configuration fields for order defaults, numbering,
status and posting controls, Settlement, pricing, capacity, maps, attachment
storage, role center filters and emergency-response details. Saved legacy
Transport Request and Delivery settings are also available and identified as
legacy in their field descriptions. `systemModifiedAt` tracks changes to the
setup record, not changes to referenced dictionaries.

`statusControl` is the company-level switch used together with a Status
Profile's `actionControlEnabled`. Read these settings to understand the controls;
they do not establish effective user permissions or validate an action. Storage
account/container names and provider selection describe configured destinations,
not a successful connectivity check.

Credentials are excluded: the response and metadata contain no Azure Storage,
Google Maps, Bing Maps or Azure Maps access keys. The API does not read encrypted
credential storage. Obsolete fields and setup wizard media are also excluded.

The endpoint is **read-only**, including for administrators: POST, PATCH and
DELETE are disabled, and no actions are exposed. Current and legacy TMS API roles
include page execution and inherit the existing TMS Setup read permission.
Change configuration through the normal [TMS Setup](setup.md) workflow.

### Forwarding Order Types, Stage Profiles, and Status Profiles

Use these read-only catalogues under `extensionsforce/tms/v1.0` to resolve the
process settings referenced by Forwarding Orders and their stages:

| Entity set | Purpose and business key |
|---|---|
| `forwardingordertypes` | FWO process templates, numbering and defaults. Match `code` to `forwardingorders.forwardingOrderTypeCode`. |
| `stageprofiles` | Reusable sets of transportation legs, identified by `code`. |
| `stagedefinitions` | Reusable leg definitions and defaults. Identify each by **both** `stageProfileCode` and `code`. |
| `statusprofiles` | Sets of document or execution statuses, identified by `code`, with their action-control setting. |
| `statuses` | Status descriptions, action flags and control settings. Identify each by **both** `statusProfileCode` and `code`. |

Each entity uses `id` (SystemId) for direct API addressing and exposes
`systemModifiedAt`. Codes on stages and statuses can repeat in different profiles.
Read a profile's members with a filter on the separate collection; these APIs do
not add nested navigation collections. For example, relative to the selected
company's API path, using codes configured in that company:

```http
GET forwardingordertypes?$filter=code eq 'FWO'
GET stageprofiles?$filter=code eq 'ROAD'
GET stagedefinitions?$filter=stageProfileCode eq 'ROAD'&$orderby=code
GET statusprofiles?$filter=code eq 'FWO'
GET statuses?$filter=statusProfileCode eq 'FWO'&$orderby=code
GET statuses?$filter=statusProfileCode eq 'FWO' and code eq 'OPEN'
GET forwardingordertypes(<id>)
```

Use the selected FWO type's `stageProfileCode`, `statusProfileCode` and
`defaultStatusCode` to resolve its defaults. Review `financialCorrection` before
choosing a type: a financial correction is a separate financial workflow and does
not create a transportation job with cargo and stages. `descriptionForCopilot`
returns the type's stored explanatory text, including multiple lines; this
calculated property is not a source-table field for filtering or ordering.

`stagedefinitions` describes master data; the existing `stages` endpoint continues
to return actual transportation legs on FWO documents. Resolve an operational
stage's definition using its `stageProfileCode` and `stageCode`. Stage defaults
marked `additional` are available for optional addition rather than normal
automatic generation. Current master definitions do not reconstruct the values
saved on an older document.

For order statuses, use the document's `statusProfileCode` and `statusCode`.
For stage execution, use `stageExecutionStatusProfile` and
`stageExecutionStatusCode`. The catalogue does not list guaranteed valid status
transitions. Action flags depend on `tmssetups.statusControl`, the profile's
`actionControlEnabled`, permissions and the document's business rules. Extended
control can require attachments or field values; its individual rule rows are
not exposed by these catalogues. Reading a status does not execute those checks.

Create, update and delete operations are disabled for all five catalogues. The
current and legacy TMS API permission sets include page execution and inherit
the existing table read permissions. Synchronize child collections separately:
editing a stage or status does not necessarily change its profile's
`systemModifiedAt`.

See [Forwarding Order Types](forwardingordertype.md), [Stages](stages.md), and
[Statuses and Status Profiles](statuses.md) for configuration.

### Choosing a Logistics Unit Type for Forwarding Order cargo

Use the read-only `logisticsunittypes` entity set under
`extensionsforce/tms/v1.0` to find types of cargo units that the company transports,
such as pallets, boxes, crates, or containers. On a Forwarding Order content line,
set `type` to `Unit Type` and use the returned `code` as `no`.

For example, relative to the selected company's API path:

```http
GET logisticsunittypes?$filter=code eq 'PALLET'
GET logisticsunittypes(<id>)
```

`id` identifies the type record; `code` connects it to the cargo line.
`description` and `description2` explain the cargo unit type. Dimensions, weight,
volume, handling settings, and their units describe the stored type specification;
the actual shipment's quantities and measurements belong on its content lines.
Use `systemModifiedAt` to track changes to the master record.

Selecting a type copies both descriptions, measurement units, external dimensions
and `unitVolume` to the FWO line, and initializes its quantity to one. Enter the
shipment quantity and gross/net weights afterward; the type's tare, payload and
filling limits are not copied as cargo weights.

The endpoint currently returns the shared Logistic Units type catalogue without a
cargo-specific filter. Choosing a suitable type remains part of cargo preparation.
The separate `vehicleunittypes` endpoint continues to describe equipment used on
Stages and Freight Orders, including its Transport Profile rules.
`logisticsunittypes` does not expose that equipment-rule navigation or allow
creating, editing, or deleting types.

The current and legacy TMS API permission sets include execution of this page.
Reading the shared master also requires Unit Type read permission from the
Logistic Units app, such as **TMAC User** or an equivalent custom role.

See [Logistic Unit Types](logisticunittype.md) for master-data maintenance.

### Creating a Forwarding Order line

Send `POST forwardingorderlines` with the existing parent's `forwardingOrderNo`
and an unused `lineNo`. Both business keys must be supplied; this endpoint does
not generate line numbers or resolve the parent number from `fwoSystemID` alone.
For example, with an existing order and a configured `PALLET` unit type resolved
through `logisticsunittypes`:

```json
{
  "forwardingOrderNo": "FWO-DEMO-001",
  "lineNo": 10000,
  "type": "Unit Type",
  "no": "PALLET",
  "quantity": 2,
  "grossWeightPerUoM": 10,
  "netWeightPerUoM": 8,
  "volumePerUoM": 0.5
}
```

Omit `id` and `fwoSystemID`. Standard insertion runs once, generates the line
identity, sets the parent GUID from `forwardingOrderNo`, defaults missing
weight/volume units and applies the parent's type-specific volume-weight
multiplier. Explicit measurement units are retained. Table weight checks and the
restriction on adding cargo to financial corrections apply.

Normal field validation still runs as the payload is read: send `type` before
`no`, then quantity and measurement units before explicit weights and volume.
An existing parent/line-number combination is an error, not an update or a retry
that silently succeeds. Store the returned `id` for GET/PATCH.

When creating through a parent's `lines` navigation route, also supply
`forwardingOrderNo` and `lineNo`. The parent GUID link alone does not replace these
business keys.

### Creating a Freight Order

Send `POST freightorders` with `freightOrderTypeCode` and the order data. Omit `no`
to use that type's number series and normally omit `id` to let Business Central
generate the identity. If the type is omitted, the default FO type in TMS Setup
is required. For example, with a configured `FO` type:

```json
{
  "freightOrderTypeCode": "FO",
  "externalTrackingNumber": "API-FO-DEMO-001"
}
```

The standard table insertion runs once. It initializes the document/loading dates
and windows, assigns the order number, then applies Bill of Lading numbering and
checks the carrier, vehicle, driver and agreement. A supplied `billOfLadingNo` is
kept. Otherwise, the BOL numbering settings can leave it blank, generate it from a
prefix plus the final FO number, or use a separate number series.

Normal field validation applies as the payload is read. Send the type before
explicit status overrides, the status profile before the status, and the carrier
before dependent vehicle, driver or agreement values. Carrier and vehicle defaults
can fill dependent fields.

An explicit `no` requires manual numbering to be allowed. An existing number causes
an error rather than updating that order. A failed insert rolls back the record;
repeating a successful POST is a new creation request. Creation does not assign
Forwarding Order stages or post the Freight Order. Store the returned `id` for
subsequent GET/PATCH operations.

### Map Location positions

The v1.0 `maplocations` API exposes `latitude`, `longitude`, and the read-only `positionIsSet`. Use that status to determine whether the pair is confirmed for map and route use. A zero component, or an intentional pair of `0,0`, is valid; numbers alone do not establish confirmation.

Latitude must be between -90 and 90, and longitude between -180 and 180. The API currently writes draft coordinates; it does not expose position confirmation or clearing actions. After checking both values on the **Map Location** card, choose **Confirm Position**. Use **Clear Position** on that card to remove the pair and its confirmation.

**Integration change:** the existing endpoint and writable coordinate properties remain available. A POST or PATCH that changes a coordinate writes a draft pair and clears confirmation; it does not make the location ready for maps or routes. Integrations must read `positionIsSet` when checking readiness and allow the draft pair to be reviewed and confirmed on the card. Do not attempt to PATCH this read-only status.

See [Map Locations](maplocation.md#confirming-a-position) for manual confirmation and airport reference data.

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

### GUID links between related records

Nested API collections use the parent record's GUID. The child record retains its own GUID key and exposes a separate owner reference:

| Child entity set | Owner GUID property |
|---|---|
| `forwardingorderinstructions` | `forwardingOrderId` |
| `freightorderinstructions` | `freightOrderId` |
| `postedforwardingorderinstructions` | `postedForwardingOrderId` |
| `postedfreightorderinstructions` | `postedFreightOrderId` |
| `transportprofiledetails` | `transportProfileId` |
| `profileunittyperules` | `transportProfileId`, `vehicleUnitTypeId` |
| `attachments` | `parentId`, interpreted with `tableID` |
| `stageentryattachments` | `stageentryid` |
| `executionentryattachments` | `executionEntryNo` (a GUID despite its existing name) |

Posted owner GUIDs identify one exact archived version. Document numbers, `postedVersion`, `profileCode` and `vehicleUnitTypeCode` remain available for display and existing filters. The GUID properties also support direct filters, for example:

```http
GET freightorderinstructions?$filter=freightOrderId eq <order-guid>
GET postedforwardingorderinstructions?$filter=postedForwardingOrderId eq <posted-order-guid>
GET profileunittyperules?$filter=transportProfileId eq <profile-guid>
GET profileunittyperules?$filter=vehicleUnitTypeId eq <unit-type-guid>
```

The generic `attachments.parentId` is populated for TMS Forwarding Orders, Freight Orders, their posted versions, forwarding and freight agreements, vehicles, drivers and carriers. Posted attachments resolve the exact archived version; agreements resolve their complete owner key. Other owner types or missing owners return a zero GUID. Continue using `tableID` when interpreting the owner and use the attachment's own `id` for downloads.

Existing records receive these references during the extension data upgrade. Unresolved owners retain an unresolved reference and are reported in the TMS upgrade log; the backfill is retried on a later upgrade until all supported owners are resolved. Instructions, profile details/rules and generic attachments remain read-only.

Execution-entry attachments return the saved owner GUID in `executionEntryNo`. Direct creation must supply it. Nested creation takes the GUID from the selected execution entry:

```http
POST executionentries(<entry-guid>)/executionentryattachments
Content-Type: application/json

{
  "fileName": "pod.jpg",
  "mimeType": "image/jpeg",
  "description": "Proof of delivery"
}
```

If `executionEntryNo` is also supplied in the nested request body, it must match the parent URL. An existing attachment cannot be moved to another execution entry through PATCH. Creation makes the attachment record; upload file bytes through its existing `UploadAttachment` bound action.

### Stage entry attachments

Use `stageentryattachments` for photographs and documents supporting a recorded stage event. Direct creation requires the event's `stageentryid`. Nested creation takes it from the selected stage entry:

```http
POST stageentries(<entry-guid>)/stageentryattachments
Content-Type: application/json

{
  "fileName": "pod.jpg",
  "mimeType": "image/jpeg",
  "description": "Proof of delivery"
}
```

Creation preserves the supplied file name, MIME type and description. If `stageentryid` is also supplied in a nested request, it must match the parent URL. The owning event is fixed after creation: a PATCH that changes or clears `stageentryid` is rejected. Updating metadata or replacing file content keeps the attachment with the same event.

Upload file bytes through the attachment's `UploadAttachment` bound action. This action replaces the file and sets its description and MIME type, while retaining its file name. `DownloadAttachment` returns the file as Base64. Updating metadata alone preserves existing file content.

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
