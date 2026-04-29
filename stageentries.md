---
title: "Stage Entries"
description: "Review execution events, statuses, location points, images, and attachments recorded against Forwarding Order stages."
---

# Stage Entries

Use **Stage Entries** to review execution events for Forwarding Order stages and Freight Orders.

A stage entry records what happened, when it happened, which execution status was used, and which carrier, driver, vehicle, location, or attachment belongs to the event.

## Before you start

Make sure that:

- a Forwarding Order has stages,
- the related Freight Order exists when carrier execution is tracked,
- the stage execution status profile is configured in [TMS Setup](setup.md),
- users have permission to view attachments and images.

## What stage entries show

| Field | Why it matters |
|---|---|
| **Entry Date and Time** | Shows when the entry was created. |
| **Stage Description** | Identifies the transportation leg. |
| **Type** | Classifies the entry as message, start, finish, proof, or incident. |
| **Execution Status Code** | Shows the current execution status for the entry. |
| **Description** | Adds user-facing context. |
| **Carrier / Driver / Vehicle** | Shows execution resources copied from the Freight Order. |
| **Attachments** | Counts files linked to the entry. |
| **Latitude / Longitude / Altitude** | Supports map display when coordinates exist. |

## How to work in this page

1. Search for **Stage Entries** or open entries from a related document.
2. Filter by date, stage, carrier, or Freight Order.
3. Review entry type, status, description, and resource details.
4. Open images or attachments when the entry contains evidence.
5. Use **Show on Map** when coordinates exist.
6. Delete an entry only when the business process allows it.

## Good to know

- Stage Entries are operational history for execution events.
- Entry status comes from the stage execution status profile.
- Attachments linked to an entry are removed when the entry is deleted.
- Posted Stage Entries are used for read-only history after posting.

## Troubleshooting

| Problem | What to check |
|---|---|
| Entry has no carrier or driver | Check whether the Freight Order had carrier, driver, and vehicle values when the entry was created. |
| Show on Map does not work | Check latitude and longitude on the entry. |
| Attachment count is wrong | Open the entry attachments and confirm they are linked to the selected entry. |
| User cannot delete an entry | Check permissions and company policy. |

## Related

- [Stages](stages.md)
- [Freight Order](freightorder.md)
- [Forwarding Order](forwardingorder.md)
- [Posted History](postedhistory.md)
