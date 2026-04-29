---
title: "Screenshot Registry"
description: "List of screenshots needed for the TMS for Logistics Service Providers documentation site."
---

# Screenshot Registry

Use this registry when creating or replacing screenshots for the GitHub Pages documentation.

Save screenshots under `docs/resources/<topic>/pics/` unless the article already uses a different folder for that topic.

## Screenshot rules

- Use a sandbox company with clean demo data.
- Hide personal data, customer addresses, vendor tax IDs, and credentials.
- Capture the Business Central web client at 1440 px wide when possible.
- Keep the browser zoom at 100 percent.
- Crop only when the article explains one field group or action.
- Replace old screenshots when the UI changes.

## Current screenshot needs

| Article | File name | Capture guidance |
|---|---|---|
| [Forwarding Order](forwardingorder.md) | `docs/resources/forwardingorder/pics/forwardingorder.png` | Forwarding Order card with General, parties, content, stages, and settlement visible. |
| [Freight Order](freightorder.md) | `docs/resources/freightorder/pics/freightorder.png` | Freight Order card with carrier, vehicle, driver, content, and charges visible. |
| [TMS Setup](setup.md) | `docs/resources/setup/pics/setup3.png` | Main TMS Setup page with core setup groups visible. |
| [Forwarding Order Types](forwardingordertype.md) | `docs/resources/forwardingordertype/pics/fwotype1.png` | Type card showing status profile, stage profile, and default fields. |
| [Statuses](statuses.md) | `docs/resources/statuses/pics/statuses1.png` | Status profile list or card with action control visible. |
| [Stages](stages.md) | `docs/resources/stages/pics/stages1.png` | Stage profile with stage lines visible. |
| [Services](services.md) | `docs/resources/services/pics/services1.png` | Services list with clean service examples. |
| [Charges](charges.md) | `docs/resources/charges/pics/charges1.png` | Charges list with cost examples. |
| [Map Locations](maplocation.md) | `docs/resources/maplocation/pics/maplocationNew.png` | Map Location card with address and coordinates visible. |
| [Google Maps Integration](googlemapintegration.md) | `docs/resources/googlemap/pics/googlemap1.png` | TMS Setup map provider section with credentials hidden. |
| [Buy Licenses](buylicenses.md) | `docs/resources/buylicenses/pics/buylicense1.png` | Microsoft admin or AppSource purchase step with tenant data hidden. |
| [Assign Permission Sets](assignpermissionsets.md) | `docs/resources/assignpermissionset/pics/assignpermission1.png` | User Permission Sets page with TMS permission sets visible. |
| [Carriers](carrier.md) | `docs/resources/carrier/pics/carrier-card.png` | Carrier card with General and Defaults groups visible. |
| [Drivers](driver.md) | `docs/resources/driver/pics/driver-card.png` | Driver card with carrier assignment and contact-related groups visible. |
| [Vehicles](vehicle.md) | `docs/resources/vehicle/pics/vehicle-card.png` | Vehicle card with General, carrier, registration, and equipment groups visible. |
| [Products](product.md) | `docs/resources/product/pics/products-list.png` | Product list with cargo descriptions, freight class, and weight columns visible. |
| [Time Slots](timeslots.md) | `docs/resources/timeslots/pics/timeslots-list.png` | Time slot profile list with profile code, description, and slot count visible. |
| [Settlement](settlement.md) | `docs/resources/settlement/pics/settlement-overview.png` | Forwarding Order settlement lines showing income, cost, allocation, and totals. |
| [Purchase Assignment](purchaseassignment.md) | `docs/resources/purchaseassignment/pics/purchase-assignment.png` | Purchase Document Assignment page with vendor invoice lines and assigned line styling. |
| [Stage Entries](stageentries.md) | `docs/resources/stageentries/pics/stage-entries.png` | Stage Entries list with execution status, carrier, driver, vehicle, attachments, and images FactBox. |
| [Freight Agreements](freightagreement.md) | `docs/resources/freightagreement/pics/freight-agreement-card.png` | Freight Agreement card with carrier, type, currency, dates, active flag, and charge lines. |
| [Freight Order Types](freightordertype.md) | `docs/resources/freightordertype/pics/freight-order-types.png` | Freight Order Types list showing status profile, default status, and number series. |
| [Order Wizards](orderwizards.md) | `docs/resources/orderwizards/pics/forwarding-order-wizard.png` | New Forwarding Order Wizard on the first or party step with demo data only. |
| [Azure Blob Storage](azureblobstorage.md) | `docs/resources/azureblobstorage/pics/azure-blob-storage-setup.png` | TMS Setup Azure Blob Storage section with account key fully hidden. |
| [Attachment Control](attachmentcontrol.md) | `docs/resources/attachmentcontrol/pics/attachment-control-overview.png` | Attachment Control page with category columns and counts. |
| [Freight Load Management](freightloadmanagement.md) | `docs/resources/freightloadmanagement/pics/freight-load-management.png` | Planning view filtered to one day or carrier. |

## New screenshots to add later

| Article | Suggested file name | Capture guidance |
|---|---|---|
| [Pricing](pricing.md) | `docs/resources/pricing/pics/pricing-example.png` | Price setup or settlement price calculation example. |
| [Customer Portal Entities](customerportal.md) | `docs/resources/customerportal/pics/portal-ticket.png` | Portal ticket list or card with customer data anonymized. |

## Sensitive screenshots

Never show these values in documentation screenshots:

- Azure storage account keys,
- API keys,
- tenant names,
- real customer or vendor names,
- personal addresses,
- carrier contract numbers when they are commercially sensitive.

## Related

- [Documentation Standards](documentation-standards.md)
- [TMS Setup](setup.md)
