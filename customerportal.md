---
title: "Customer Portal Entities"
description: "Use portal tickets and notifications to connect customer-facing systems with TMS operations."
---

# Customer Portal Entities

Use **Customer Portal** entities when an external portal needs to exchange requests, messages, or status notifications with TMS.

These records help connect external customer communication with internal Forwarding Order processing.

## Before you start

Make sure that:

- the portal integration user is created,
- API permissions are assigned,
- customer and contact mapping is agreed,
- the external portal knows which TMS references it must store.

## Main entities

| Entity | Use it for |
|---|---|
| **Portal Ticket** | Tracks a customer-facing request or issue connected to a TMS document. |
| **Portal Notification** | Stores messages or events that should be shown or sent through the portal. |
| **Forwarding Order reference** | Connects portal work to the operational document. |
| **Customer reference** | Lets the portal filter records for the correct customer. |

## Recommended portal flow

1. Portal creates or updates a ticket.
2. TMS user reviews the linked request.
3. TMS user creates or updates the Forwarding Order.
4. TMS writes status or document references back through API-visible fields.
5. Portal reads notifications and shows them to the customer.

## Good to know

- Keep customer portal IDs separate from Business Central document numbers.
- Use references consistently so users can move from portal ticket to Forwarding Order and back.
- Do not expose internal cost or margin fields to customer portals unless your API layer intentionally allows it.

## Troubleshooting

| Problem | What to check |
|---|---|
| Portal cannot see a ticket | Check company, customer filter, API permissions, and record status. |
| Ticket is not linked to the order | Confirm the Forwarding Order reference and external ID mapping. |
| Customer sees old status | Check the notification job, API cache, and status ownership. |

## Related

- [Public API](api.md)
- [Forwarding Order](forwardingorder.md)
- [Integration](integration.md)
