---
title: "Settlement"
description: "Enter quantities and prices as on the target document; TMS calculates their effect on profit automatically."
---

# Settlement

Use **Settlement** on a Forwarding Order to plan customer income and vendor costs, connect them to Business Central documents, and review profit.

**Enter Quantity and Price exactly as you would on the target sales or purchase document.** In Fixed Amount mode, enter the document amount before discount. Choose **Type** and **Financial Effect**; TMS calculates the effect on profit automatically in the **P/L Amount** fields. An ordinary vendor cost uses positive quantity, price and Amount, just like its purchase invoice line.

Start with [Settlement principles](#settlement-principles), then follow [Add an Income line](#add-an-income-line) or [Purchase Assignment](purchaseassignment.md) for the task you need.

> **Entering an ordinary cost:** Quantity **1**, Price **100**, and Amount **100** describe the purchase line. With a **20%** line discount, Line Amount is **80** and P/L Amount is **−80**. Use **P/L Amount (LCY)** to compare income and costs across currencies.

## Before you start

Make sure that:

- the Forwarding Order status allows Settlement changes;
- **Ordering Party** identifies the customer when you will create sales documents;
- [Services](services.md) have the required **Invoice Line - Type**, **Invoice Line - No.**, and unit of measure;
- [Charges](charges.md) have purchase mappings when you will create or link purchase lines;
- customers, vendors, posting groups, VAT setup, currencies, and exchange rates are ready;
- [Freight Agreements](freightagreement.md) are configured if carrier rates come from agreements;
- posting controls are configured in [TMS Setup](setup.md).

If a Settlement row shows **Settlement Upgrade Required**, an administrator must complete the [manual amount-model upgrade](setup.md#upgrade-existing-settlement-amounts). The indicator is shown on Settlement rows, not in the FWO header, list, or FactBoxes. New document fields and P/L totals are incomplete while legacy rows remain. Installing the extension does not automatically run this conversion.

Enter and edit lines in the Forwarding Order **Settlement** section. The separate **Settlement Lines** list is read-only and is useful for reviewing document links and verification status.

## Settlement principles

### What a line represents

A Settlement line is one income or cost contribution to a Forwarding Order. It can start as a planned amount before an invoice exists. Creating a Settlement line does not post a sales or purchase invoice.

| Setting | Question it answers | Values |
|---|---|---|
| **Type** | Which side of the job does this line belong to? | **Income** for customer services; **Cost** for vendor or carrier charges. |
| **Code** | What is being billed or costed? | A Service for Income; a Charge for Cost. |
| **Financial Effect** | Is this normal billing or a credit? | **Standard** for an Order or Invoice; **Credit** for a Credit Memo. |
| **Pricing Mode** | Which values do I enter? | **Per Unit**: Quantity and Price. **Fixed Amount**: Amount before discount. |
| **Document Type / Document No.** | Which financial document is connected? | Filled by creating, importing, or linking a document line. |

A credit does not change the side: a customer credit remains **Income**, and a vendor credit remains **Cost**.

### Financial Effect

Choose **Financial Effect** for the intended document. TMS applies the corresponding direction to the line's net amount when calculating P/L. For a positive document amount:

- **Standard** is the normal direction: customer income increases profit; vendor cost reduces profit.
- **Credit** reverses that direction: a customer credit reduces income; a vendor credit reduces cost and increases profit.

Financial Effect selects the direction and compatible document family. It does not choose between an Order and an Invoice. Use the exact action under **Create**, for example **Sales Invoice**.

The sign of Amount alone does not identify a Credit Memo. Negative quantities or prices can produce a negative line on an ordinary invoice. Select **Credit** when the intended document is a Credit Memo.

### Pricing Mode

| Mode | Enter | TMS calculates | Use when |
|---|---|---|---|
| **Per Unit** | **Quantity**, **Unit of Measure Code**, and **Price** | **Amount = Quantity × Price**, then discount and P/L amounts. | A rate per trip, pallet, hour, kilometre, or weight unit. |
| **Fixed Amount** | **Amount before discount**, plus quantity and unit. | Price from Amount and quantity. Changing quantity preserves Amount; changing discount changes Line Amount. | A negotiated base total that must stay fixed when quantities change. |

In **Per Unit**, Amount is calculated and cannot be edited directly. In **Fixed Amount**, Price is calculated and cannot be edited directly.

For a simple fixed fee, use **Quantity = 1**. A deliberately entered Fixed Amount with quantity zero can create a financial line with quantity 1 to carry the total. Once linked, Settlement adopts the actual quantity and Per Unit mode. Unit of Measure Code is still required for Income. Arbitrary existing document lines with zero quantity and a nonzero amount cannot be imported.

### Quantity, Price, and Amount signs

**Use document values when entering the line; read its financial effect in P/L.** You do not need to add a minus to represent a cost or manually reverse a credit. Enter the same quantity, price and discount that belong on the intended invoice, order or credit memo, and select the appropriate Type and Financial Effect.

**Amount** is the document amount before line discount. **P/L Amount** applies the financial direction automatically. These examples all use **Quantity = 1** and **Price = 100**, one currency, and no VAT or discounts:

| Type | Financial Effect | Amount | Line Amount | P/L Amount | Intended document |
|---|---|---:|---:|---:|---|
| Income | Standard | 100 | 100 | +100 | Sales Order or Sales Invoice |
| Income | Credit | 100 | 100 | −100 | Sales Credit Memo |
| Cost | Standard | 100 | 100 | −100 | Purchase Order or Purchase Invoice |
| Cost | Credit | 100 | 100 | +100 | Purchase Credit Memo |

For a Per Unit draft, **Amount = Quantity × Price**. In both modes, **Line Amount = Amount − Line Discount Amount**.

**P/L Amount = net amount excluding VAT after applicable discounts × direction**. Direction is +1 for Income Standard and Cost Credit, and −1 for Cost Standard and Income Credit.

For an ordinary fixed carrier cost of 700, enter **Cost / Standard**, **Quantity = 1**, and **Amount = 700**. The calculated Price is 700 and P/L Amount is −700.

**Negative quantities and prices are still supported.** Enter a negative value when that is the value you want on the document line. The optional **TMS Setup > Finance and Invoicing > Settlement > Make Settlement Input Positive** setting is off by default. When enabled, new negative Quantity, Price and Amount inputs become positive and a Message explains why. Turn it off temporarily when a negative document value is intentional. Existing values, source-document synchronization and calculated P/L keep their signs; see [Settlement input signs](setup.md#settlement-input-signs).

In Per Unit mode, Amount is negative when exactly one of Quantity and Price is negative. With the setting off, these **Cost / Standard** lines have no VAT or discounts:

| Quantity | Price | Amount | P/L Amount |
|---:|---:|---:|---:|
| −1 | 100 | −100 | +100 |
| 1 | −100 | −100 | +100 |

The negative document line reduces the cost, so its P/L is positive. It remains a standard invoice line; a negative value does not select a credit memo. In Fixed Amount mode, enter a negative Amount when the document total should be negative. Changing Quantity in that mode keeps Amount fixed and derives Price.

Creating a document preserves the entered signs. Linking or synchronizing an existing line copies its actual signs back to Settlement; allocated rows receive their share. TMS applies the financial direction only when calculating P/L. Edit the values directly in Settlement while the plan is editable, or in the source document after linking.

Changing **Financial Effect** on an editable draft preserves quantity, price, Amount, and discounts and reverses P/L. A fixed customer credit of 100 uses Income / Credit and Amount 100.

### Discounts and document values

**Line Discount %** applies to Amount in both pricing modes. It is hidden by default and can be shown through personalization. The exact **Line Discount Amount** is stored separately to preserve document rounding; it is not currently shown as an editable page field.

For quantity 10, price 100, and discount 20%, Amount is 1,000, Line Discount Amount is 200, and Line Amount is 800. A Cost / Standard row has P/L Amount −800 before currency conversion. Creating a document transfers the discount with the quantity and price. A line discount does not require Fixed Amount.

**Invoice Discount Amount**, **Net Amount Excl. VAT**, and **Amount Including VAT** store the remaining document values directly. Net Amount Excl. VAT is the base for P/L; it includes all applicable discounts. An unlinked plan does not estimate VAT, so Amount Including VAT initially equals Line Amount. **Quantity (Base)** stores the quantity in the base unit of measure. The eight older Source quantity and monetary fields are retained only for old-data conversion and are not used for current calculations or synchronization.

**Line Amount** is after line discount and before invoice discount. If the source has an invoice discount, P/L uses the actual amount after that discount. If **Prices Including VAT** is enabled on an already linked document, mirrored Amount and Line Amount include VAT while P/L excludes VAT. Initial import and linking restrictions still apply; see [Posting and supported scope](#posting-and-supported-scope).

### How profit is calculated

Totals use **P/L Amount (LCY)** to compare different transaction currencies. P/L Amount is meaningful across rows only in the same transaction currency. **P/L Amount (ACY)** converts the LCY contribution to the additional reporting currency configured in TMS Setup. When that currency is blank, ACY equals LCY, including its sign and allocated rounding remainder. The value is stored for reporting and API use even though the P/L Amount (ACY) column is shown only when **TMS Setup > Additional Reporting Currency** is filled. This common calculation is used when linking and synchronizing open or posted document lines and when the existing conversion wizard reflects those source lines into Settlement.

Both P/L Amount (LCY) and P/L Amount (ACY) are calculated for an unlinked planned line when it is created or its financial inputs change. The plan uses its own currency factor and exchange-rate date, initialized from the forwarding order context. Linking replaces the plan with source-document values, including the document's currency factor and date, and recalculates the P/L amounts through the same amount service.

Changing **TMS Setup > Additional Reporting Currency** recalculates stored reporting amounts from LCY. For linked allocations, rounding preserves the converted group total and each share's sign; a small share can become zero in ACY. This keeps the Settlement row, purchase link, quantity and allocation factor. For example, four shares of 0.01 LCY converted at 0.5 ACY per LCY with precision 0.01 receive 0.01, 0.01, 0 and 0 ACY. Their Cost P/L values are -0.01, -0.01, 0 and 0 ACY. Consistent active and posted copies of the same share receive the same reporting amounts.

| Example line | P/L Amount (LCY) |
|---|---:|
| Customer income | +1,000 |
| Carrier cost | −700 |
| **Total Profit (LCY)** | **300** |

Total Cost (LCY) is already signed. Profit is **Total Income (LCY) + Total Cost (LCY)**; do not subtract a negative cost a second time. Amount (LCY) is the gross document value and is not a profitability measure.

When the FWO has linked corrective orders, **Result Including Corrections** appears below these totals. It shows the order's own profit, **Corrections (LCY)**, and **Profit Including Corrections (LCY)**. Choose **Corrections (LCY)** to open the corrective orders. The same group is available in Posted FWO Settlement, using the saved profit of the exact posted version. These totals apply to the whole FWO, including when a line filter hides its Settlement rows.

Planned and linked lines both affect the result. Link an invoice to its existing planned Cost; importing another row can count the cost twice.

## How Settlement relates to documents

A link connects a Settlement row to a specific Sales or Purchase **line**, not just to an invoice number. A purchase line can also be allocated across several Forwarding Orders.

| Stage | What the amounts mean | Where to make changes |
|---|---|---|
| Planned, unlinked | Expected income or cost. | Edit a manual or Default Service Set draft in Settlement. |
| Linked to an unposted document | Values from the actual document line, including discounts, VAT, and currency context. The document is not yet posted. | Edit the source document using standard Business Central actions; reopen it first if needed. Settlement follows the saved line. |
| Linked to a posted document | Values and references from the posted invoice or credit memo. | Use the supported correction workflow. Posted amounts are not editable. |
| Posted Forwarding Order | A read-only Settlement snapshot in transport history. | Review the posted records; do not edit history. |

Linking replaces the old plan with the actual document values, even when quantity, price, signs, discounts, or currency differ. Amount mirrors the source amount before line discount; Line Amount mirrors the amount after line discount. P/L uses the net amount excluding VAT after all applicable discounts and applies the Income/Cost and Standard/Credit direction. Allocated rows show their share of the source. Linked fields are read-only in Settlement; make further changes in the financial document.

Explicit linking also records the current document header and line SystemId values. Old identifiers retained from another database do not override the selected source. Existing links to a different document must still be detached first. The conversion wizard likewise refreshes these identifiers for rows it converts when the source exists; a missing source on a posted FWO retains its historical reference.

During amount-model conversion, a missing source on an active FWO is detached automatically. The selected Settlement becomes Unlinked and no longer carries the missing document reference, while its saved P/L is retained. A normal direct line becomes a Fixed Amount plan. Independent Freight, correction and generation relationships keep their own management rules. This conversion repair does not change the ordinary Detach action's checks.

**Posted Source Deleted** distinguishes a recorded deletion from an unexplained missing link. When a posted sales or purchase invoice, credit memo, or its linked line is deleted through Business Central, TMS sets this read-only flag on linked active Settlement and every linked posted version. Document references, quantities, amounts, P/L and posted status remain unchanged. A valid posted snapshot is not downgraded merely because the source was deleted, and it can still be archived with its FWO.

The flag is retained in posted history and the API. A confirmed new source link or Detach clears it. Open-document deletion and Recreate do not set it. Deletions before this feature was installed are not inferred or backfilled. In the conversion wizard, recorded deleted-source history retains its link and saved amounts even on an active FWO; the automatic detach rule above applies to missing sources without this recorded-deletion flag.

The actual document quantity is represented as **Per Unit** after linking, even if the draft was Fixed Amount or used a zero-quantity fixed fee.

These actions have different purposes:

| Starting point | Action | Result |
|---|---|---|
| Settlement has unlinked lines to bill. | **Create** → the required Sales or Purchase document. | Creates a new document and links its new lines. |
| Settlement has the lines; an open financial document exists but needs new lines. | **Add to Document** → **Select Sales Document** or **Select Purchase Document**. | Adds new financial lines to that document and links them. |
| A purchase invoice already contains the cost line; Settlement has its planned Cost. | **Import to Settlement** → **Link to Purchase Line**. | Connects the existing purchase line to the current Cost line. |
| A financial document contains lines that are not yet in Settlement. | **Import to Settlement** → the appropriate import action. | Creates Settlement rows from eligible existing document lines. |

After **Create** successfully creates a Sales or Purchase Order, Invoice, or Credit Memo, TMS asks: **Do you want to open the created document?** Choose **Yes** to open its card or **No** to stay on the Forwarding Order; the document and its Settlement links are saved either way. The Forwarding Order header actions **Create Sales Invoice** and **Create Credit Memo** offer the same choice. Creating a combined sales invoice from selected Forwarding Orders asks once, after all selected orders have been processed.

**Add to Document** does not match an existing invoice line. Use the single purchase line link when both sides already exist.

New links to existing unposted documents follow **TMS Setup > Finance and Invoicing > Settlement > Sales Document Linking Mode** or **Purch. Document Linking Mode**, according to the source side. Each setting offers **Open and Released** (default), **Open Only**, and **Released Only**. The purchase mode applies consistently to **Link to Purchase Line**, **Import Purchase Document**, **Charge Allocation**, and **Freight Order assignment**. Pending Approval and Pending Prepayment sources are excluded in every mode. See [Source document linking status](setup.md#source-document-linking-status).

The selection lists and the operation both check the configured mode; TMS does not release or reopen the source automatically. **Create** and **Add to Document** create new financial lines and retain their **Open** requirement, independently of the linking modes.

For both purchase actions, a filled **Customer/Vendor No.** on the current **Cost** row filters the selection list by **Buy-from Vendor No.**; a blank value on Cost shows all vendors. **Import Purchase Document** from an **Income** row ignores its customer number and shows all vendors, even if that number also exists as a vendor. This applies to Purchase Orders, Purchase Invoices, Purchase Credit Memos, Posted Purchase Invoices, and Posted Purchase Credit Memos. **Import Purchase Document** still creates new Cost rows; the current row is not replaced by the import.

Both **Import Purchase Document** and **Link to Purchase Line** require a **User Setup** record with **TMS Allow Purchase Document Allocation** enabled. This applies to unposted and posted purchase documents. Reopen the Forwarding Order after changing the option.

For an invoice that has already been posted, use **Posted Purchase Invoice Line** under **Link to Purchase Line**, or **Posted Purchase Invoice** under **Import Purchase Document**. The Cost receives the posted line's actual values and is immediately marked **Posted**. These actions do not post the purchase again. See [Link an already posted purchase invoice](purchaseassignment.md#link-an-already-posted-purchase-invoice) for eligibility and correction restrictions.

For a posted vendor credit, use **Posted Purchase Credit-Memo Line** to replace an existing **Cost / Credit** plan, or **Posted Purchase Credit-Memo** to create new **Cost / Credit** rows. A source amount of **100** contributes **+100**, reducing cost; a negative source amount keeps its negative sign. See [Link an already posted purchase credit memo](purchaseassignment.md#link-an-already-posted-purchase-credit-memo).

## Add an Income line

Use these steps for a manually agreed customer service. If a suitable Default Service Set line already exists, complete that draft instead of creating a duplicate.

1. Open the **Forwarding Order**. In **Ordering Party**, check **Order Party Type = Customer**, **Order Party No. / Order Party Name**, and **Currency Code** for the future sales document. Check the order's **Posting Date** too.
2. Expand **Settlement** and add a new row.
3. Set **Type = Income**.
4. In **Code**, select the Service being billed. TMS copies its description and available unit default. The Forwarding Order Type can restrict which services are available.
5. Review **Description** and **Customer/Vendor Name**. New Income lines take the customer from the current Forwarding Order Ordering Party. The same FWO can retain earlier linked or posted Income for other customers; see [Change the customer for the next invoice](#change-the-customer-for-the-next-invoice).
6. Choose **Financial Effect**: **Standard** for an ordinary charge to the customer; **Credit** for a customer credit.
7. Choose **Pricing Mode**, then enter the values in the scenario table below.
8. Fill or confirm **Unit of Measure Code**. It is required before creating or adding every Income line to a sales document, including fixed fees and services mapped to a G/L account or resource. Use a unit allowed by the mapped item when the service uses an Item.
9. Check **Currency Code** and **Exchange Rate Date**. Currency Code is the currency of this service price; blank means local currency. Review dimensions and **Location Code** if your process uses it. Location Code is hidden by default and can be shown through personalization.

For an editable **Income** draft, enter Location Code before creating the sales document; the generated sales line receives the value. A nonblank **Cost** Settlement Location Code is likewise passed to a newly created purchase line through normal Business Central validation. When it is blank, standard document defaults apply. After creation, Settlement stores the actual location of the purchase line. Applying an existing purchase document line also brings its location into Settlement. The Cost field remains read-only, and the column stays hidden by default. The field does not change Settlement calculations; document lines follow the normal Business Central location rules.
10. If needed, show **Line Discount %** and enter the agreed discount. Check **Amount**, **Line Amount**, **P/L Amount (LCY)**, and the order totals before creating a financial document.

### Choose values for your Income scenario

All examples below are in one currency and exclude VAT.

| What the customer agreed | Financial Effect | Pricing Mode | Values to enter | Expected Amount |
|---|---|---|---|---:|
| One transport service at 1,000 | Standard | Per Unit | Quantity 1; the service unit; Price 1,000. | +1,000 |
| 10 pallets at 25 each | Standard | Per Unit | Quantity 10; the pallet unit; Price 25. | +250 |
| A total fee of 1,000 regardless of quantity changes | Standard | Fixed Amount | Quantity 1; the service unit; Amount +1,000. | +1,000 |
| Credit the customer 100 | Credit | Per Unit | Quantity 1; the service unit; Price 100. | +100; P/L −100 |
| A fixed customer credit of 100 | Credit | Fixed Amount | Quantity 1; the service unit; Amount 100. | +100; P/L −100 |

For distance- or weight-based billing, use Per Unit and the correct unit and rate. On verified, unlinked Income lines, **Recalculate Settlement** updates the quantity from transportation data when **Calc. Qty. as Distance** or **Calc. Qty. as Weight** is configured. With Fixed Amount, it preserves the agreed total and adjusts the derived Price. Linked, posted, and Needs Review lines are not recalculated this way.

For automatic pricing, use **Get Prices** when enabled and review the generated lines. Price-list or agreement-controlled lines must be maintained through their source workflow. A line copied from a Default Service Set remains an editable draft until linked.

### Credit an existing Income or Cost line

1. Select the original line with **Financial Effect = Standard**.
2. Choose **Line** → **Create Credit Line**.
3. Review the new independent draft. It keeps the original Type, has **Financial Effect = Credit**, and reverses the financial contribution while retaining document signs.
4. Adjust the credit draft to the agreed value while it is editable. In Per Unit, adjust quantity or price. In Fixed Amount, adjust Amount before discount.
5. Create the appropriate **Sales Credit Memo** or **Purchase Credit Memo**.

Create Credit Line preserves the commercial quantity, price, and line discount for an ordinary line. When the original includes VAT in its prices or has an invoice discount, the independent draft uses the net amount excluding VAT as a Fixed Amount base and clears the document discount. Review that draft before creating its credit memo.

Full Reversal is not available. This action creates an independent draft; it does not post a correction or modify the original document. Review the credit amount and its assignment to each affected Forwarding Order, then create and post the appropriate credit memo through the normal document workflow.

### Correct a posted sales or purchase invoice

See [Invoice Corrections and Corrective Forwarding Orders](invoice-corrections.md) for step-by-step instructions, the three correction actions, active versus posted FWO behavior, and financial-result examples.

TMS follows these standard Business Central actions for invoice lines already assigned to Settlement. Business Central's document eligibility checks still apply.

| Action | Settlement behavior |
|---|---|
| **Cancel** | Creates the credit allocation and follows the full corrective credit memo created and posted by Business Central. |
| **Correct** | Follows both the cancellation credit and the new replacement invoice. The replacement has its own linked Standard rows and can be edited before posting. |
| **Create Corrective Credit Memo** | Links the copied lines of the editable credit memo. Change the supported source quantities or prices for a partial credit; the linked Credit rows follow those changes. |

Sales credits use **Income / Credit**; purchase credits use **Cost / Credit**. Replacement invoices use **Standard**. An ordinary sales invoice for 1,000 contributes P/L +1,000; its full credit contributes −1,000. The equivalent purchase contributes −1,000 and its credit +1,000. Original invoice rows retain their references and amounts.

When the original FWO is active, the new rows remain on that order. When the allocation belongs only to posted history, TMS asks once to create active **corrective Forwarding Orders**, using [Correction FWO Type Code](setup.md#corrective-forwarding-orders). Each original order gets one corrective FWO for the operation, with the same Ordering Party. **Correct** places its credit and replacement allocations on that same corrective FWO. The original posted FWO, its Settlement, and its trip history are not rewritten or reopened.

Identical historical copies of one allocation count once; a matching active allocation takes precedence. TMS checks source identity and the saved shares instead of treating each posted version as a new charge. Conflicting or unresolved history must be resolved before the operation can continue. A source line shared between FWOs keeps its original allocation factors: a partial credit of 200 for a 60/40 allocation contributes 120 and 80. Currency and quantity rounding remainders are assigned consistently so the shares equal the source. An unchanged purchase credit from **Cancel** or **Correct** preserves the exact original shares, including their rounding. If its financial values change while it is open, TMS allocates its current values using the original factors.

Make changes to managed unposted correction rows in the financial document. TMS updates their quantities, prices, discounts, currency context, and dimensions from that source. The copied source relationship is preserved through the supported line-recreation workflow. Newly added document lines without an original allocation still need their own Settlement assignment. Do not separately import a line that is already managed by the correction operation.

Any created credit memo that remains unposted can be edited, including one left by an unsuccessful **Cancel** or **Correct**. Post it when its changes are complete. TMS checks Settlement against the current credit-memo lines, not the original invoice's price or quantity. Posting moves the existing Settlement references to the posted financial lines. Posting such a credit manually does not resume **Correct**, create a replacement invoice, or perform the original action's cancellation bookkeeping. Replacement invoices can also be edited and posted through their normal document workflow.

Deleting a supported unposted corrective-credit line or replacement-invoice line removes its managed active Settlement rows, including when the credit originated from **Cancel** or **Correct**. Deleting the replacement does not undo a credit already posted by **Correct**. A corrective FWO containing any Settlement rows cannot be deleted; after those rows have been removed through the supported workflow, the empty order can be deleted while the document-correction operation is retained. The original-FWO reference is stored on the corrective order itself. Normal order deletion permissions and status rules still apply.

An FWO containing managed correction rows cannot be posted until those financial sources are posted and their copied links are complete, even if **Posted Documents Control** is disabled. A retained credit memo posted manually does not leave the FWO waiting for a replacement invoice that Business Central will no longer create. Posting the corrective FWO preserves its link to the original. Review **Has Corrections** on the original posted FWO, **Corrects Forwarding Order** on the corrective FWO, and the separate [profit including corrections](postedhistory.md#invoice-cancellation-in-posted-history) before closing the work.

Ordinary **Copy Document** and credit memos created independently of these invoice actions still use explicit Settlement linking or import. This workflow applies to new operations; it does not rebuild older corrections or rewrite their history.

**Correction Baseline Status = Linked After Posting** identifies a sales or purchase invoice or credit memo first linked to Settlement after posting. It records when the link was established; it does not establish the source's complete prior correction history.

Unknown prior correction history, including **Legacy Unknown** and **Linked After Posting**, is preserved in history and does not by itself block closing the Forwarding Order.

## Sales documents

### Create a customer invoice

1. Complete and review the Income lines, including their unit of measure.
2. Check that the lines to invoice have **Financial Effect = Standard**. In **Ordering Party**, select the customer and set **Currency Code** for this invoice; blank means local currency.
3. In Settlement, choose **Create** → **Sales Invoice**. Use **Sales Order** if your process requires an order first.
4. Review any exchange-rate warning. The new sales document uses the current FWO Ordering Party customer, Posting Date, and invoice currency.
5. Open the created document and check customer, currency, quantities, prices, line discounts, VAT, and totals.
6. Post the sales document when ready. Return to Settlement and open **Document No.** to review the linked document.

**Create** includes all compatible unlinked, verified Income lines for that document's Financial Effect on the current FWO. Selecting one row does not limit creation to that row, and differing service-price currencies do not split the lines into separate invoices. The lines are converted to the current FWO invoice currency. **Create** → **Sales Credit Memo** uses Credit lines.

You can create another **Sales Invoice** while earlier sales documents for the FWO remain open. Existing linked or posted Income lines keep their document references and are excluded from the new invoice. For example, create a EUR invoice for the current free Income, change the FWO invoice currency to USD, add the next Income lines, and create another invoice. Each invoice retains its own currency. **Create** → **Sales Order** retains its existing restriction when an unposted Sales Order or Sales Invoice is already linked.

To add only particular Settlement lines to an existing open document, select them and choose **Add to Document** → **Select Sales Document**. Standard lines can target an Order or Invoice; Credit lines can target only a Credit Memo. Mixed Standard/Credit selections are rejected.

### Change the customer for the next invoice

Change **Ordering Party No.** on the Forwarding Order to bill the next services to another customer. Earlier linked or posted Income can remain on the same order: their customers, source documents, and financial values stay with those documents.

The change assigns the new customer to **all unlinked Income** on the order, including manually entered lines, generated lines, and lines marked **Legacy** or **Needs Review**. It preserves their quantities, prices, discounts, service-price currencies and exchange rates, saved amounts, verification state, and generation or correction references. Cost lines keep their own vendors and values. Customer dimensions update on editable manual and Default Service Set drafts; other generated rows and rows marked **Legacy** or **Needs Review** retain their saved dimensions.

The header receives the new customer's default Forwarding Agreement, but existing Income is not replaced with that agreement's charges. Default agreement generation still applies when a customer is selected on an order without Income. Explicitly selecting or refreshing a Forwarding Agreement remains a separate operation with its normal restrictions on the generated lines it would replace.

For example, create an invoice for customer A, then change the Ordering Party to customer B. The first invoice and its linked Income remain with A. All unlinked Income now belongs to B, and the next **Sales Invoice** uses B. Review all unlinked lines before changing the customer, since the change applies to the whole unlinked Income set.

Changing the customer does not make **Legacy** or **Needs Review** lines ready for invoicing. Complete the normal verification, service mapping, unit of measure, currency, and financial checks before creating a document. Order status, customer blocking and credit controls still apply. A financial correction order must retain the Ordering Party of its saved original posted FWO.

### Import an existing sales document

Choose **Import to Settlement** → **Import Sales Document**, then select the document type:

| Action | Source | Result |
|---|---|---|
| **Sales Order** | An unposted sales order in a status allowed by Sales Document Linking Mode. | New linked Income lines with Standard effect. |
| **Sales Invoice** | An unposted sales invoice in a status allowed by Sales Document Linking Mode. | New linked Income lines with Standard effect. |
| **Sales Credit Memo** | An unposted sales credit memo in a status allowed by Sales Document Linking Mode. | New linked Income lines with Credit effect. |
| **Posted Sales Invoice** | An eligible posted sales invoice. | New posted Income lines with Standard effect. |
| **Posted Sales Credit-Memo** | An eligible posted sales credit memo. | New posted Income lines with Credit effect. |

All five actions show documents for all customers, regardless of the FWO's **Ordering Party Type** or **Ordering Party No.** They accept documents in any currency, independently of the FWO's current invoice currency. The three unposted actions filter source status using **Sales Document Linking Mode**; the posted actions keep their separate eligibility rules. The imported Income takes its customer from the source document; importing does not change the FWO's Ordering Party.

The document lookup opened by **Import Sales Document** includes a read-only **Comment** column. It shows existing Settlement assignments and source limitations, and identifies unposted or related posted credit memos for an invoice when a relationship is recorded. Credit-memo comments can show the referenced invoice. Review the listed document numbers and their allocations before importing.

Comments add no new selection restrictions; the existing import checks below still apply. Close and reopen the lookup to refresh its selection snapshot and comments after a document or its assignments change.

You can import into an FWO with **Ordering Party Type = Company**, an empty Ordering Party, or a different customer. This also applies to a corrective FWO that retains **Company** from its original. The source must have a **Sell-to Customer No.**, and its lines must belong to that same source customer. A different **Bill-to Customer** remains part of the financial document. TMS imports the document's lines into the current FWO; selecting an existing Settlement row does not make it the import target.

Import preserves the source document's actual currency, currency factor, and amounts in the new Settlement rows. It keeps the FWO's invoice currency unchanged. For example, a USD invoice can be imported when the FWO's **Currency Code** is blank (LCY). **Add Lines to Existing Forwarding Order** on a Sales Invoice uses the same import rule. **Add to Document** creates new sales lines and retains its existing currency-compatibility checks.

Each imported non-comment line is matched to **Services** through **Invoice Line - Type** and **Invoice Line - No.** A single match is used automatically. If several Services match, the one with **Default for Sales Import** enabled takes priority. If none is marked, TMS uses the first matching **Code** in ascending order, independently of list sorting, filters, or record creation order. This fallback does not mark a default or change setup. Missing mappings or multiple marked defaults stop import. The selected Service must be allowed by the FWO type's **Allowed Service/Charges Set**, when configured; TMS does not try another match if the selected one is disallowed. This rule applies to open and posted documents; see [Choose a service for sales import](services.md#choose-a-service-for-sales-import).

In Services setup, configuring the first Service for a complete type and number enables its import default automatically; additional Services for that pair remain unmarked. Existing mappings are not assigned defaults during upgrade, and a manually cleared flag stays cleared until you change the mapping or choose a default yourself.

TMS copies the source description, quantity, unit price, discounts, net amount, unit of measure, location, currency context, and dimensions, and retains the document and line reference. The actual source amount is preserved without rounding it again using current currency settings. Invoice amounts normally increase Income; credit memo amounts reduce it. Negative source quantities or amounts retain their economic meaning. Changing the import default affects future automatic selections and does not reclassify existing Settlement links or history.

Import always creates new Income rows. Existing planned Income remains. For example, importing an invoice for 1,000 while a planned Income of 1,000 remains gives total Income of 2,000; review existing plans before importing.

An integration can import an existing **Sales Order** through the
[`importSalesOrder` API action](api.md#importing-a-sales-order-into-settlement)
on the target Forwarding Order. It imports all non-comment lines using the same
status, mapping, and source checks as the interactive Sales Order action, and
creates the same document links. The API does not approve, release, reopen, or
post the source. Complete any required approval workflow before calling it.
Existing planned Income remains, and repeating an import after its source lines
have been linked returns an error.

For **unposted documents**, the current status must satisfy **Sales Document Linking Mode**, and all non-comment lines are checked before any Income is created. A missing Service mapping, multiple marked defaults, an unsupported source line, or any line already linked to active or archived Settlement blocks the whole import. Prices may include or exclude VAT. **Allow Invoice Disc.** may be **Yes** or **No**: it permits invoice discount calculation and does not mean a discount has been applied. Initial import still requires zero **Inv. Discount Amount** and zero **Inv. Disc. Amount to Invoice** on each source line. If either amount is nonzero, the error identifies the document and line and asks you to review the actual invoice discount. Import does not turn the flag off, remove a discount, or recalculate the source amounts. Ordinary line discounts are supported. These are initial-link requirements; subsequent changes to an already linked source line are reflected in Settlement.

For **posted documents**, comments and lines already linked to active or archived Settlement are skipped. All remaining lines are checked before importing; a failed check leaves no partial import. Posted invoices must not be marked Cancelled, Corrective, or Correction, and cannot be prepayment invoices. Posted credit memos must not be Cancelled or prepayment credit memos; an otherwise eligible Corrective or Correction credit memo is allowed. Prices may include or exclude VAT; actual invoice discount must still be zero, and unsupported processing such as deferrals is excluded.

VAT-inclusive import uses the existing source-value representation: prices and document amounts retain the source's VAT basis, while **Net Amount Excl. VAT** and **P/L Amount** exclude VAT. The source is not repriced or converted by the import. Later supported changes to an unposted source continue through the same [linked-document synchronization](#change-or-remove-a-document-link).

The posted source supplies the actual values and posting date. New rows are verified and **Posted**, with **Correction Baseline Status = Linked After Posting**. The import does not post the source again or create financial entries. Eligible posted Sales links can be detached through **Detach Settlement**. Posted Sales Invoice lines also provide **TMS > Detach Settlement** and **Detach All Lines**; the normal FWO closing checks still apply.

## Purchase documents

Enter an expected vendor charge as **Type = Cost**, select its **Code**, confirm **Customer/Vendor Name**, and choose Financial Effect and Pricing Mode using the sign rules above.

When the vendor invoice arrives, decide whether its lines already exist in Business Central:

| Situation | What to do |
|---|---|
| The unposted invoice has an existing line for your planned Cost. | [Link that purchase line to the existing Cost](purchaseassignment.md#link-one-purchase-line-to-an-existing-cost). No Release step is required. |
| An unposted invoice has several costs that are not yet recorded in this FWO. | [Import the whole purchase document](purchaseassignment.md#import-a-whole-purchase-document). No Release step is required. |
| The invoice has already been posted. | [Link a posted invoice line or import a whole posted invoice](purchaseassignment.md#link-an-already-posted-purchase-invoice). Choose the single-line action to replace a plan, or the whole-document action to create new Cost rows. |
| The vendor credit memo has already been posted. | [Link its line to an existing Cost row or import new Cost / Credit rows](purchaseassignment.md#link-an-already-posted-purchase-credit-memo). The source sets the Credit effect; positive source amounts reduce cost. |
| One vendor line covers several Forwarding Orders. | Release its unposted document, then [allocate the purchase line across orders](purchaseassignment.md#allocate-one-purchase-line-across-forwarding-orders). |
| Settlement contains costs and you want TMS to create a new purchase document. | Use **Create** → **Purchase Invoice**, **Purchase Order**, or **Purchase Credit Memo**. Creation uses compatible unlinked Cost lines for the selected vendor and Financial Effect. |
| An open purchase document needs new lines from Settlement. | Select the Cost lines and use **Add to Document** → **Select Purchase Document**. |

See [Purchase Assignment](purchaseassignment.md) for prerequisites, matching rules, Freight Order assignment, step-by-step linking, and removing an incorrect assignment.

Existing unposted purchase lines can be linked, imported, or allocated when their document status is allowed by **Purch. Document Linking Mode**. The same mode applies to direct FWO Settlement actions, **Charge Allocation**, and **Freight Order assignment**. Eligible **Posted Purchase Invoice** and **Posted Purchase Credit Memo** lines can also be linked afterward under their separate rules; payment or application alone does not prevent linking. Posted Purchase Invoice lines also offer **TMS > Charge Allocation** to split a free source line between several FWOs.

When linking one purchase line to an existing Cost, the actual source replaces the old plan, including its vendor, Financial Effect, quantity, price, discounts, amount, currency, dates, location, and dimensions. **Needs Review**, **Legacy**, or **Unresolved** metadata and old amounts do not prevent this link by themselves. A filled **Customer/Vendor No.** on the current Cost limits the selection list to the matching **Buy-from Vendor No.**; a blank value shows all vendors. Actual conflicting document, Freight, correction, and allocation references remain protected. A compatible existing Charge Code is retained, even when another matching Charge is the import default. Otherwise, TMS uses the matching Charge marked **Default for Purchase Import**, or the first matching **Code** in ascending order when none is marked. See [single-line purchase linking](purchaseassignment.md#link-one-purchase-line-to-an-existing-cost).

Whole purchase document import creates new Cost rows and uses that same automatic Charge selection rule for each free source line. It does not use the Code on the currently selected Settlement row. See [Choose a charge for purchase import](charges.md#choose-a-charge-for-purchase-import).

For [multi-FWO Charge Allocation](purchaseassignment.md#allocate-one-purchase-line-across-forwarding-orders), select a free Cost in each order whose vendor exactly matches **Buy-from Vendor** and whose existing, nonblank Charge Code maps to the source line's **Type / No.** **Pay-to Vendor** is not the match criterion. Blank or different vendors and blank or incompatible codes are excluded; mass allocation does not replace the Charge Code. Exactly one eligible row is selected automatically; several eligible rows require an explicit **Settlement Line No.** choice. The command rechecks those targets before applying positive allocation ratios that total 100%. For Amount 100 split three ways, the document shares are 33.33, 33.33, and 33.34. The last share receives each component's rounding remainder, including LCY and ACY, so totals match the source. Quantity, price, and percentage retain their source meaning.

The same **Select Allocation** dialog is available from **TMS > Charge Allocation** on a **Posted Purchase Invoice** line. A free posted source is linked to every chosen Cost with its own share and a Posted Invoice reference. The operation preserves the financial document and entries, supports the source's actual line/invoice discounts and VAT-inclusive input mode, and saves the complete group atomically. Existing active or archived Settlement/Freight links disable allocation and are checked again by the command.

## Currencies and dates

| Field | How to use it |
|---|---|
| **Currency Code** | Currency of the planned service or cost. Blank means LCY. |
| **Amount (LCY)** | Amount before line discount converted to LCY, retaining the document direction. |
| **Line Amount (LCY)** | Line Amount after line discount converted to LCY. |
| **P/L Amount (LCY)** | Signed net financial contribution used in FWO profitability. |
| **P/L Amount (ACY)** | P/L Amount (LCY) converted to the TMS additional reporting currency, including when the transaction currency equals ACY. Equals LCY when no reporting currency is configured. |
| **Exchange Rate Date** | Date used for the planned exchange rate. Linking adopts the financial document's exchange-rate context. |
| **Posting Date** on Settlement | The planned line's own date. Changing the FWO date does not replace it. Purchase linking adopts the source Posting Date, or its Document Date when an open source has no Posting Date. FWO posting preserves the stored date. |
| **Document Posting Date** | The Posting Date of the linked financial document. |
| **Currency Code** in the FWO **Ordering Party** section | Currency for the next sales document. It may differ from planned Income line currencies and earlier invoices on the same FWO. Blank means LCY. |

For example, a planned service of 100 USD can be invoiced in EUR. Creation converts the service using the sales document's Posting Date and Currency Factor. After linking, Settlement shows the actual EUR values and currency context from that sales line.

A new Sales document takes its Posting Date from the current FWO; Income line dates can differ. Purchase document creation requires a common Posting Date across the selected Cost lines. A combined Sales Invoice requires a common Posting Date across the selected FWOs.

A combined Sales Invoice also requires the same Ordering Party and invoice currency. The selected FWOs may have different Document Dates and dimension sets. The invoice header takes its dimensions and Document Date from the first processed FWO, or uses today's Document Date when **Invoicing Today** is enabled. Each invoice line keeps the dimensions of its corresponding Settlement line.

**Fulfillment Overview** shows Posting Date, Document Date, and both global dimensions as columns. Before **Create Sales Invoice** proceeds, it reports all selected-order differences in Ordering Party, invoice currency, and Posting Date together. For each field, the message shows the most common value and its order count, followed by every differing FWO number and value. If several values are equally common, the message identifies the reference value without treating it as the correct value. The same message lists orders whose status prevents invoicing, whose Ordering Party Type is not Customer, or whose Ordering Party No. or Posting Date is missing. An empty currency code is shown as local currency. Document Date and dimensions are not compared. Settlement-line and financial-document checks still run after this selection check passes.

You can change the FWO **Currency Code** in **Ordering Party** even when Income lines are already linked or posted, provided the FWO status allows the change. This sets the currency for the next sales document. Existing documents and their linked Settlement amounts, currencies, and exchange rates retain their current values; unlinked service prices also keep their own currencies. To add new lines to an earlier invoice, the current Ordering Party customer and FWO invoice currency must match that document before using **Add to Document**.

Changing **Order Party No.** reassigns all unlinked Income to the new customer, including agreement, price-list, Legacy, and Needs Review lines. Existing linked or posted Income retains its source customer and financial values. See [Change the customer for the next invoice](#change-the-customer-for-the-next-invoice).

When you select a different customer in **Ordering Party**, the new customer's default dimension values replace existing values for the same dimension codes on the FWO and on its editable manual or Default Service Set Income drafts. This includes manually entered values. New customer dimension codes are added; existing dimensions not supplied by the new customer are retained. Other default sources can still fill missing dimensions. For example, the customer's **Region = South** replaces **Region = North**, while an existing **Project = P17** remains if the new customer does not supply Project.

Customer defaults also replace an editable Income draft's differing value when the old and new customers have the same value on the FWO. Other generated rows and rows marked **Legacy** or **Needs Review** retain their saved dimensions when the customer changes. Linked and posted Income keeps the dimensions of its source document.

Filled quantities, prices, amounts, and review markers do not by themselves prevent changing **Ordering Party**. If an unlinked imported line remains blocked for normal editing or invoicing after amount conversion, an administrator can use [Repair Settlement Drafts](setup.md#repair-blocked-settlement-drafts). The wizard checks the line's ownership, independent status, and financial values before restoring normal draft editing. This includes eligible converted legacy lines still requiring review. It can also fill an empty Income **Customer/Vendor No.** from the FWO's nonblank customer Ordering Party; Preview shows the proposed customer number. A different existing customer remains a conflict for that repair workflow, and Cost lines do not inherit the FWO customer. Financial amounts are preserved.

The same wizard repairs **FWO System ID** and **FWO Type Code** on eligible unlinked Income and Cost lines using the existing active FWO identified by **Forwarding Order No.** It also corrects an old GUID pointing to another existing order, such as the copy source: the row's Forwarding Order No. determines its owner. Preview shows the proposed correction, so this case does not require a separate GUID Repair run. A missing FWO remains a diagnostic. The Settlement row's own identity, prices, amounts, and P/L are preserved; document-linked, posted, and protected source-controlled rows remain outside this draft repair.

The wizard repairs **Ready** rows while leaving skipped or failed rows for review. Each row is saved separately, so successful repairs remain saved if another row fails or the run is interrupted. Afterward, **Preview Details** shows each row's **Repair Status** and diagnostic. Refresh the Forwarding Order with **F5** before editing its repaired lines. A remaining blocked Income still needs resolution before it can be invoiced.

Business Central controls **Bill-to Customer** and **Pay-to Vendor** on document headers. Settlement identifies the operational **Sell-to Customer** or **Buy-from Vendor**.

## Change or remove a document link

Invoice and credit-memo lines in Sales and Purchase, including posted documents, show one **Allocated Amount** for their linked active and posted Settlement rows. It is the net amount excluding VAT in the document currency, with the document sign. Identical copies of the same FWO and Settlement line count once; an active copy takes precedence over its matching posted versions. Copies are combined only when their source identities, allocation shares, quantities, amounts, and currency context agree. Conflicting historical snapshots remain separate contributions so they can be reviewed; the displayed amount is not proof that such a group is eligible for automatic cancellation.

Choose **Allocated Amount** to open the active list, the posted list, or a combined **Linked Settlement Lines** list when both are present. The details retain every physical version for comparison, including copies counted only once in the total, and identify each FWO's status and exact posted version. Zero-value links remain available. See [Purchase Assignment](purchaseassignment.md) for navigation details.

A linked Settlement line takes its actual values from the document. Every saved change to the existing Sales or Purchase line refreshes the values represented in Settlement: description, quantity, unit, price or direct unit cost, line and invoice discounts, net and VAT-inclusive amounts, location, and dimensions. VAT posting groups and other document settings remain controlled by Business Central; their calculated results flow into Settlement. TMS also refreshes existing links before posting the financial document. Purchase allocations retain their existing shares and total. Posting the FWO later archives the saved Settlement values.

Changing dimensions on the FWO updates its defaults. If you confirm updating Settlement lines, the change applies only to editable, unlinked drafts, subject to the FWO status rules. Linked, posted, and protected generated rows keep their existing dimensions. To change a linked row's dimensions, use the source document: saved dimension changes on an unposted Sales or Purchase line continue to update its linked Settlement rows automatically.

After posting, the actual **posted document line** supplies the final description, quantities, amounts, unit, location, and dimensions. Both shortcut dimensions and the complete **Dimension Set ID** replace the previous Settlement values, including when dimensions were changed in the invoice before posting. Every Settlement share of an allocated purchase line receives those posted dimensions; its Forwarding Order reference and allocation share remain in place.

You can reopen a linked purchase document for editing when Business Central requires it. Its supported source refresh continues regardless of the current linking mode; changing the mode or the source status does not invalidate an existing link. A new link, import, **Charge Allocation**, or **Freight Order assignment** must satisfy **Purch. Document Linking Mode** at the time of that operation.

An ordinary deletion of a linked Sales or Purchase line requires detaching its Settlement link first. Standard header changes, such as changing currency, can temporarily delete and recreate document lines. TMS treats that Recreate operation separately: it restores the link to the recreated line and refreshes the document values, discounts, and P/L amounts.

Changes to the same source line do not require detaching first. This includes VAT, prices including VAT, invoice discounts, quantity, unit, description, dimensions, and the line's **Type / No.** The existing physical document line remains authoritative even when its new values differ from the Service or Charge defaults used when linking it. A currency change on an open document updates the linked currency and exchange rates automatically; review the result in Settlement. Changing allocation membership or deleting and replacing the physical source uses the corresponding link workflow. Initial-link eligibility and the posting lifecycle restrictions listed below still apply.

| Action | Result |
|---|---|
| **Detach Settlement** under **TMS** on an open Sales or Purchase line | Removes the selected source line's complete direct Income or Cost group, including supported archived references. Keeps the document line and Settlement records. |
| **Detach Settlement** under **TMS** in Posted Sales Invoice, Posted Purchase Invoice, or Posted Purchase Credit Memo lines | Removes the selected posted source line's complete direct Settlement group. Keeps the posted document and all financial entries. |
| **Detach All Lines** under **TMS** in Sales Invoice, Purchase Invoice, Posted Sales Invoice, Posted Purchase Invoice, or Posted Purchase Credit Memo lines | After one confirmation, removes every direct Settlement group for that document, including lines outside the current selection or page filters. Unlinked source lines are ignored. |
| **Import to Settlement** → **Detach Settlement** in Settlement | Uses the same source-group command as the document page. Supports the selected direct Income or Cost link, including an eligible posted invoice or credit memo. |

All entry points use one detach mechanism. The selected Settlement row identifies the source line; it does not limit the operation to that row or its FWO. Every physical reference to the affected source is included, across active Settlement and all posted FWO versions. An invoice with only archived references can be detached from its invoice-line actions.

Detach does not require an unposted Sales or Purchase document to have **Open** status. An eligible **Released** document can remain Released. The operation removes Settlement links without editing its source lines; the existing processing, correction, source-identity, and FWO checks still apply.

For example, a purchase line for **100** is distributed equally to **five FWOs**. Three FWOs have been posted and two remain active. **Detach Settlement** removes all five assignments, including every saved version of the three posted orders. The two active rows become unlinked **Fixed Amount** plans with their existing **P/L Amount = −20** each. The three posted orders retain their historical amounts and P/L; only their source-reference and allocation-link fields are cleared.

After a successful direct Detach, each affected active row is an independent manual draft ready for normal editing without running Repair. The row no longer belongs to its former agreement, price list, or other generator. An Income draft receives the current FWO Ordering Party customer, including when its former document used a different customer. Cost retains its vendor. Normal FWO status restrictions still apply.

Active rows retain their P/L in the transaction, local, and additional reporting currencies. Ordinary document amounts and line discounts are retained. Where the linked values need VAT or invoice-discount context, the independent plan uses the saved net amount excluding VAT as its Amount base and clears the document discount. Detach uses the saved values without applying current exchange rates or restoring an earlier estimate. The source, correction, and Freight allocation checks below still determine whether direct Detach is allowed.

Archived rows stay read-only historical records. Their quantities, prices, discounts, amounts, currency values, and P/L remain unchanged. Historical versions can contain different values: detach removes their references without synchronizing those snapshots to today's document. See [Posted History](postedhistory.md#detach-an-archived-settlement-reference).

**Detach All Lines** is available when the document has active or archived Settlement references, even if the current line is unlinked. **Detach Settlement** requires a link on the current line; a zero allocated amount does not hide an existing link. Posted Purchase Credit Memo lines provide these two detach actions without Charge Allocation. All affected records are checked before the first change. A changed source, invalid link, or blocked member causes the entire operation to roll back; no successfully detached prefix is left behind.

Direct detach requires verified rows in the current amount model and valid source identities. The status of an active FWO must allow the change. Freight Order parent allocations, unresolved legacy links, TMS correction chains, cancelled sources, and unsupported source processing remain excluded. Supported archived source types are **Order**, **Invoice**, **Credit Memo**, **Posted Invoice**, and **Posted Credit Memo**; archived Quote, Blanket Order, and Return Order references are not supported by this command. An enabled action indicates that references exist, not that every validation will pass.

Detach does not delete or reverse the financial document. Posted document amounts, ledger entries, and payment applications stay unchanged. For an unposted document, delete its line separately only if it is no longer needed; retaining it and creating another invoice can duplicate billing. Review the retained active plans before linking a replacement. See [Remove or change an assignment](purchaseassignment.md#remove-or-change-an-assignment).

## Posting and supported scope

Review Settlement before posting the Forwarding Order. If **Posted Documents Control** or **FWO Posting Date Control** is enabled in TMS Setup, financial documents and dates must satisfy those controls first.

The supported document workflow covers Sales Orders, Invoices, and Credit Memos; Purchase Orders, Invoices, and Credit Memos; and full posting where each open source line produces one posted line.

Direct assignment also supports eligible **Posted Purchase Invoice** lines. The invoice must not be cancelled, corrective, or a prepayment invoice; prices may include or exclude VAT, and actual invoice discount amounts must be zero. Deferrals and other unsupported source processing remain excluded. TMS checks existing allocations in active and posted Settlement and Freight records, so closing a transportation order does not free its invoice line for another assignment. See the [posted purchase prerequisites](purchaseassignment.md#link-an-already-posted-purchase-invoice).

Eligible **Posted Purchase Credit Memo** lines can be linked to existing **Cost** rows or imported as new rows, subject to their [source and mapping checks](purchaseassignment.md#link-an-already-posted-purchase-credit-memo). The source sets the Credit effect. The credit memo itself must not be Cancelled; Corrective or Correction credits are allowed when otherwise eligible. Prices may include or exclude VAT; zero actual invoice discount and no prepayment or deferral are still required. The result is verified and **Posted**, without new financial entries. Eligible direct credit-memo links use the same **Detach Settlement** command as other supported sources.

For a linked Sales Order, post the full quantity with **Ship and Invoice**. For a linked Purchase Order, use **Receive and Invoice** for the full quantity. Posting only the shipment or receipt first is not supported.

Use compatible customer/vendor and currency settings. Creating a document normally works within one FWO; the dedicated combined Sales Invoice workflow is the exception and allows different FWO dimension sets. Purchase document creation requires compatible dimension sets. Adding to an existing document has its own compatibility checks.

The following are outside the supported Settlement workflow:

- partial shipment, receipt, or invoicing, separate shipment/receipt posting, and combining sources into posted lines;
- prepayments, deferrals, warehouse handling, reservations, and item tracking;
- Purchase Return Orders for physical return and item application;
- linking already posted Sales lines to an existing planned Income row; use **Import Sales Document** to create new posted Income rows instead;
- creating new split allocations from an already posted purchase credit memo; posted purchase invoices support **TMS > Charge Allocation**;
- detaching Freight Order parent allocations, TMS correction chains, or archived Quote, Blanket Order, and Return Order references;
- editing the original posted Settlement financial snapshot; use a separate [financial correction FWO](invoice-corrections.md#create-a-corrective-fwo-manually) for new adjustments;
- split/merge correction chains.

After amount-model conversion, **Needs Review**, **Unresolved**, and other legacy metadata retain their existing archive behavior; posting does not repair them. **Settlement Upgrade Required** is a separate conversion status: complete the amount-model wizard before processing these legacy amount rows. An independent Cost without a conflicting source identity can be linked directly to its actual purchase line even when its old metadata needs review. Use the controlled replacement below when a blocked manual Cost needs to remain an editable plan.

### Archive Settlement when posting the Forwarding Order

Choose **Post** on a Forwarding Order or post selected orders from the list to close them and create their transport history. TMS copies each saved **Income** and **Cost** row to the corresponding Posted Settlement row. Linking, changing, and posting sales or purchase documents prepares the Settlement values before this step.

**FWO posting preserves the stored values.** For ordinary Settlement rows, it does not compare quantities, prices, amounts, discounts or currency context with the source sales or purchase line, and it does not recalculate them. Customer/vendor, dates, dimensions, document references, allocation shares, and migration and correction history are copied as saved. Posting does not repair technical states or redistribute costs. Existing Settlement rows on other FWOs are not refreshed.

For an existing financial source, posting resolves the document and line by their saved type, number and line number and refreshes the technical record identifiers. A missing referenced document or line still blocks posting unless **Posted Source Deleted** records its deletion; recorded deleted-source history retains its saved identifiers. Rows without a document number or line number keep their saved identifiers. This reference lookup does not load or replace the financial snapshot. Freight Order purchase and allocation checks remain part of their separate workflow.

**Document Correction** rows retain their separate correction-operation checks, including source consistency, original shares and correction totals. The operation must be ready and its correction documents posted before these rows can be archived.

The FWO still needs its Posting Date and must satisfy its status rules and enabled posting controls. **Posted Documents Control** requires each Settlement row's saved **Posted** marker to be true. **FWO Posting Date Control** compares the saved **Document Posting Date** with the FWO Posting Date. The configured allowed posting range also continues to apply. **Link State**, **Data Verification Status**, **Source Quantity Mode**, and an unknown correction baseline do not add separate FWO posting restrictions. Rows still marked **Settlement Upgrade Required** must first be converted to the new amount model.

Only the chosen orders are archived. Their active Settlement rows are removed after the copies and totals have been checked. A posting error rolls back the order or selected batch. Sales and purchase documents and their financial entries are not posted again.

Review or correct financial values through the relevant document workflow before closing the FWO. Amount-model migration checks eligible legacy rows before conversion, but ordinary FWO posting does not repeat that source comparison. A source change missed by synchronization will therefore remain in the archived Settlement values. Archiving does not establish missing correction history.

### Recreate blocked manual Cost lines

On the Forwarding Order, choose **Settlement** → **Line** → **Recreate Blocked Cost Lines** to replace all eligible blocked manual Cost lines in that order. The action is disabled when the order has no eligible lines. Selecting a particular row does not limit the operation. Review quantities, prices, and Financial Effect before confirming.

The action excludes editable drafts, Income, posted rows, current or previous document links, Freight Order and allocation references, system-created rows, price-list or generated rows, and correction history. It does not detach documents or change their lines.

Each replacement is a new editable **Per Unit** draft. It preserves the Charge Code, Description, Unit of Measure Code, vendor number, Currency Code, existing Posting Date and Exchange Rate Date, dimensions, and distance/weight quantity settings. The vendor name is refreshed from the vendor card. Missing dates and an empty dimension set use the normal new-draft defaults from the FWO.

For an unverified migrated row whose quantity mode is still **Legacy**, the replacement converts the old signed quantity and price to nonnegative values. **Financial Effect** is recovered from the saved Amount rounded to the currency's current amount precision: negative means **Standard**, positive means **Credit**, and zero keeps the existing effect. Price is adjusted when needed so that the normal per-unit calculation preserves that rounded financial contribution. For example, an old Cost with quantity 1, price -100, and legacy amount -100 becomes Standard with quantity 1, price 100, Amount 100, and P/L Amount −100. The old cost sign is not applied twice.

Other rows retain their Financial Effect and signed Quantity and Price, and Amount is recalculated normally; intentional negative inputs are preserved. The currency factor is obtained again for the exchange-rate date, and LCY and reporting amounts are recalculated. Document references and migration or verification flags are not copied to the new draft.

After all replacements are created successfully, the original blocked rows are deleted in the same transaction. If any validation fails, the entire replacement is rolled back. A zero-quantity row with a nonzero old Amount is rejected because a per-unit calculation would erase its value. Complete that case separately before retrying.

## Troubleshooting

| Problem | What to check |
|---|---|
| An ordinary Cost increases profit | In Per Unit, check for a negative Quantity or Price and confirm Standard. In Fixed Amount, enter a positive Amount for an ordinary positive purchase line; its P/L is negative. |
| A credit has the wrong sign | Confirm Type and Credit. Changing Financial Effect reverses P/L while preserving document amounts; enter the same quantity, price, and Amount as the credit memo. |
| Amount is read-only | The draft is Per Unit, so edit Quantity and Price. Linked, posted, and source-controlled rows have additional protection. |
| Price is read-only | The draft is Fixed Amount, so edit Amount. |
| A fixed total changed after invoice linking | The document replaces the plan with its actual Amount before discount, Line Amount, discounts, and currency. The document quantity is shown as Per Unit after linking. |
| Income cannot be invoiced | Complete Unit of Measure Code on every affected Income row; check service invoice mapping, customer, status, VAT/posting setup, and currency. |
| Sales import reports an invoice discount | Review **Inv. Discount Amount** and **Inv. Disc. Amount to Invoice** on the document and line named in the message. Initial import requires both amounts to be zero. **Allow Invoice Disc. = Yes** alone does not block import, and TMS does not change the flag or remove a discount for you. |
| The next invoice must use a different customer | Change the FWO Ordering Party. All unlinked Income receives the new customer; linked or posted Income keeps its source customer and documents. |
| Ordering Party cannot be changed | Check order status, customer blocking and credit controls, and whether this is a financial correction tied to an original posted FWO. Legacy or Needs Review markers and existing linked or posted Income alone do not block a customer change. |
| An independent Income draft has no customer although the FWO does | Use [Repair Settlement Drafts](setup.md#repair-blocked-settlement-drafts) to preview filling the missing Customer/Vendor No. from the FWO Ordering Party. Existing different customers remain conflicts; Cost lines are not assigned this customer. |
| An imported unlinked Income or Cost has the wrong FWO System ID or FWO Type Code | Use the same [Repair Settlement Drafts](setup.md#repair-blocked-settlement-drafts) wizard to restore the owner fields from the active FWO number, including a GUID left from another existing order. Missing parents remain diagnostics. |
| More lines were invoiced than the selected row | Create uses all compatible unlinked lines for that effect. Add to Document uses the current or selected lines. |
| The same vendor cost appears twice | A whole-document import creates new rows. If a planned Cost existed, use a single-line link instead and review the duplicate before posting. |
| A purchase invoice cannot be found or linked | Follow [Purchase Assignment troubleshooting](purchaseassignment.md#troubleshooting); choose the unposted or posted invoice action. For an unposted source, check **Purch. Document Linking Mode** and the document status. Check source eligibility, Charge mapping, and existing document or allocation references. Old vendor, effect, and Needs Review markers alone do not block a single-line link. |
| A linked document line cannot be deleted | Use Detach Settlement first; it includes supported active and archived direct references. Freight allocations and correction chains require their own workflow. |
| Forwarding Order posting is blocked | Check the FWO status and Posting Date, then the enabled Posted Documents, document-date, and allowed-posting-range controls. Complete the amount-model wizard for rows marked Settlement Upgrade Required. Other technical Settlement states retain their existing archive rules. |
| A linked Income or Cost shows Unresolved or stale values | Review it through the relevant financial document workflow before closing the FWO. FWO posting copies the saved row without repairing it. |
| Settlement amounts differ from the financial document | Check the linking or financial posting workflow. FWO posting preserves the stored amounts and does not recalculate them. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Invoice Corrections and Corrective Forwarding Orders](invoice-corrections.md)
- [Create and Invoice Your First Forwarding Job](first-forwarding-job.md)
- [Services](services.md)
- [Charges](charges.md)
- [Freight Agreements](freightagreement.md)
- [Purchase Assignment](purchaseassignment.md)
- [Pricing](pricing.md)
- [TMS Setup](setup.md)
