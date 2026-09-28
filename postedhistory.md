---
title: "Posted History"
description: "Review posted Forwarding Orders, posted Freight Orders, posted settlement, and related posted TMS records."
---

# Posted History

Use **Posted History** to review transportation work after it has been completed and posted.

Posted records give customer service, finance, and operations a read-only audit trail.

## Before you start

Make sure that:

- active documents have been posted,
- users have permission to open posted TMS records,
- date and customer filters are set correctly when searching history.

## Main posted records

| Record | Use it for |
|---|---|
| **Posted Forwarding Order** | Main completed job history. |
| **Posted Freight Order** | Historical carrier execution record. |
| **Posted Stage Entries** | Historical execution event records. |
| **Posted Settlement** | Historical income, cost, allocation, and margin. |
| **Posted Logistic Unit** | Historical cargo unit structure. |
| **Posted Attachments** | Files linked to posted documents. |
| **Posted Instructions** | Read-only rich-content instruction snapshots stored with posted FWO and FO history. See [Instructions](instructions.md). |

## How to work with posted history

1. Search for the posted list you need.
2. Filter by customer, carrier, posting date, document number, or reference.
3. Open the posted document.
4. Review parties, cargo, stages, settlement, and attachments.
5. Open related posted sales or purchase documents when needed.
6. Print historical documents only when the process allows it.

## Posted Freight Order card

The posted card follows the Freight Order card's field order and groups:

1. **General** — document dates, references, type, status, route, and comments.
2. **Date and Time Planning** — loading and unloading windows.
3. **Instructions** — the saved instruction snapshots.
4. **Carrier** — Actual Carrier and Carrier Agent / Pay-to.
5. **Vehicle and Driver** — Vehicle Dimensions, Vehicle, and Driver.
6. **Stages**, **Content**, and **Charges** — records from the selected posted version.
7. **Additional Information** — creation/modification details and the related Forwarding Order No.

**General** includes **Voyage Number**, the optional shipping line or vessel operator reference copied automatically from the Freight Order at posting. Its saved text retains the original case and format, up to 50 characters, and is read-only here. The posted Freight Order API exposes it as **voyageNumber**. See [Freight Order](freightorder.md) for how this reference differs from IMO Number, SCAC, and Flight Number. **Voyage Number** is also available in the **Posted Freight Orders** list; it is hidden by default and can be shown through personalization.

The vehicle/equipment type is shown as **Vehicle Unit Type Code**, using the value saved at posting. Payload Weight, Internal Volume, and Footage follow the same hidden defaults as the active card and remain available for personalization.

The **Content** part shows all content for this posted Freight Order version. Selecting a stage updates the **Stage** FactBox with its archived dates, locations, contacts, distance, and duration. The **Pay-to Detail** FactBox shows current vendor statistics; those statistics are not an archived value.

Posted fields remain read-only.

## Good to know

- Posted history is read-only.
- The Posted Freight Order card includes **Date and Time Planning**, with the loading and unloading dates, times, allowed windows, calculated ends, bays, and priorities saved at posting.
- Corrections should follow your finance and operational correction process.
- Posted records are the best source for audit, reporting, and customer service review.

## Troubleshooting

| Problem | What to check |
|---|---|
| Posted document is not found | Check filters, company, posting date, and the original document number. |
| Related invoice is missing | Review settlement posting and linked document references. |
| User cannot open posted history | Check posted-document permissions. |

## Related

- [Posted Forwarding Order](postedforwardingorder.md)
- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Stage Entries](stageentries.md)
- [Settlement](settlement.md)
- [Instructions](instructions.md)
