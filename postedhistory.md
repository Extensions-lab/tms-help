---
title: "Posted History"
description: "Review posted Forwarding Orders, posted Freight Orders, posted settlement, and related posted TMS records."
---

# Posted History

Use **Posted History** to review transportation work after it has been completed and posted.

Posted records give customer service, finance, and operations a read-only audit trail.

**Posted Forwarding Orders** and **Posted Freight Orders** open with the most recently posted records first, using the date and time each posted version was saved. Orders posted later appear above earlier records, including within the same day and when an earlier **Posting Date** is used. A saved view or a manually selected column sort can use a different order.

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

## Posting order and shared trips

Post the **Freight Order** first to close carrier execution, after its linked purchases are posted and every Freight Order charge is fully allocated to Forwarding Order Settlement. One Posted Freight Order preserves the whole trip: its header, all related Forwarding Order stages, content, charges, instructions, and Freight Order attachments. The related Forwarding Orders may still be active while customer income, sales invoicing, and other closing requirements are completed.

Post each **Forwarding Order** afterward, once all Freight Orders assigned to its stages are posted and its other closing controls pass. Its own content, settlement, instructions, and attachments are saved with that Posted Forwarding Order. Its stages retain their links to the existing Posted Freight Orders; this step completes the customer job's historical identity without creating additional trip archives or rewriting the trip's operational and financial facts.

For a trip serving **FWO-A** and **FWO-B**, both posted jobs refer to the same Posted Freight Order and its shared carrier documents. Open the linked trip history for those files; open the Posted Forwarding Order's attachments for files belonging directly to that customer job.

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
6. **Stages**, **Content**, and **Charges** — the complete trip records from the selected posted version, including all related Forwarding Orders.
7. **Additional Information** — creation/modification details.

**General** includes **Voyage Number**, the optional shipping line or vessel operator reference copied automatically from the Freight Order at posting. Its saved text retains the original case and format, up to 50 characters, and is read-only here. The posted Freight Order API exposes it as **voyageNumber**. See [Freight Order](freightorder.md) for how this reference differs from IMO Number, SCAC, and Flight Number. **Voyage Number** is also available in the **Posted Freight Orders** list; it is hidden by default and can be shown through personalization.

**Forwarding Orders(s)** in the list and **General** shows the distinct Forwarding Order numbers carried by this posted Freight Order version. The numbers are saved from its posted stages when the trip is posted, sorted by number and separated by commas. This is a stored, read-only field, so list search and filters can find its saved text. The text is limited to 100 characters; longer lists are cut off, and numbers beyond that limit are not searchable through this column. Choosing the field reads the complete membership from the posted stages and opens only related **Posted Forwarding Orders**: one matching record opens its card, several open the standard filtered list, and no matches display a message. Active Forwarding Orders remain in the saved text but are not opened by this DrillDown. Existing archives are filled from their saved stages once when the application is upgraded.

The vehicle/equipment type is shown as **Vehicle Unit Type Code**, using the value saved at posting. Payload Weight, Internal Volume, and Footage follow the same hidden defaults as the active card and remain available for personalization.

**Vehicle Registration No.** in **Vehicle and Driver > Vehicle** is the text saved on the Freight Order at posting, including any manual correction or additional description, up to 50 characters. It is read-only and hidden by default; use personalization to show it. Changes to the current Vehicle register do not update this snapshot. Existing posted versions are not filled automatically. See [Freight Order](freightorder.md) for entering the value on the active card.

The **Content** part shows all content for this posted Freight Order version. Selecting a stage updates the **Stage** FactBox with its archived dates, locations, contacts, distance, and duration. Stages can belong to Forwarding Orders that have not yet been posted. Posting those jobs later completes their historical links without changing the saved trip details. The **Pay-to Detail** FactBox shows current vendor statistics; those statistics are not an archived value.

Posted fields remain read-only.

## Posted Settlement amounts

