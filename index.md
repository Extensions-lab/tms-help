---
title: "TMS for Logistics Service Providers"
description: "User documentation for the LSP transportation management extension for Microsoft Dynamics 365 Business Central."
layout: home
---

# TMS for Logistics Service Providers Help

TMS for Logistics Service Providers helps freight forwarders, brokers, and 3PL teams manage forwarding, carrier execution, settlement, and customer invoicing in Microsoft Dynamics 365 Business Central.

Use this help when you need to create a Forwarding Order, assign Freight Orders, control stages and documents, allocate carrier costs, invoice customers, or review posted history.

Use these terms this way:

| Term | Meaning |
|---|---|
| **Forwarding Order** | The main LSP document. It defines what must be moved, who ordered it, cargo details, parties, stages, and settlement. |
| **Freight Order** | The carrier execution document. It defines who executes a stage, with carrier, vehicle, driver, content, charges, and transport documents. |
| **Stage** | A leg of transportation, such as pickup, main carriage, or final delivery. |
| **Settlement** | The financial workspace for income, carrier costs, invoices, credit memos, allocation, and profit review. |
| **Service** | What you charge the customer for. |
| **Charge** | What you pay or allocate as cost. |
| **Posted Forwarding Order** | Read-only transport history after the Forwarding Order is posted. |

## New to TMS

If you are setting up TMS for the first time, follow this order:

1. [Install TMS](installation.md)
2. [Buy licenses](buylicenses.md)
3. [Assign permission sets](assignpermissionsets.md)
4. [Complete TMS Setup](setup.md)
5. Set up [Statuses and Status Profiles](statuses.md)
6. Set up [Stages](stages.md)
7. Set up [Freight Order Types](freightordertype.md)
8. Create your first [Forwarding Order](forwardingorder.md)
9. Run [Create and Invoice Your First Forwarding Job](first-forwarding-job.md)
10. Review [Settlement](settlement.md)

After step 10, your users should understand the basic LSP flow from customer request to carrier execution, settlement, invoicing, and posted history.

## First 30 minutes

Use this short path when you want to prove the basic process in a sandbox:

1. Complete the minimum setup in [TMS Setup](setup.md).
2. Create one [Forwarding Order Type](forwardingordertype.md).
3. Create one [Forwarding Order](forwardingorder.md).
4. Add content, shipper, consignee, planned dates, and stages.
5. Create one [Freight Order](freightorder.md) for a stage.
6. Add one income line and one cost line in [Settlement](settlement.md).
7. Create a customer invoice or review the settlement totals.
8. Use the short [first forwarding job tutorial](first-forwarding-job.md) when you need a guided end-to-end test.

Do this before configuring advanced price lists, attachment rules, API integrations, or Azure Blob Storage.

## Choose your role

| Role | Start with | Why |
|---|---|---|
| Forwarding coordinator | [Forwarding Order](forwardingorder.md), [Stages](stages.md), [Freight Order](freightorder.md) | Manage customer requests and carrier execution. |
| Dispatcher or operations planner | [Freight Load Management](freightloadmanagement.md), [Freight Order](freightorder.md), [Stage Entries](stageentries.md), [Carriers](carrier.md) | Plan carrier work, execution capacity, and stage progress. |
| Finance user | [Settlement](settlement.md), [Purchase Assignment](purchaseassignment.md), [Services](services.md), [Charges](charges.md), [Pricing](pricing.md) | Control income, cost, allocation, and margin. |
| TMS administrator | [TMS Setup](setup.md), [Forwarding Order Types](forwardingordertype.md), [Freight Order Types](freightordertype.md), [Statuses](statuses.md), [Stages](stages.md) | Configure the rules that control daily work. |
| Document controller | [Attachment Control](attachmentcontrol.md), [Check List](checklist.md), [Reports and Documents](reports.md) | Track required files, tasks, and customer/carrier documents. |
| Integration user | [API](api.md), [Business Central Integration](integration.md), [Customer Portal Entities](customerportal.md) | Connect TMS to portals, Power BI, and external systems. |

## Daily work

