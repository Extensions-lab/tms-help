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

## Posted Settlement amounts

Posted Settlement retains the document signs of Quantity, Price, Amount before discount, and Line Amount after line discount. **P/L Amount (LCY)** is the signed financial contribution used for profit; **P/L Amount (ACY)** is its conversion to the additional reporting currency configured in TMS Setup. If that setting is blank, ACY stores the same value as LCY. For an ordinary purchase without discounts, Amount 100 and Line Amount 100 correspond to P/L Amount −100. Invoice Discount Amount, Net Amount Excl. VAT, and Amount Including VAT preserve the remaining document values directly. The P/L Amount (ACY) column is visible when Additional Reporting Currency is filled in TMS Setup. Posted source documents use the same amount calculation service as open documents.

**Posted Source Deleted** records deletion of a linked posted sales/purchase invoice, credit memo, or source line. Every affected historical version retains its original link, posted state and financial amounts. This controlled metadata update does not recalculate the historical snapshot. The same flag is copied when an active FWO is subsequently archived; a new independent plan does not inherit it.

Older rows marked **Settlement Upgrade Required**, including the first document/P/L model, have not yet been converted to the canonical document fields (amount-model version 2). Their new amounts and related P/L totals are incomplete. An administrator can use **TMS Setup > Data Migration > Settlement Amount Model Upgrade** to preview and convert posted orders, including their saved versions. This is a controlled change to Settlement snapshots, not a reposting of the financial documents. Read the [wizard steps and group rules](setup.md#upgrade-existing-settlement-amounts) before converting history. The conversion is not run automatically during extension upgrade.

When the same allocation appears in several historical versions, review each version separately. Those versions are snapshots of the same allocation, not additional costs to sum into one source total. The wizard converts consistent copies together and stops for conflicting allocations.

## Detach an archived Settlement reference

**Detach Settlement** removes a direct financial-document assignment through the same mechanism used for active Settlement. On Posted Sales Invoice and Posted Purchase Invoice lines, use **TMS > Detach Settlement** for one source line or **TMS > Detach All Lines** for the invoice. The invoice actions also work when all its Settlement references are archived. Starting the command from an active Settlement row selects that source line's complete group, not just the current order.

Every physical reference in the group is included, across active Settlement and all saved posted FWO versions. For example, if a purchase line belongs to **five FWOs, three posted and two active**, the command clears the references in all three posted orders and both active orders. Multiple archived versions are all included.

The two kinds of record have different outcomes:

- Active rows become unlinked **Fixed Amount** plans and keep their previous P/L in all currencies.
- Posted rows remain historical records. Their quantities, prices, discounts, amounts, currency values, P/L, and saved business details do not change. Only the financial-document reference and related allocation-link state are cleared.

Detach does not synchronize history to the current document, revalue its currency amounts, reopen a posted FWO, or change financial entries. Different historical versions retain their own values. Posted fields remain read-only; this controlled command does not permit ordinary editing of archived records.

Supported archived sources are **Order**, **Invoice**, **Credit Memo**, **Posted Invoice**, and **Posted Credit Memo**. Freight Order parent allocations, TMS correction chains, unresolved legacy data, and archived Quote, Blanket Order, or Return Order references are not supported by direct detach. Every member must pass the checks; a failure rolls back the whole command. See [Settlement](settlement.md#change-or-remove-a-document-link) and [Purchase Assignment](purchaseassignment.md#remove-or-change-an-assignment).

## Good to know

- FWO posting archives the saved Settlement values without recalculating them. Freight Charge lines are full snapshots of the stored parent in each posted Freight Order version. FWO-specific allocated costs remain in Posted Settlement; adding parent snapshots across versions is not the allocation total.

- Posted history is read-only. The controlled **Detach Settlement** command can clear supported source references while preserving the financial snapshot.
- The Posted Freight Order card includes **Date and Time Planning**, with the loading and unloading dates, times, allowed windows, calculated ends, bays, and priorities saved at posting.
- Corrections should follow your finance and operational correction process.
- Posted records are the best source for audit, reporting, and customer service review.

## Troubleshooting

| Problem | What to check |
|---|---|
| Posted document is not found | Check filters, company, posting date, and the original document number. |
| Related invoice is missing | The source document may no longer exist. During the manual amount-model conversion, posted FWO Settlement retains its historical reference and financial result and uses saved values; source absence alone is not an error. Active FWO rows still require their source. Explicit Detach is a separate operation that intentionally clears a reference. |
| User cannot open posted history | Check posted-document permissions. |

## Related

- [Posted Forwarding Order](postedforwardingorder.md)
- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Stage Entries](stageentries.md)
- [Settlement](settlement.md)
- [Instructions](instructions.md)