The **Settlement** subpage on a Posted Forwarding Order shows the saved **Customer/Vendor No.**, **Financial Effect**, and **Pricing Mode** for each line. **Financial Effect** identifies **Standard** billing or a **Credit**; **Pricing Mode** identifies **Per Unit** or **Fixed Amount** calculation. These columns are read-only and use the values saved with that posted version. See [Settlement](settlement.md#financial-effect) for the meaning of the financial effect and pricing modes.

Posted Settlement retains the document signs of Quantity, Price, Amount before discount, and Line Amount after line discount. **P/L Amount (LCY)** is the signed financial contribution used for profit; **P/L Amount (ACY)** is its conversion to the additional reporting currency configured in TMS Setup. If that setting is blank, ACY stores the same value as LCY. For an ordinary purchase without discounts, Amount 100 and Line Amount 100 correspond to P/L Amount −100. Invoice Discount Amount, Net Amount Excl. VAT, and Amount Including VAT preserve the remaining document values directly. The P/L Amount (ACY) column is visible when Additional Reporting Currency is filled in TMS Setup. Posted source documents use the same amount calculation service as open documents.

**Posted Source Deleted** records deletion of a linked posted sales/purchase invoice, credit memo, or source line. Every affected historical version retains its original link, posted state and financial amounts. This controlled metadata update does not recalculate the historical snapshot. The same flag is copied when an active FWO is subsequently archived; a new independent plan does not inherit it.

Older rows marked **Settlement Upgrade Required**, including the first document/P/L model, have not yet been converted to the canonical document fields (amount-model version 2). Their new amounts and related P/L totals are incomplete. An administrator can use **TMS Setup > Data Migration > Settlement Amount Model Upgrade** to preview and convert posted orders, including their saved versions. This is a controlled change to Settlement snapshots, not a reposting of the financial documents. Read the [wizard steps and group rules](setup.md#upgrade-existing-settlement-amounts) before converting history. The conversion is not run automatically during extension upgrade.

When the same allocation appears in several historical versions, review each version separately. Those versions are snapshots of the same allocation, not additional income or costs to sum into one source total. **Allocated Amount** counts matching copies of the same FWO and Settlement line once and gives precedence to a matching active copy. Source identities, shares, quantities, amounts, and currency context must agree. Conflicting snapshots remain separate contributions for review; opening the financial document is not blocked. Its drilldown retains all physical versions. The wizard and automatic invoice cancellation perform their own stricter consistency checks before changing data.

## Invoice cancellation in posted history

See [Invoice Corrections and Corrective Forwarding Orders](invoice-corrections.md) for the full correction workflow and financial-result example.

For a linked **Posted Purchase Invoice** or **Posted Sales Invoice**, **Cancel**, **Correct**, and **Create Corrective Credit Memo** use the same original allocation. New financial corrections do not add rows to the original posted FWO or change its Settlement or Freight history.

If the original allocation has no matching active FWO copy, TMS asks once to create a new active **corrective Forwarding Order**. Configure [Correction FWO Type Code](setup.md#corrective-forwarding-orders) first. One corrective order is created per original FWO and operation, with the same Ordering Party and without new transportation stages, cargo, or automatically generated charges. Identical historical versions of one allocation do not create multiple corrective orders or multiply the financial amount.

**Cancel** creates a full credit. **Correct** uses the same corrective FWO for its full credit and replacement invoice. **Create Corrective Credit Memo** leaves an editable credit: its linked Settlement follows supported changes, including a partial credit allocated in the original proportions. Purchase corrections are Cost; sales corrections are Income. New managed rows show **Correction Type = Document Correction** and retain their origin and financial-document links. Active original allocations stay on the existing active FWO.

A credit that remains unposted after an unsuccessful **Cancel** or **Correct** can also be edited and posted manually. Settlement follows its current values using the original allocation factors; posting validates those current values and links rather than requiring the original invoice's price or quantity. Manual posting does not resume the original action, create a replacement invoice, or perform its cancellation bookkeeping.

Choose **General > Correction > Has Corrections** on the original posted card to open **Corrective Forwarding Orders**. This field is not shown on active FWO cards. The list shows the corrective order, its active or posted state, and the original invoice, credit memo, and replacement invoice for a document-correction operation. **Documents Created** identifies completion of that operation's document creation; it does not mean the documents have been posted. On an active corrective card, **Corrects Forwarding Order** selects the original posted order through lookup. On a posted corrective card, it opens the saved original version.

The same list and profit calculation include [manually created financial corrections](forwardingorder.md#create-a-financial-correction-manually). They link directly to an original posted FWO without a sales or purchase correction operation; their operation ID and operation-specific financial-document fields are empty. When the original is selected manually, TMS records its number and latest posted version on the corrective FWO header and copies its Ordering Party. The user enters or assigns the new Settlement separately.

The posted corrective card retains **Financial Correction** and **Corrects Forwarding Order** as read-only saved fields in **General > Correction**. Its cargo lines, logistic units, Shipper & Consignee, planning, stages, and transport actions remain hidden, while Settlement is visible. Later changes to the FWO Type do not change this saved classification or the original-order reference. Existing posted records are not rewritten to fill these new fields.

When corrections exist, the **Settlement** subpage on the original posted card shows **Result Including Corrections** below the usual totals:

| Field | Meaning |
|---|---|
| **Saved Profit (LCY)** | The existing Settlement profit of this exact posted FWO version. |
| **Corrections (LCY)** | Current Settlement profit of its separate corrective orders, including later corrections in the chain. |
| **Profit Including Corrections (LCY)** | Saved profit plus that correction effect, counting each corrective order once. |

The active Settlement subpage shows **Order Profit (LCY)** for its own result. The correction list also shows **Correction Profit (LCY)** and **Including Later Corrections (LCY)**. The list and totals read the links saved on active and posted FWO headers. Each corrective order contributes once: its active record takes precedence, otherwise its latest posted version is used. These are read-only calculations from the orders' existing totals; they do not recalculate or rewrite historical Settlement. Active corrective orders can contain **unposted** financial documents, so the combined result includes their current planned effect and is not a posted-ledger-only figure. Filtering or hiding all Settlement rows does not change the order-wide totals; posted totals retain the exact version opened on the card.

For example, a posted sale contributes 1,000. **Correct** creates a credit of −1,000 and an editable replacement invoice of 1,200 on the corrective FWO. Its current effect is +200 and the combined result is 1,200, even while the replacement is unposted. The original saved 1,000 remains unchanged. Deleting the replacement leaves the posted credit in place; it does not restore the original invoice.

Every financial correction must identify an original posted FWO before posting. For document corrections, post the corrective FWO only after its managed financial sources are posted and their copied links are complete. This check applies even when **Posted Documents Control** is off. A retained credit posted manually does not leave the FWO waiting for a replacement invoice. Manual FWO corrections follow the applicable Settlement and posting controls without a document-correction operation. Delete supported unposted corrective-credit or replacement lines through their source documents; their managed active Settlement rows are removed, including for credits created by **Cancel** or **Correct**. An empty corrective FWO can be deleted subject to normal status and permission checks; any document-correction operation remains for audit. There is no separate order-link record to maintain.

Existing history from earlier versions is not migrated by this workflow. Independently created credit memos and ordinary **Copy Document** still require explicit assignment. See [Settlement](settlement.md#correct-a-posted-sales-or-purchase-invoice) and [Purchase Assignment](purchaseassignment.md#cancel-a-linked-posted-purchase-invoice).

## Detach an archived Settlement reference

**Detach Settlement** removes a direct financial-document assignment through the same mechanism used for active Settlement. On Posted Sales Invoice and Posted Purchase Invoice lines, use **TMS > Detach Settlement** for one source line or **TMS > Detach All Lines** for the invoice. The invoice actions also work when all its Settlement references are archived. Starting the command from an active Settlement row selects that source line's complete group, not just the current order.

Every physical reference in the group is included, across active Settlement and all saved posted FWO versions. For example, if a purchase line belongs to **five FWOs, three posted and two active**, the command clears the references in all three posted orders and both active orders. Multiple archived versions are all included.

The two kinds of record have different outcomes:

- Active rows become unlinked **Fixed Amount** plans and keep their previous P/L in all currencies.
- Posted rows remain historical records. Their quantities, prices, discounts, amounts, currency values, P/L, and saved business details do not change. Only the financial-document reference and related allocation-link state are cleared.

Detach does not synchronize history to the current document, revalue its currency amounts, reopen a posted FWO, or change financial entries. Different historical versions retain their own values. Posted fields remain read-only; this controlled command does not permit ordinary editing of archived records.

Supported archived sources are **Order**, **Invoice**, **Credit Memo**, **Posted Invoice**, and **Posted Credit Memo**. Freight Order parent allocations, TMS correction chains, unresolved legacy data, and archived Quote, Blanket Order, or Return Order references are not supported by direct detach. Every member must pass the checks; a failure rolls back the whole command. See [Settlement](settlement.md#change-or-remove-a-document-link) and [Purchase Assignment](purchaseassignment.md#remove-or-change-an-assignment).

## Good to know

- FWO posting archives the saved Settlement values without recalculating them. Freight Order posting saves the complete parent Charges once with the trip. FWO-specific allocated costs remain in Posted Settlement; the shared Freight Order's parent amounts are not separate costs to add again for each linked FWO.

- When a report or integration reads a posted Freight Order's **Cost Amount (Actual)**, the total uses the saved **Amount (LCY)** of Charges belonging to that exact posted version, including any previously stored correction charges. Editing or deleting active Charges does not change this historical total. New financial corrections belong to the separate corrective FWO and do not change the original trip's Charges. The total follows the stored document signs; it is not a financial net cost after credits. Review Settlement P/L and the corrective-order links for the financial effect.

- Posted history is read-only. New invoice corrections use separate corrective FWOs. The existing **Detach Settlement** command can clear supported source references while preserving the financial snapshot.
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