| Job | Start here |
|---|---|
| Create a customer transportation job | [Forwarding Order](forwardingorder.md) |
| Define the transportation legs | [Stages](stages.md) |
| Assign the carrier or executor | [Freight Order](freightorder.md) |
| Plan carrier workload | [Freight Load Management](freightloadmanagement.md) |
| Record or review execution events | [Stage Entries](stageentries.md) |
| Add billable services | [Settlement](settlement.md) |
| Allocate carrier invoices | [Purchase Assignment](purchaseassignment.md), [Charges](charges.md), and [Settlement](settlement.md) |
| Create customer invoices | [Settlement](settlement.md) |
| Track required files | [Attachment Control](attachmentcontrol.md) |
| Track operational tasks | [Check List](checklist.md) |
| Review posted history | [Posted Forwarding Order](postedforwardingorder.md) |

## Core concepts

| Topic | What it explains |
|---|---|
| [Forwarding Order](forwardingorder.md) | The main LSP transportation document |
| [Freight Order](freightorder.md) | Carrier execution for one or more stages |
| [Stage Entries](stageentries.md) | Execution events, statuses, attachments, images, and map points |
| [Settlement](settlement.md) | Income, cost, allocation, invoicing, and profit |
| [Stages](stages.md) | Stage profiles and transportation legs |
| [Statuses and Status Profiles](statuses.md) | Workflow control and action permissions |
| [Posted History](postedhistory.md) | Read-only history after posting |
| [Reports and Documents](reports.md) | Print and email documents |

## Setup and master data

| Topic | What it covers |
|---|---|
| [TMS Setup](setup.md) | Core system settings |
| [Forwarding Order Types](forwardingordertype.md) | Templates for FWO behavior, defaults, stages, and statuses |
| [Freight Order Types](freightordertype.md) | Defaults for Freight Order numbering, status, charge calculation, and cost distribution |
| [Carriers](carrier.md) | Transport service providers |
| [Vehicles](vehicle.md) | Fleet and equipment records |
| [Drivers](driver.md) | Driver records and defaults |
| [Products](product.md) | Generic cargo descriptions |
| [Services](services.md) | Billable service master data |
| [Charges](charges.md) | Cost master data and purchase mapping |
| [Forwarding Agreements](forwardingagreement.md) | Customer commercial agreements |
| [Freight Agreements](freightagreement.md) | Carrier commercial agreements and cost lines |
| [Routes](route.md) | Reusable lanes or service areas |
| [Time Slots](timeslots.md) | Standard appointment windows |
| [Map Locations](maplocation.md) | Geocoded location records |
| [Map Location Types](maplocationtype.md) | Classification of map locations |
| [Logistic Unit Types](logisticunittype.md) | Unit and capacity profiles |
| [Google Maps Integration](googlemapintegration.md) | Google API key setup |

## Integration and administration

| Topic | What it covers |
|---|---|
| [API](api.md) | API areas, permissions, and integration pattern |
| [Business Central Integration](integration.md) | Standard BC records used by TMS |
| [Customer Portal Entities](customerportal.md) | Portal tickets and notifications |
| [AI and Copilot Features](copilot.md) | AI-assisted draft creation and AI task results |
| [Order Wizards](orderwizards.md) | Guided Forwarding Order and Freight Order creation |
| [Azure Blob Storage for Attachments](azureblobstorage.md) | External storage for high-volume TMS attachments |
| [Assign Permission Sets](assignpermissionsets.md) | User and integration permissions |
| [Buy Licenses](buylicenses.md) | License purchase and assignment |

## Troubleshooting starting points

| Problem | Start with |
|---|---|
| A user cannot open TMS pages | [Assign Permission Sets](assignpermissionsets.md) |
| A Forwarding Order action is disabled | [Statuses and Status Profiles](statuses.md) |
| Freight Orders cannot be created | [Forwarding Order](forwardingorder.md), [Stages](stages.md), [Statuses](statuses.md) |
| Settlement cannot create an invoice | [Settlement](settlement.md), [Services](services.md), [Setup](setup.md) |
| Carrier cost cannot be allocated | [Purchase Assignment](purchaseassignment.md), [Charges](charges.md), [Settlement](settlement.md) |
| Distance or route is not calculated | [Google Maps Integration](googlemapintegration.md), [Map Locations](maplocation.md) |
| Required documents are missing | [Attachment Control](attachmentcontrol.md), [Statuses](statuses.md) |

## Maintenance

- [Create and Invoice Your First Forwarding Job](first-forwarding-job.md)
- [Documentation standards](documentation-standards.md)
- [Screenshot registry](screenshot-registry.md)
