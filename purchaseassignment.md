---
title: "Purchase Assignment"
description: "Link vendor invoices and posted credit memos to planned Settlement costs, import new costs, and allocate one purchase line across Forwarding Orders."
---

# Purchase Assignment

Use purchase assignment to connect a vendor's Business Central purchase line to the transportation cost it supports. A link identifies the **document type, document number, and line number**. Entering a vendor invoice reference or attaching an invoice file alone does not create this financial link.

You can match an existing planned **Cost** line, import new Cost lines, or distribute a Purchase Invoice or Posted Purchase Invoice line across several Forwarding Orders. The FWO Settlement actions **Link to Purchase Line** and **Import Purchase Document** do not restrict unposted purchase document status; **Open** and **Released** documents are both included. **Charge Allocation** across Forwarding Orders and **Freight Order assignment** still require **Status = Released** for an unposted source. After linking, the purchase document supplies the actual quantity, price, discounts, currency, and amount. Posting an already linked unposted purchase document updates its link to the posted document. You can also link eligible **Posted Purchase Invoice** and **Posted Purchase Credit Memo** lines directly to Settlement after posting.

For the meaning of **Financial Effect**, **Pricing Mode**, and positive or negative amounts, start with [Settlement principles](settlement.md#settlement-principles).

Enter a planned Cost's quantity, price and discount as you would on the purchase document. An ordinary **Cost / Standard** with Quantity **1** and Price **500** has Amount **500** and P/L Amount **−500**, before discounts or VAT. TMS applies the financial direction automatically. Negative quantity or price remains available when the purchase line itself should be negative; see [Quantity, Price, and Amount signs](settlement.md#quantity-price-and-amount-signs).

## Before you start

Make sure that:

- the Forwarding Order exists and its status allows Settlement changes;
- for direct linking or importing existing lines, the purchase document is an unposted **Order**, **Invoice**, or **Credit Memo**, without a document-status restriction, or an eligible [Posted Purchase Invoice](#link-an-already-posted-purchase-invoice) or [Posted Purchase Credit Memo](#link-an-already-posted-purchase-credit-memo);
- **Buy-from Vendor**, **Pay-to Vendor**, posting date, currency, and the purchase lines are complete;
- each relevant [Charge](charges.md) has **Purchase Line - Type** and **Purchase Line - No.** matching the purchase line's **Type** and **No.**;
- the source line has not already been assigned through Settlement or a Freight Order;
- the source is within the [supported Settlement document scope](settlement.md#posting-and-supported-scope).

For initial linking, follow the document-specific source requirements below; prepayments, deferrals, and partially processed sources remain outside the supported flow. Use the corresponding posted-document actions when the invoice or credit memo has already been posted. The existing posted single-line link and whole-document import actions require prices excluding VAT and zero actual invoice discount. The posted invoice **Charge Allocation** action described below supports the source's VAT-inclusive entry mode and invoice discount. Comment lines cannot receive a new cost allocation. After linking, changes to the existing source line are authoritative: Settlement accepts its new VAT, discount, price, quantity, unit, description, location, and dimension values. Changing **Type / No.** on that same physical line does not require its old Charge mapping to match again.

**Create** and **Add to Document** have a different purpose: they create new purchase lines in a document with **Status = Open**. The **Released** requirement applies to **Charge Allocation** across Forwarding Orders and **Freight Order assignment**, while direct linking and whole-document import have no document-status requirement.

For direct Settlement assignment, the chosen purchase document supplies **Buy-from Vendor** and its name to the Cost line. Its previous vendor does not restrict single-line selection. **Pay-to Vendor** remains on the purchase document and can differ from Buy-from Vendor.

## Choose the right workflow

| Your situation | Action and result |
|---|---|
| The vendor invoice line already exists, and you already planned its cost in Settlement. | Use [Link to Purchase Line](#link-one-purchase-line-to-an-existing-cost). The current planned Cost line receives the existing purchase line. |
| The purchase document contains costs that have not yet been entered in Settlement. | Use [Import Purchase Document](#import-a-whole-purchase-document). TMS creates new Cost lines for its eligible, unallocated lines. |
| The invoice is already posted, and its cost is planned in Settlement. | Use [Posted Purchase Invoice Line](#link-an-already-posted-purchase-invoice). TMS replaces the current Cost's planned values with the posted line's actual values. |
| The invoice is already posted, and its costs are not yet entered in Settlement. | Use [Posted Purchase Invoice](#link-an-already-posted-purchase-invoice) under **Import Purchase Document** to create new posted Cost rows. |
| A vendor credit memo is already posted. | [Link its line to an existing Cost row, or import its free lines as new Cost / Credit rows](#link-an-already-posted-purchase-credit-memo). The source sets the Credit effect; a normal credit reduces cost. |
| One vendor invoice line covers several Forwarding Orders. | On the purchase line, use [TMS > Charge Allocation](#allocate-one-purchase-line-across-forwarding-orders) and choose the exact planned Cost line for each order. |
| You have planned Cost lines, but the purchase document or its lines do not exist yet. | Use [Create or Add to Document](#create-purchase-lines-from-planned-costs). These actions create purchase lines. |
| The carrier invoice belongs to a Freight Order and its related Forwarding Orders. | Use the [Freight Order assignment workflow](#assign-a-carrier-invoice-through-a-freight-order). |

**Add to Document** adds new purchase lines. To connect an invoice line that has already been entered, use **Import to Settlement** or **Charge Allocation**. Otherwise you can duplicate the vendor charge.

## Link one purchase line to an existing cost

Use this when, for example, you planned a carrier cost of 500 and now have the vendor's invoice for 520. The link replaces the planned values with the actual values on the same Settlement row. Review the purchase document before selecting its line; no **Release** step is required.

Your **User Setup** must have **TMS Allow Purchase Document Allocation** enabled to use **Link to Purchase Line**, including links to posted invoices and credit memos. If your User Setup record is missing or this option is off, both **Link to Purchase Line** and **Import Purchase Document** are disabled. Reopen the Forwarding Order after changing this option.

1. Open the **Forwarding Order** and expand **Settlement**.
2. Select the existing **Cost** line without a document link. Check the Charge mapping for the intended purchase line.
3. Choose the source document type. The source sets **Financial Effect** to **Standard** for a Purchase Order or Purchase Invoice, or **Credit** for a Purchase Credit Memo; the old effect does not restrict linking.
4. Choose **Import to Settlement > Link to Purchase Line > Purchase Invoice Line**. For another document type, choose **Purchase Order Line** or **Purchase Credit Memo Line**.
5. In **Purchase Document Assignment**, find the document using **Document No.** and **Vendor Invoice No.**. This FWO entry point lists unposted documents from all vendors without filtering their status. Check the source vendor before selecting its line.
6. Select the one purchase line that belongs to this cost and confirm the lookup. Check **Type**, **No.**, **Quantity**, **Direct Unit Cost**, **Line Amount**, and currency before confirming. Do not select a comment or an already allocated line.
7. Review the updated Settlement row. Confirm its document reference, **Financial Effect**, quantity, price, amount, currency, and dimensions.

The selected Charge must map to the purchase line's exact **Type** and **No.** Matching the description or amount is not sufficient. If several Charges use the same purchase posting object, selecting the required **Code** on the planned Cost line makes the intended Charge explicit.

This action works on the **current Cost line**, not a batch of selected Settlement rows. Repeat it for other invoice lines. A Cost row already linked to another source must first be detached through the supported workflow.

Old **Needs Review**, **Legacy**, or **Unresolved** markers and old planned values do not prevent linking when there is no conflicting document or allocation identity. The chosen source replaces the vendor, Financial Effect, quantity, price, discounts, amount, currency and exchange-rate context, dates, description, unit, location, and dimensions. Quantity uses the actual source quantity in **Per Unit** mode. Blank source location and dimensions clear the previous values. The owning Forwarding Order remains. A compatible existing Charge Code is retained; otherwise, the source's Type / No. must identify exactly one Charge, which replaces the old Code. TMS still checks source eligibility, existing assignments, and actual document, Freight, or correction references.

For a normal quantity-based line, TMS copies the purchase quantity and **Direct Unit Cost** and changes **Pricing Mode** to **Per Unit**. Amount is the source amount before line discount; Line Amount is after line discount. P/L applies the Cost and Standard/Credit direction to the actual amount excluding VAT after applicable discounts. Description, unit, currency, exchange-rate context, and dimensions also come from the source. The old estimate is replaced; it is not retained as another cost.

**Example:** a planned Cost with Amount 500 linked to a Purchase Invoice line with quantity 1, price 520, and no discount becomes **Amount = 520**, **Line Amount = 520**, and **P/L Amount = −520** on the same row. For a source with price 100 and a 20% discount, the row becomes Amount 100, Line Discount Amount 20, Line Amount 80, and P/L Amount −80. No extra Cost row is added.

## Import a whole purchase document

Use this when the purchase costs do not already have planned Settlement rows and all free lines on the selected document belong to the current Forwarding Order. Review the purchase document before importing it; no **Release** step is required.

Whole-document import requires a **User Setup** record with **TMS Allow Purchase Document Allocation** enabled for your user. An administrator can enable this option. It controls both the **Import Purchase Document** and **Link to Purchase Line** action groups, including posted documents. Reopen the Forwarding Order after changing this option.

1. Open the **Forwarding Order** and expand **Settlement**.
2. Choose **Import to Settlement > Import Purchase Document > Purchase Invoice**. Use **Purchase Order** or **Purchase Credit Memo** for those document types.
3. Select the unposted document. The list has no document-status filter. For invoices and credit memos, review **Buy-from Vendor**, the vendor reference, **Currency Code**, **Allocation Status**, and **Free Lines**.
4. Confirm the selection.
5. Read the result message. It reports how many Settlement lines were created and how many already allocated lines were skipped.
6. Review every new Cost line and the Settlement totals.

TMS creates one new Cost row for each eligible, unallocated purchase line. It ignores comments and skips lines already allocated elsewhere. If all eligible lines are allocated, there is nothing to import.

Whole-document import does **not** search for and replace existing planned Cost rows. If the same cost is already planned, use [single-line linking](#link-one-purchase-line-to-an-existing-cost) instead. If only some free lines belong to this Forwarding Order, link those lines individually.

Each imported purchase **Type / No.** must resolve to exactly one Charge. If there is no mapping, configure it in **Charges**. If several Charges have the same mapping, prepare a Cost row with the intended **Code** and use single-line linking.

| Allocation Status | Meaning |
|---|---|
| **Not Allocated** | None of the document's allocatable lines is linked. |
| **Partially Allocated** | Some lines are linked; **Free Lines** shows the remaining count. |
| **Fully Allocated** | All allocatable lines are linked. |
| **No Allocatable Lines** | The document has no cost-bearing lines to import. |

These counters describe line availability. TMS still checks the selected lines and Charge mappings when you confirm the import.

## Link an already posted purchase invoice

Use these actions when the invoice was posted before its transportation costs were assigned to Settlement. The link still identifies a specific invoice **line**. TMS reads the posted values without reposting the purchase or changing its financial entries.

Before linking, check that:

- the invoice is not **Cancelled**, **Corrective**, or a prepayment invoice;
- **Prices Including VAT** is off, actual invoice discount amounts are zero, and the source has no deferral or other processing outside the [supported Settlement scope](settlement.md#posting-and-supported-scope);
- the invoice line has not already been allocated in active or posted Settlement or Freight Order records;
- the posted line's **Type / No.** has a valid Charge mapping, and the existing Cost, if used, has no conflicting document, Freight, or correction references. A compatible existing Code is retained; otherwise the source must identify exactly one Charge.

Payment alone does not prevent linking: an otherwise eligible paid invoice can be used. **Pay-to Vendor** can differ from **Buy-from Vendor**; the posted invoice supplies Buy-from Vendor to Settlement.

### Replace an existing planned Cost

1. Open the **Forwarding Order** and expand **Settlement**.
2. Select the existing **Cost** row without a document link. Check its Charge **Code**; old status, vendor, Financial Effect, and planned amounts are replaced by the source.
3. Choose **Import to Settlement > Link to Purchase Line > Posted Purchase Invoice Line**.
4. Select the matching invoice line from the list of all vendors. Review its vendor, document number, Type, No., quantity, direct unit cost, amount, and currency before confirming.
5. Review the same Settlement row. It now has the posted invoice and line references, actual quantity, price, amount, currency, unit, and dimensions, and is marked **Posted**.

For example, a planned Cost with Amount **500** linked to a posted invoice line for **520**, without discounts, becomes Amount **520** and P/L Amount **−520** on the same row. No additional Cost is created. An already allocated source line cannot be linked again, including when its earlier FWO or Freight Order has been closed.

### Import new costs from a whole posted invoice

1. Open the **Forwarding Order** and expand **Settlement**.
2. Choose **Import to Settlement > Import Purchase Document > Posted Purchase Invoice**. Your **User Setup** must allow **TMS Allow Purchase Document Allocation**.
3. Select the invoice and review its vendor, reference, currency, **Allocation Status**, and **Free Lines**.
4. Confirm, then review the result message and the new Cost rows.

TMS creates one **Posted** Cost for each eligible free invoice line. Comments are ignored; lines already allocated in active or posted Settlement or Freight records are skipped. Each imported **Type / No.** must identify exactly one Charge. If any required mapping or source check fails, the import does not retain a partial set of new rows.

This action does not replace existing plans. Importing an invoice for **520** while a planned Cost with P/L **−500** remains creates another row with P/L **−520**, giving total P/L **−1,020**. Use the single-line action when the expense was already planned.

### Corrections and limits

**Correction Baseline Status = Linked After Posting** means that the purchase invoice was already posted when its Cost was linked to Settlement. The actual values are verified; the status records when the link was established.

Review the source carefully before confirming. To correct an eligible direct assignment, use [the detach actions on the Posted Purchase Invoice lines](#detach-a-posted-purchase-invoice). The **Detach Settlement** action in Settlement uses the same mechanism, including supported posted sources. Detaching an assignment does not reverse the invoice. To record a vendor credit, create a **Cost / Credit** row and the corresponding Purchase Credit Memo, or link an existing credit memo through the supported actions.

The Forwarding Order can still be closed and archived when its usual posting checks pass. Unknown prior correction history does not by itself block FWO closure.

To distribute a posted invoice line across several FWOs, use **TMS > Charge Allocation** on the Posted Purchase Invoice line. For a posted credit memo, use [the separate credit-memo actions](#link-an-already-posted-purchase-credit-memo); new split allocations from posted credit memos and posted Freight Order assignment are not provided by the posted invoice allocation action. Existing links established before posting continue through their normal posting and supported correction workflows.

## Link an already posted purchase credit memo

Use this to record a vendor credit that was already posted before its Settlement assignment. A normal posted credit memo line for **100**, without discounts, becomes **Cost / Credit**, **Amount = 100**, and **P/L Amount = +100**, reducing expense and increasing profit. A negative source amount remains negative in Amount; on Cost / Credit its P/L is also negative.

The credit memo itself must not be **Cancelled**. A **Corrective** or **Correction** credit memo can be linked when otherwise eligible: it can represent the credit that cancels or corrects an invoice. The source must have prices excluding VAT, zero actual invoice discount, and no prepayment, deferral, or other unsupported processing. A payment or application by itself does not prevent linking. The source line must be free in both active and archived Settlement and Freight records. The source supplies **Buy-from Vendor**, even when **Pay-to Vendor** differs.

The posted line also supplies **Location Code**: a blank source location clears the planned location. Linking and import preserve the actual posted amount, including when current currency rounding settings differ from those used by the posted document.

### Link one posted credit memo line

1. In the Forwarding Order **Settlement**, select an existing **Cost** without a conflicting document, Freight, or correction reference. The old vendor, status, and Financial Effect do not restrict selection.
2. Confirm the source's Charge mapping. A compatible existing **Code** is retained; otherwise the posted line's **Type / No.** must identify exactly one Charge. Linking sets the effect to **Credit** from the source, including when the plan previously used Standard.
3. Choose **Import to Settlement > Link to Purchase Line > Posted Purchase Credit-Memo Line**.
4. Select the required source line, checking its document number, vendor reference, quantity, price, amount, and currency.
5. Review the same Cost row. The posted source replaces the plan, including its description, quantity, price, unit, amount, currency context, and dimensions. No new Cost row is created.

For example, a planned Cost / Credit with Amount 80 linked to a posted credit memo line for 100 becomes Amount 100 and P/L Amount +100 on that same row, assuming no discounts.

### Import a whole posted credit memo

1. Choose **Import to Settlement > Import Purchase Document > Posted Purchase Credit-Memo**. Your **User Setup** must allow **TMS Allow Purchase Document Allocation**.
2. Select the credit memo. The list initially shows the highest document numbers first; review the vendor, reference, currency, **Allocation Status**, and **Free Lines**.
3. Confirm and review the new Cost rows and the import result.

TMS creates one **Cost / Credit** row per eligible free source line. Comments are ignored, and already allocated lines are skipped, including allocations in transportation history or Freight Orders. Each **Type / No.** must identify exactly one Charge. A mapping or source validation failure rolls back the whole import.

Whole-document import does not replace planned credit rows. If a credit with P/L **+80** was already planned, importing a source credit of **100** adds another P/L **+100**; total P/L becomes **+180**. Use the single-line action to replace the plan.

Both actions establish a verified **Posted** link with **Correction Baseline Status = Linked After Posting**. They do not repost the credit memo or change its financial entries. Review the source carefully. Eligible direct Posted Purchase Credit Memo links can be removed through **Detach Settlement** in Settlement; the same command includes any supported archived members of that source group. FWO closure follows the usual posting checks.

## Allocate one purchase line across Forwarding Orders

Use this when one vendor invoice line is shared by several jobs. TMS splits the line's quantity and amounts across the selected Settlement rows; their total remains equal to the source line.

1. In each affected **Forwarding Order**, prepare a free **Cost** row without an existing document, Freight, or correction reference. Its **Customer/Vendor No.** must exactly match the source document's **Buy-from Vendor No.**, and its existing, nonblank **Charge Code** must map to the source line's **Type / No.**
2. Open the **Purchase Invoice** and choose **Release**, or open the **Posted Purchase Invoice** if it has already been posted.
3. Select the one purchase line to distribute.
4. In the line actions, choose **TMS > Charge Allocation**. The current source line must have no existing Settlement or Freight allocation, including posted transportation history. An unposted invoice must also be **Released**; a posted invoice has no Release step.
5. In **Select Allocation**, choose **Distribution Type**. Options include **Equal**, **By Number Of Logistic Units**, **By Gross Weight**, **Selected Forwarding Orders**, and **By Footage**. Choosing a distribution type resets the selection, so review the **Select** checkboxes afterward.
6. Select only the Forwarding Orders that should receive this cost. The list contains orders with at least one free Cost row for the same **Buy-from Vendor** and a matching existing Charge mapping. A blank or different vendor, a blank Charge Code, or an incompatible Charge mapping excludes that row.
7. Check **Settlement Line No.**, **Charge Code**, and **Settlement Description** for every selected order. TMS selects the row automatically only when that FWO has exactly one eligible target. If several qualify, use **Settlement Line No.** to choose the exact row. Mass allocation keeps its existing Charge Code; it does not replace an incompatible code with another mapping.
8. Review **Charges Distribution Ratio**. Every selected order must receive a positive share and the complete allocation must total 1 (100%). For weight, footage, or unit-based distribution, check that the underlying values are complete.
9. Confirm the allocation and review **Allocated Amount** on the purchase line and the linked Cost rows in each Forwarding Order.

On purchase invoice and credit-memo lines, including posted documents, **Allocated Amount** shows one total for all linked active and posted Settlement rows. It uses net amounts excluding VAT in the document currency, with the document sign. The former separate **Allocated Amount (Posted)** column is hidden.

Choose **Allocated Amount** to open its details. Active-only links open **Settlement Lines**; posted-only links open **Posted Settlement Lines**. If the document line has both, **Linked Settlement Lines** shows them together with **FWO Status** and **Posted Version**. Every linked historical version is included, using the same scope as the displayed total. Choose **Forwarding Order No.** to open the order or its exact posted version; in the combined list, **Settlement Line No.** opens the individual Settlement row. Zero totals can also be opened when linked rows exist.

Sales invoice and credit-memo lines, including posted documents, provide the same **Allocated Amount** column and navigation for their linked Income rows.

The dialog selects **one Settlement target per Forwarding Order** for this purchase line. It does not automatically select the first Cost row or create missing planned rows. Repeat the process separately for other purchase lines.

The vendor match uses **Buy-from Vendor**, not **Pay-to Vendor**. For example, an invoice bought from vendor A but paid to vendor B lists only eligible Costs for vendor A. A blank vendor is not a wildcard. These strict target-selection rules apply to multi-FWO **Charge Allocation**; the separate single-line link and import workflows retain their documented source-authoritative behavior.

The allocation command checks the selected persisted targets again before applying the distribution. If a selected Cost is no longer free or its vendor or existing Charge mapping no longer matches, allocation stops instead of selecting another row or replacing its code. Correct the target or reopen the selection and review it again.

For a **Posted Purchase Invoice**, the command updates the selected active Cost rows with **Document Type = Posted Invoice**, the source invoice/line references, **Posted = Yes**, and each allocation factor. Quantities and amounts are split; the source price and discount percentage are retained. The posted invoice header, its lines, and financial entries are not modified. Paid invoices can be used; cancelled, corrective, correction, prepayment, and deferral sources are rejected. VAT-inclusive input amounts and invoice discounts are copied from the posted source, while P/L remains excluding VAT. No new Cost rows are created. A failure in any target rolls back the complete group.

Invoice allocation ratios keep high precision internally. **Equal** for three orders uses thirds rather than rounding each factor to 0.33; the allocator rounds monetary values and assigns the remaining cents to the last target in FWO/Settlement order. The displayed ratio can show fewer decimal places than the stored factor. **By Gross Weight** requires a positive total selected weight.

**Example:** a Purchase Invoice line for 1,000 without discounts split equally between two Forwarding Orders gives each row Amount 500, Line Amount 500, and P/L Amount −500. A source amount of 100 split three ways gives document amounts **33.33, 33.33, 33.34**. The last share receives the rounding remainder for each component and for LCY/ACY totals so the group matches the source. Earlier rounded shares cannot consume more than the remaining source amount, so rounding does not reverse the final share's sign. A very small invoice amount of 0.02 split equally four ways can produce Costs of **-0.01, -0.01, 0, 0**: positive allocation ratios can result in zero rounded amounts.

A purchase line cannot simultaneously have a direct Settlement allocation and a Freight Order charge allocation. An existing allocation must be removed through its own workflow before assigning the line again.

The same existing-link check runs before opening **Select Allocation** and in the allocation command, including a second check after it locks the source and targets. An already assigned line raises an error; the command does not silently return or extend an existing allocation. This check includes zero-amount links and every related FWO, irrespective of the proposed targets, vendor, or Charge Code. A retained physical source-line reference also keeps the line assigned even if its saved document reference is inconsistent.

## Cost signs and vendor credits

Enter a normal purchase invoice or credit memo using the usual Business Central quantity and direct unit cost. TMS derives the Settlement direction from the source document type.

| Source line, without discounts | Financial Effect | Amount / Line Amount | P/L Amount |
|---|---|---:|---:|
| Purchase Order, Purchase Invoice, or Posted Purchase Invoice | Standard | 100 | −100 |
| Purchase Credit Memo or Posted Purchase Credit Memo | Credit | 100 | +100 |

For a planned **Per Unit** Cost, quantity **1** and price **100** produce **Amount = 100**. In **Fixed Amount**, enter **Amount = 100** for the same cost before discount. Standard gives P/L −100 and Credit gives P/L +100, assuming no discounts. See [quantity, price, and amount signs](settlement.md#quantity-price-and-amount-signs).

Negative quantities or prices reverse the arithmetic as well. They are not a substitute for selecting **Credit**, and changing the sign of a linked Settlement row is not how you create a vendor credit.

To record a new vendor credit, create a **Purchase Credit Memo** from separate **Cost / Credit** rows, link an existing unposted memo without a Release requirement, or use [the posted credit-memo actions](#link-an-already-posted-purchase-credit-memo). This records the cost reduction separately from the invoice. To credit an already posted linked invoice, create a normal purchase credit memo and link or import its Settlement rows. There is no Full Reversal command. Detaching a posted invoice's Settlement assignment only removes the analytical link; it does not reverse the purchase or its financial entries.

## Create purchase lines from planned costs

Use these actions only when the required purchase lines have not been entered yet:

- **Create > Purchase Order** or **Create > Purchase Invoice** creates a new document from compatible planned **Standard** Cost lines.
- **Create > Purchase Credit Memo** creates a new document from compatible planned **Credit** Cost lines.
- **Add to Document > Select Purchase Document** adds the current or selected Cost rows as new lines to a compatible document with **Status = Open**. **Standard** offers orders and invoices; **Credit** offers credit memos.

These actions create and link new purchase lines while the document is **Open**. You do not need to release a document before TMS creates its new lines. To match existing lines instead, use the corresponding direct link or import action without a status restriction. Release an unposted source before **Charge Allocation** or **Freight Order assignment**.

For **Add to Document**, select only the intended unlinked Cost rows. They must match the selected document's Buy-from Vendor, Financial Effect, currency, and currency factor. Do not mix Standard and Credit in one selection. Creating a new Purchase document also requires compatible posting dates and dimensions across its Cost rows.

If the invoice line is already present, return to [single-line linking](#link-one-purchase-line-to-an-existing-cost) instead of adding it again.

## Review, edit, and post linked documents

After assignment, check the document reference and Settlement totals before posting. Linking is not posting: an Open or Released linked invoice remains unposted until you post it in Business Central.

You can **Reopen** a linked purchase document for supported edits. Saving changes to description, quantity, unit, dimensions, **Direct Unit Cost**, line discounts, or **Line Amount** refreshes the complete Settlement allocation. TMS also refreshes older linked snapshots before posting. Review every affected Forwarding Order when a line is shared. The purchase document remains the source of these actual values. Direct linking and import do not require it to be released again; **Charge Allocation** and **Freight Order assignment** still require a Released unposted source.

Changes to the same physical source line, including **Type / No.**, refresh Settlement from that source. Changing allocation membership or replacing the source line requires the supported detach and reassignment workflow. An ordinary deletion of a linked purchase line is blocked: detach first. Standard header changes such as a currency change can temporarily delete and recreate lines; that Recreate operation preserves the link and refreshes it to the recreated line. After financial posting, the actual posted line supplies the final quantity, price, discounts, amounts, currency, description, unit, location, and dimensions for every Settlement share. Allocation shares and Forwarding Order references are preserved.

Post the linked source in full. For a linked **Purchase Order**, choose **Receive and Invoice** for the full quantity in one posting operation. Separate receipt posting is unsupported even if you receive the full quantity.

Partial receipts, partial invoicing, combined receipts, item tracking, warehouse processing, prepayments, and deferrals are outside the supported Settlement flow. After successful posting, TMS changes the existing source reference to the **Posted Purchase Invoice** or **Posted Purchase Credit Memo** and marks the linked Settlement rows as posted. Open the related document from Settlement to verify it.

For a source that was posted before assignment, use [the posted-invoice workflow](#link-an-already-posted-purchase-invoice) or [the posted credit-memo workflow](#link-an-already-posted-purchase-credit-memo). These create direct posted links and require no further purchase posting.

## Remove or change an assignment

Use **Detach Settlement** for a direct purchase assignment. The command is the same whether you start in Settlement or on the financial document.

On Purchase Invoice lines, **Detach Settlement** is enabled only when the current line has linked active or posted Settlement rows. A link on another invoice line does not enable it. **Detach All Lines** checks the complete invoice instead.

1. Select the linked Cost row in **Settlement** and choose **Import to Settlement > Detach Settlement**. Alternatively, select the source purchase line and choose **TMS > Detach Settlement** where that action is available. Detach does not require **Reopen**: an eligible unposted document can remain **Released**, because its source line is not changed.
2. Review the confirmation. It applies to the **complete direct allocation of the source line**, including other FWOs and all supported posted versions.
3. Confirm and review all affected orders. Active Cost rows become unlinked **Fixed Amount** plans with their existing P/L and currency values. Archived rows retain their complete financial snapshots and lose only the source and allocation references.
4. Edit an active plan or choose its replacement source as needed. A direct replacement link has no document-status requirement. Release an unposted source before a new **Charge Allocation** or **Freight Order assignment**. Delete an unwanted unposted source line separately after detaching.

Detach does not delete the purchase line or restore an earlier estimate. A Cost with **P/L Amount = −520** remains at **−520** after detach, even if the original estimate was −500. Current exchange rates are not applied during detach. If saved values depend on VAT or invoice-discount context, the active plan uses its saved net amount excluding VAT as the new Amount base; its P/L stays unchanged. The corresponding archived values are not normalized or recalculated.

To detach the complete Purchase Invoice, use **TMS > Detach All Lines** next to **Detach Settlement**. The command includes every source line with a direct reference, regardless of selection or page filters. It is available for active or archived references. All groups are checked before any change; failure rolls back the entire operation.

For example, one purchase line is allocated to **five FWOs: three posted and two active**. Detaching that source line clears every related physical reference: both active rows and all archived versions of the other three orders. The active rows remain plans with their previous P/L. Posted quantities, prices, discounts, amounts, currency amounts, and P/L remain exactly as saved. Closing a FWO does not cause its assignment to be skipped.

Supported archived source types are Order, Invoice, Credit Memo, Posted Invoice, and Posted Credit Memo. Freight Order parent allocations, TMS correction history, unresolved legacy links, and archived Quote, Blanket Order, or Return Order references are outside this direct-detach workflow. Active FWO status and source-processing checks still apply. Complete the amount-model wizard for rows that require conversion before detaching them.

### Detach a posted purchase invoice

Use this to remove a direct Settlement assignment while keeping the Posted Purchase Invoice and its financial entries. Links established before or after invoice posting use the same command. Archived members are included, even when no active Settlement rows remain.

1. Open the **Posted Purchase Invoice** and go to **Lines**.
2. Choose **TMS > Detach Settlement** for the current line's complete group, or **TMS > Detach All Lines** for the whole invoice.
3. Confirm once. TMS checks the source and every active and archived member before writing; a failure leaves all references intact.
4. Review the retained active plans and posted history. All affected source references are cleared. Existing generated-row and agreement controls still apply to the active plans.

An enabled action means references exist; it does not guarantee that the complete group is eligible. Cancelled or corrective invoices, Freight allocations, TMS correction chains, invalid identities, and unsupported source processing remain blocked. Payment alone does not prevent detach.

Both actions preserve posted invoice lines and amounts, vendor ledger entries, and payment applications. The **Detach Settlement** action in Settlement calls the same mechanism; it is not limited to unposted purchases. These commands change the assignment only. Use a Purchase Credit Memo when the vendor's charge itself must be credited.

### Detach a posted purchase credit memo

In the **Posted Purchase Credit Memo** line actions, open **TMS** and choose:

- **Detach Settlement** to remove all direct Settlement links of the current line, including active FWOs and every linked posted FWO version.
- **Detach All Lines** to remove all direct Settlement links of the credit memo, including lines outside the current selection or filters.

The current-line action is enabled only when that line has Settlement links. The all-lines action is enabled when any line in the document has links, even if the selected line is unlinked. Zero amounts still count as links. After confirmation, the same common command used from Settlement removes the assignments: active rows become plans, posted FWO amounts stay unchanged, and the credit memo and its financial entries remain. Existing source and correction checks still apply. **Charge Allocation** is not offered on posted purchase credit memos.

Review the retained plans before linking a replacement to avoid counting an expense twice. Use **Posted Purchase Invoice Line** or **Posted Purchase Credit-Memo Line**, as appropriate, to link a suitable active Cost to its replacement source. Archived Settlement remains a historical record; detach does not make it an editable plan or reopen its FWO.

## Assign a carrier invoice through a Freight Order

Use this route when the purchase cost should first appear in **Freight Order Charges** and then be allocated to that Freight Order's related Forwarding Orders. Complete and **Release** the existing Purchase Invoice before assigning it.

1. Open the **Freight Order** and check its **Pay-to Vendor No.** and assigned Forwarding Order stages.
2. In **Charges**, choose **Assignment > Purchase Document**.
3. In **Purchase Document Assignment**, select the unallocated invoice lines for this Freight Order. This entry point lists **Released**, unposted Purchase Invoices whose Buy-from and Pay-to Vendor both match the Freight Order's Pay-to Vendor.
4. Confirm the selection, then choose the distribution type and participating Forwarding Orders in **Select Allocation**.
5. Confirm and review the new Freight Order charges and the allocated Cost rows in each Forwarding Order's Settlement.

This workflow creates Freight charge records and their Settlement allocations. Do not also import the same purchase line directly into Settlement. If Buy-from and Pay-to Vendor differ, use the direct Settlement workflow appropriate to the cost.

The direct **Detach Settlement** command does not detach Freight Order parent allocations. A linked Freight charge, or a charge with Settlement children, is protected from ordinary deletion. The **Detach Settlement** action on the purchase assignment page uses the same direct-source mechanism; it does not add a separate Freight parent detach workflow. If a Freight assignment is wrong, review the complete Freight allocation with the responsible accounting or TMS administrator before making a correction.

## Troubleshooting

| Problem | What to check |
|---|---|
| The invoice is not listed for single-line linking. | Use **Purchase Invoice Line** for an unposted invoice, or **Posted Purchase Invoice Line** for a posted invoice. The FWO single-line selector has no status filter and shows all vendors; check the relevant source eligibility rules and existing allocations. |
| An unposted purchase cannot be linked, imported, or allocated. | Direct **Link to Purchase Line** and **Import Purchase Document** actions have no document-status requirement; check source eligibility and existing links. **Charge Allocation** and **Freight Order assignment** require a Released source. **Create** and **Add to Document** create new lines in Open documents. |
| **Import Purchase Document** and **Link to Purchase Line** actions are disabled. | Ask an administrator to check your **User Setup** record and **TMS Allow Purchase Document Allocation**, then reopen the Forwarding Order. |
| An order is missing from **Select Allocation**. | Check that it has a free Cost with Customer/Vendor No. equal to the source Buy-from Vendor and an existing, nonblank Charge Code mapped to the source Type / No. Blank or different vendors and blank or incompatible codes are excluded. If several rows qualify, select the exact Settlement Line No. |
| TMS asks for **Settlement Line No.** | More than one compatible Cost row exists. Choose the intended row with the lookup. |
| The source Charge cannot be resolved. | A compatible existing Code is retained. Otherwise, configure exactly one Charge whose **Purchase Line - Type / Purchase Line - No.** matches the source **Type / No.**. Similar descriptions or equal amounts do not establish a match. |
| Whole-document import reports several matching Charges. | Use a planned Cost row with the intended **Code** and single-line linking, or make the Charge mapping unambiguous. |
| Cost appears twice after import. | Whole-document import creates new rows. Check whether the same cost was already planned; use single-line linking when replacing a plan with an actual source. |
| A source line is already allocated. | Review its existing Settlement or Freight allocation, including posted transportation history. Closing an FWO or Freight Order does not make the source line free. |
| Ordinary costs increase profit. | Check **Financial Effect** and the signs of Quantity, Price, and Amount. An ordinary positive purchase line uses Standard with positive quantity, price, and Amount; its P/L Amount (LCY) is negative. |
| Allocation fails with a zero share. | Check selected orders and the values used by Distribution Type. Every selected share must be greater than zero. |
| A linked purchase line cannot be changed or deleted. | Reopen the document when standard Business Central requires it. Saved changes to the existing line update Settlement automatically, including VAT and invoice discounts. For deletion, detach the complete eligible allocation first. |
| The document has already been posted. | Use [the posted invoice actions](#link-an-already-posted-purchase-invoice) or [the posted credit-memo actions](#link-an-already-posted-purchase-credit-memo). The source sets Standard or Credit on the existing Cost. |
| A posted credit memo line cannot be linked to a Cost. | Check Charge mapping, actual document/Freight/correction references, active and archived allocations, and source eligibility. The old vendor, Financial Effect, or Needs Review marker alone does not prevent linking. |
| An old Cost is marked Needs Review or Legacy. | If it has no conflicting source or allocation identity, link the actual purchase line directly. Its source values replace the old plan and verification state; recreating the Cost first is unnecessary. |
| A paid invoice cannot be linked. | Payment alone is permitted. Check cancellation/corrective status, invoice discounts, supported source processing, Charge mapping, and existing allocations. |
| Detach Settlement does not apply to the row. | Check its source identity and amount-model conversion. Eligible active and archived direct invoice references use the same command. Freight allocations, TMS correction chains, unresolved links, and unsupported archived document types remain excluded. |
| Posted invoice detach is enabled but fails. | Check the reported source, active FWO status, verification state, Freight or TMS correction references, cancellation, and stale identities. Archived members alone do not block detach. A failed Detach All Lines operation keeps all links intact. |

## Related

- [Settlement](settlement.md)
- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Charges](charges.md)
- [Freight Agreements](freightagreement.md)
- [Posted History](postedhistory.md)
