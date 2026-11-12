---
title: "Forwarding Control Tower"
description: "Review Forwarding Orders, stage planning, Freight Order assignment, carriers, checklist tasks, and document completeness from one operations view."
---

# Forwarding Control Tower

Use **Forwarding Control Tower** to monitor active Forwarding Orders from one operations view.

The page combines the main control signals from Forwarding Orders, Freight Orders, stages, attachments, and checklists so dispatchers can quickly see which orders need attention.

By default, the page shows only orders with **Attention Needed** selected. Read **Attention Summary** for the required follow-up. Choose **Filters > Show All** to include orders without an attention flag, or **Needs Attention** to return to the attention filter.

## Before you start

Make sure that:

- Forwarding Orders and stages exist,
- Freight Orders are created for stages that need carrier execution,
- attachment category filters are configured in **Attachment Settings** when document completeness must be monitored,
- checklist lines are configured when operational tasks are used.

## What the page shows

| Area | Use it for |
|---|---|
| **Forwarding Order list** | Review one row per Forwarding Order, including attention flag, stage gaps, Freight Order count, carriers, documents, and open checklist tasks. |
| **Stages and Freight Orders** | See the selected Forwarding Order split by stage, with planned dates, linked Freight Order, FO status, carrier, driver, vehicle, and stage entries. |
| **Attachments by Type** | Review selected attachment categories and counts on the Forwarding Order and related Freight Orders. |
| **Check List** | Review operational checklist tasks for the selected Forwarding Order. |
| **FWO Instructions** | Review instruction codes and descriptions for the selected Forwarding Order and open a formatted read-only preview. |

## Attention flag

The **Attention Needed** field is selected when at least one monitored area needs follow-up. When the field is selected, read **Attention Summary** to see the reason.

Common reasons include:

- one or more stages are not planned,
- one or more stages do not have a Freight Order,
- a stage or Freight Order has no carrier,
- selected document categories are missing,
- checklist tasks are still open.

## How to work in this page

1. Open **Forwarding Control Tower**. Only orders that need attention are shown initially.
2. Use the predefined views such as **Unplanned**, **Without Freight Order**, **Without Carrier**, **Missing Documents**, or **Open Tasks**.
3. Select a Forwarding Order.
4. Review stages and related Freight Orders in the details area.
5. Review document categories in **Attachments by Type**.
6. Review [Instructions](instructions.md) in the **FWO Instructions** FactBox when handling guidance affects the next action.
7. Open the Forwarding Order, Freight Orders, attachments, or stage entries when follow-up is needed.
8. Choose **Update** after users change planning, carrier assignment, documents, or checklist tasks.

## Predefined views

| View | Shows |
|---|---|
| **Needs Attention** | Forwarding Orders with at least one planning, carrier, document, or task gap. This is also the default filter when the page opens. |
| **Without Freight Order** | Forwarding Orders with stages where execution has not been created or assigned yet. |
| **Without Carrier** | Forwarding Orders where at least one Freight Order exists, but the carrier is not selected. |
| **Missing Documents** | Forwarding Orders missing one or more required attachment categories. |
| **Open Tasks** | Forwarding Orders with checklist items that are not completed. |
| **Unplanned** | Forwarding Orders with stages missing planned start or end dates. |

## Good to know

- The page is a monitoring workspace. It does not replace the Forwarding Order, Freight Order, Attachment Control, or Check List pages.
- Attachment completeness uses the same selected category filters as Attachment Control.
- Freight Order attachment counts include the Freight Orders assigned to stages of the selected Forwarding Order.
- Posted Forwarding Orders are not included in this active control view.
- **Update** recalculates the attention flags and keeps the current filters. With the attention filter active, orders disappear from the list once all monitored gaps are resolved.

## Troubleshooting

| Problem | What to check |
|---|---|
| An active order is missing from the list | The default filter hides orders without an attention flag. Choose **Filters > Show All** and clear any other active view or filters to see them. |
| A document category is always missing | Open **Attachment Settings** and confirm the category filter. |
| A Freight Order is not shown in details | Check whether the Freight Order is assigned to a stage on the Forwarding Order. |
| Carrier is missing even though a carrier exists elsewhere | Open the Freight Order and confirm **Carrier No.** is filled. |
| Counts look stale | Choose **Update** to rebuild the temporary control view. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Freight Load Management](freightloadmanagement.md)
- [Attachment Control](attachmentcontrol.md)
- [Check List](checklist.md)
- [Stage Entries](stageentries.md)
- [Instructions](instructions.md)
