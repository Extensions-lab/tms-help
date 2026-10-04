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

| Example line | P/L Amount (LCY) |
|---|---:|
| Customer income | +1,000 |
| Carrier cost | −700 |
| **Total Profit (LCY)** | **300** |

Total Cost (LCY) is already signed. Profit is **Total Income (LCY) + Total Cost (LCY)**; do not subtract a negative cost a second time. Amount (LCY) is the gross document value and is not a profitability measure.

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
| Settlement has the lines; no financial document exists. | **Create** → the required Sales or Purchase document. | Creates a new document and links its new lines. |
| Settlement has the lines; an open financial document exists but needs new lines. | **Add to Document** → **Select Sales Document** or **Select Purchase Document**. | Adds new financial lines to that document and links them. |
| A purchase invoice already contains the cost line; Settlement has its planned Cost. | **Import to Settlement** → **Link to Purchase Line**. | Connects the existing purchase line to the current Cost line. |
| A financial document contains lines that are not yet in Settlement. | **Import to Settlement** → the appropriate import action. | Creates Settlement rows from eligible existing document lines. |

After **Create** successfully creates a Sales or Purchase Order, Invoice, or Credit Memo, TMS asks: **Do you want to open the created document?** Choose **Yes** to open its card or **No** to stay on the Forwarding Order; the document and its Settlement links are saved either way. The Forwarding Order header actions **Create Sales Invoice** and **Create Credit Memo** offer the same choice. Creating a combined sales invoice from selected Forwarding Orders asks once, after all selected orders have been processed.

**Add to Document** does not match an existing invoice line. Use the single purchase line link when both sides already exist.

The FWO Settlement actions **Link to Purchase Line** and **Import Purchase Document** do not restrict the status of an unposted purchase Order, Invoice, or Credit Memo: **Open** and **Released** documents are both included. Source-line eligibility checks still apply. **Charge Allocation** across Forwarding Orders and **Freight Order assignment** still require an unposted source to have **Status = Released**. **Create** and **Add to Document** create new purchase lines in an **Open** document.

Both **Import Purchase Document** and **Link to Purchase Line** require a **User Setup** record with **TMS Allow Purchase Document Allocation** enabled. This applies to unposted and posted purchase documents. Reopen the Forwarding Order after changing the option.

For an invoice that has already been posted, use **Posted Purchase Invoice Line** under **Link to Purchase Line**, or **Posted Purchase Invoice** under **Import Purchase Document**. The Cost receives the posted line's actual values and is immediately marked **Posted**. These actions do not post the purchase again. See [Link an already posted purchase invoice](purchaseassignment.md#link-an-already-posted-purchase-invoice) for eligibility and correction restrictions.

For a posted vendor credit, use **Posted Purchase Credit-Memo Line** to replace an existing **Cost / Credit** plan, or **Posted Purchase Credit-Memo** to create new **Cost / Credit** rows. A source amount of **100** contributes **+100**, reducing cost; a negative source amount keeps its negative sign. See [Link an already posted purchase credit memo](purchaseassignment.md#link-an-already-posted-purchase-credit-memo).

## Add an Income line

Use these steps for a manually agreed customer service. If a suitable Default Service Set line already exists, complete that draft instead of creating a duplicate.

1. Open the **Forwarding Order**. In **Ordering Party**, check **Order Party Type = Customer**, **Order Party No. / Order Party Name**, and **Currency Code** for the future sales document. Check the order's **Posting Date** too.
2. Expand **Settlement** and add a new row.
3. Set **Type = Income**.
4. In **Code**, select the Service being billed. TMS copies its description and available unit default. The Forwarding Order Type can restrict which services are available.
5. Review **Description** and **Customer/Vendor Name**. The customer comes from the Forwarding Order Ordering Party. Income lines on one FWO cannot be assigned to different customers.
6. Choose **Financial Effect**: **Standard** for an ordinary charge to the customer; **Credit** for a customer credit.
7. Choose **Pricing Mode**, then enter the values in the scenario table below.
8. Fill or confirm **Unit of Measure Code**. It is required before creating or adding every Income line to a sales document, including fixed fees and services mapped to a G/L account or resource. Use a unit allowed by the mapped item when the service uses an Item.
9. Check **Currency Code** and **Exchange Rate Date**. Currency Code is the currency of this service price; blank means local currency. Review dimensions, and **Location Code** if your process uses it. Location is hidden by default and can be shown through personalization.
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

Using Business Central **Correct**, **Cancel**, or **Copy Document** to create a credit memo does not automatically create its Settlement allocation. Link or import the credit memo to the affected Forwarding Orders so that their profit includes the credit; until then, the original income or cost remains unoffset in Settlement.

**Correction Baseline Status = Linked After Posting** identifies a sales or purchase invoice or credit memo first linked to Settlement after posting. It records when the link was established; it does not establish the source's complete prior correction history.

Unknown prior correction history, including **Legacy Unknown** and **Linked After Posting**, is preserved in history and does not by itself block closing the Forwarding Order.

## Sales documents

### Create a customer invoice

1. Complete and review the Income lines, including their unit of measure.
2. Check that the lines to invoice have **Financial Effect = Standard**.
3. In Settlement, choose **Create** → **Sales Invoice**. Use **Sales Order** if your process requires an order first.
4. Review any exchange-rate warning. The new sales document uses the current FWO Posting Date and invoice currency.
5. Open the created document and check customer, currency, quantities, prices, line discounts, VAT, and totals.
6. Post the sales document when ready. Return to Settlement and open **Document No.** to review the linked document.

**Create** includes all compatible unlinked, verified Income lines for that document's Financial Effect on the current FWO. Selecting one row does not limit creation to that row. **Create** → **Sales Credit Memo** uses Credit lines.

To add only particular Settlement lines to an existing open document, select them and choose **Add to Document** → **Select Sales Document**. Standard lines can target an Order or Invoice; Credit lines can target only a Credit Memo. Mixed Standard/Credit selections are rejected.

### Import an existing sales document

Choose **Import to Settlement** → **Import Sales Document**, then select the document type:

| Action | Source | Result |
|---|---|---|
| **Sales Order** | An open sales order. | New linked Income lines with Standard effect. |
| **Sales Invoice** | An open sales invoice. | New linked Income lines with Standard effect. |
| **Sales Credit Memo** | An open sales credit memo. | New linked Income lines with Credit effect. |
| **Posted Sales Invoice** | An eligible posted sales invoice. | New posted Income lines with Standard effect. |
| **Posted Sales Credit-Memo** | An eligible posted sales credit memo. | New posted Income lines with Credit effect. |

When **Ordering Party No.** is filled, all five actions limit the selection to documents whose **Sell-to Customer No.** matches it. When the number is blank, the lists show documents for all customers. The existing invoice currency and document eligibility filters still apply. Browsing or selecting a document never fills **Ordering Party No.** automatically.

Before importing, manually set **Ordering Party Type = Customer** and **Ordering Party No.** on the FWO. Confirming a selection while they are missing stops the import without creating Settlement lines. The customer and currency are checked again when importing; a different **Bill-to Customer** does not change the operational customer match. TMS imports the document's lines into the current FWO; selecting an existing Settlement row does not make it the import target.

Each imported non-comment line must match exactly one **Service** through **Invoice Line - Type** and **Invoice Line - No.** The Service must be allowed by the FWO type's **Allowed Service/Charges Set**, when configured. TMS copies the source description, quantity, unit price, discounts, net amount, unit of measure, location, currency context, and dimensions, and retains the document and line reference. The actual source amount is preserved without rounding it again using current currency settings. Invoice amounts normally increase Income; credit memo amounts reduce it. Negative source quantities or amounts retain their economic meaning.

Import always creates new Income rows. Existing planned Income remains. For example, importing an invoice for 1,000 while a planned Income of 1,000 remains gives total Income of 2,000; review existing plans before importing.

For **open documents**, all non-comment lines are checked before any Income is created. A missing or ambiguous Service mapping, an unsupported source line, or any line already linked to active or archived Settlement blocks the whole import. Initial import requires **Allow Invoice Disc. = No**, zero **Inv. Discount Amount**, and zero **Inv. Disc. Amount to Invoice** on each source line. Review and adjust the document before importing if needed; importing does not change these source fields. Ordinary line discounts are supported. These are initial-link requirements; subsequent changes to an already linked source line are reflected in Settlement.

For **posted documents**, comments and lines already linked to active or archived Settlement are skipped. All remaining lines are checked before importing; a failed check leaves no partial import. Posted invoices must not be marked Cancelled, Corrective, or Correction, and cannot be prepayment invoices. Posted credit memos must not be Cancelled or prepayment credit memos; an otherwise eligible Corrective or Correction credit memo is allowed. Prices must exclude VAT, actual invoice discount must be zero, and unsupported processing such as deferrals is excluded.

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

Existing unposted purchase lines can be linked or imported from FWO Settlement without a document-status restriction. **Charge Allocation** and **Freight Order assignment** still require a **Released** unposted source. Eligible **Posted Purchase Invoice** and **Posted Purchase Credit Memo** lines can also be linked afterward; payment or application alone does not prevent linking. Posted Purchase Invoice lines also offer **TMS > Charge Allocation** to split a free source line between several FWOs.

When linking one purchase line to an existing Cost, the actual source replaces the old plan, including its vendor, Financial Effect, quantity, price, discounts, amount, currency, dates, location, and dimensions. **Needs Review**, **Legacy**, or **Unresolved** metadata and old amounts do not prevent this link by themselves. The selection lists show all vendors. Actual conflicting document, Freight, correction, and allocation references remain protected. A compatible existing Charge Code is retained; otherwise, the source must identify exactly one Charge. See [single-line purchase linking](purchaseassignment.md#link-one-purchase-line-to-an-existing-cost).

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
| **Currency Code** in the FWO **Ordering Party** section | Invoice currency used when creating a sales document. It may differ from planned Income line currencies. |

For example, a planned service of 100 USD can be invoiced in EUR. Creation converts the service using the sales document's Posting Date and Currency Factor. After linking, Settlement shows the actual EUR values and currency context from that sales line.

A new Sales document takes its Posting Date from the current FWO; Income line dates can differ. Purchase document creation requires a common Posting Date across the selected Cost lines. A combined Sales Invoice requires a common Posting Date across the selected FWOs.

Before Income lines are linked, eligible drafts can use a different FWO invoice currency without rewriting their service prices. Once any Income is linked or posted, the FWO **Order Party Type**, customer, and **Currency Code** in **Ordering Party** are protected. Changing the customer is also blocked by customer-specific agreement/price-list lines or data requiring review.

Business Central controls **Bill-to Customer** and **Pay-to Vendor** on document headers. Settlement identifies the operational **Sell-to Customer** or **Buy-from Vendor**.

## Change or remove a document link

Invoice and credit-memo lines in Sales and Purchase, including posted documents, show one **Allocated Amount** for their linked active and posted Settlement rows. It is the net amount excluding VAT in the document currency, with the document sign. Choose it to open the active list, the posted list, or a combined **Linked Settlement Lines** list when both are present. The combined list identifies each FWO's status and exact posted version. Its rows use the same filters as the displayed amount; zero-value links remain available. See [Purchase Assignment](purchaseassignment.md) for navigation details.

A linked Settlement line takes its actual values from the document. Every saved change to the existing Sales or Purchase line refreshes the values represented in Settlement: description, quantity, unit, price or direct unit cost, line and invoice discounts, net and VAT-inclusive amounts, location, and dimensions. VAT posting groups and other document settings remain controlled by Business Central; their calculated results flow into Settlement. TMS also refreshes existing links before posting the financial document. Purchase allocations retain their existing shares and total. Posting the FWO later archives the saved Settlement values.

After posting, the actual **posted document line** supplies the final description, quantities, amounts, unit, location, and dimensions. Both shortcut dimensions and the complete **Dimension Set ID** replace the previous Settlement values, including when dimensions were changed in the invoice before posting. Every Settlement share of an allocated purchase line receives those posted dimensions; its Forwarding Order reference and allocation share remain in place.

You can reopen a linked purchase document for editing. Direct **Link to Purchase Line** and **Import Purchase Document** actions do not require it to be released again. Release is still required for a new **Charge Allocation** or **Freight Order assignment**; an existing link does not require the document to remain Released throughout editing.

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

Active rows retain their P/L in the transaction, local, and additional reporting currencies. Ordinary document amounts and line discounts are retained. Where the linked values need VAT or invoice-discount context, the independent plan uses the saved net amount excluding VAT as its Amount base and clears the document discount. Detach uses the saved values without applying current exchange rates or restoring an earlier estimate. Existing restrictions for generated or agreement-controlled rows remain.

Archived rows stay read-only historical records. Their quantities, prices, discounts, amounts, currency values, and P/L remain unchanged. Historical versions can contain different values: detach removes their references without synchronizing those snapshots to today's document. See [Posted History](postedhistory.md#detach-an-archived-settlement-reference).

**Detach All Lines** is available when the document has active or archived Settlement references, even if the current line is unlinked. **Detach Settlement** requires a link on the current line; a zero allocated amount does not hide an existing link. Posted Purchase Credit Memo lines provide these two detach actions without Charge Allocation. All affected records are checked before the first change. A changed source, invalid link, or blocked member causes the entire operation to roll back; no successfully detached prefix is left behind.

Direct detach requires verified rows in the current amount model and valid source identities. The status of an active FWO must allow the change. Freight Order parent allocations, unresolved legacy links, TMS correction chains, cancelled sources, and unsupported source processing remain excluded. Supported archived source types are **Order**, **Invoice**, **Credit Memo**, **Posted Invoice**, and **Posted Credit Memo**; archived Quote, Blanket Order, and Return Order references are not supported by this command. An enabled action indicates that references exist, not that every validation will pass.

Detach does not delete or reverse the financial document. Posted document amounts, ledger entries, and payment applications stay unchanged. For an unposted document, delete its line separately only if it is no longer needed; retaining it and creating another invoice can duplicate billing. Review the retained active plans before linking a replacement. See [Remove or change an assignment](purchaseassignment.md#remove-or-change-an-assignment).

## Posting and supported scope

Review Settlement before posting the Forwarding Order. If **Posted Documents Control** or **FWO Posting Date Control** is enabled in TMS Setup, financial documents and dates must satisfy those controls first.

The supported document workflow covers Sales Orders, Invoices, and Credit Memos; Purchase Orders, Invoices, and Credit Memos; and full posting where each open source line produces one posted line.

Direct assignment also supports eligible **Posted Purchase Invoice** lines. The invoice must not be cancelled, corrective, or a prepayment invoice; prices must exclude VAT and actual invoice discount amounts must be zero. Deferrals and other unsupported source processing remain excluded. TMS checks existing allocations in active and posted Settlement and Freight records, so closing a transportation order does not free its invoice line for another assignment. See the [posted purchase prerequisites](purchaseassignment.md#link-an-already-posted-purchase-invoice).

Eligible **Posted Purchase Credit Memo** lines can be linked to existing **Cost** rows or imported as new rows, subject to their [source and mapping checks](purchaseassignment.md#link-an-already-posted-purchase-credit-memo). The source sets the Credit effect. The credit memo itself must not be Cancelled; Corrective or Correction credits are allowed when otherwise eligible. They also use prices excluding VAT, zero actual invoice discount, and no prepayment or deferral. The result is verified and **Posted**, without new financial entries. Eligible direct credit-memo links use the same **Detach Settlement** command as other supported sources.

For a linked Sales Order, post the full quantity with **Ship and Invoice**. For a linked Purchase Order, use **Receive and Invoice** for the full quantity. Posting only the shipment or receipt first is not supported.

Use compatible customer/vendor, currency, and dimension settings. Creating a document normally works within one FWO; the dedicated combined Sales Invoice workflow is the exception. Adding to an existing document has its own compatibility checks.

The following are outside the supported Settlement workflow:

- partial shipment, receipt, or invoicing, separate shipment/receipt posting, and combining sources into posted lines;
- prepayments, deferrals, warehouse handling, reservations, and item tracking;
- Purchase Return Orders for physical return and item application;
- linking already posted Sales lines to an existing planned Income row; use **Import Sales Document** to create new posted Income rows instead;
- creating new split allocations from an already posted purchase credit memo; posted purchase invoices support **TMS > Charge Allocation**;
- detaching Freight Order parent allocations, TMS correction chains, or archived Quote, Blanket Order, and Return Order references;
- corrections after the FWO is archived, or split/merge correction chains.

After amount-model conversion, **Needs Review**, **Unresolved**, and other legacy metadata retain their existing archive behavior; posting does not repair them. **Settlement Upgrade Required** is a separate conversion status: complete the amount-model wizard before processing these legacy amount rows. An independent Cost without a conflicting source identity can be linked directly to its actual purchase line even when its old metadata needs review. Use the controlled replacement below when a blocked manual Cost needs to remain an editable plan.

### Archive Settlement when posting the Forwarding Order

Choose **Post** on a Forwarding Order or post selected orders from the list to close them and create their transport history. TMS copies each saved **Income** and **Cost** row to the corresponding Posted Settlement row. Linking, changing, and posting sales or purchase documents prepares the Settlement values before this step.

**FWO posting preserves the stored values.** It does not synchronize document links, recalculate quantities, prices, amounts or exchange rates, repair technical states, or redistribute costs. Customer/vendor, dates, dimensions, document references, allocation shares, and migration and correction history are copied as saved. Existing Settlement rows on other FWOs are not refreshed.

The FWO still needs its Posting Date and must satisfy its status rules and enabled posting controls. **Posted Documents Control** requires each Settlement row's saved **Posted** marker to be true. **FWO Posting Date Control** compares the saved **Document Posting Date** with the FWO Posting Date. The configured allowed posting range also continues to apply. **Link State**, **Data Verification Status**, **Source Quantity Mode**, and an unknown correction baseline do not add separate FWO posting restrictions. Rows still marked **Settlement Upgrade Required** must first be converted to the new amount model.

Only the chosen orders are archived. Their active Settlement rows are removed after the copies and totals have been checked. A posting error rolls back the order or selected batch. Sales and purchase documents and their financial entries are not posted again.

Review or correct financial values through the relevant document workflow before closing the FWO. Archiving does not establish missing correction history.

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
| A different customer cannot be selected | Income belongs to the FWO Ordering Party. Change the FWO customer while all Income rows are eligible drafts. |
| More lines were invoiced than the selected row | Create uses all compatible unlinked lines for that effect. Add to Document uses the current or selected lines. |
| The same vendor cost appears twice | A whole-document import creates new rows. If a planned Cost existed, use a single-line link instead and review the duplicate before posting. |
| A purchase invoice cannot be found or linked | Follow [Purchase Assignment troubleshooting](purchaseassignment.md#troubleshooting); choose the unposted or posted invoice action. Direct linking has no unposted document-status restriction. Check source eligibility, Charge mapping, and existing document or allocation references. Old vendor, effect, and Needs Review markers alone do not block a single-line link. |
| A linked document line cannot be deleted | Use Detach Settlement first; it includes supported active and archived direct references. Freight allocations and correction chains require their own workflow. |
| Forwarding Order posting is blocked | Check the FWO status and Posting Date, then the enabled Posted Documents, document-date, and allowed-posting-range controls. Complete the amount-model wizard for rows marked Settlement Upgrade Required. Other technical Settlement states retain their existing archive rules. |
| A linked Income or Cost shows Unresolved or stale values | Review it through the relevant financial document workflow before closing the FWO. FWO posting copies the saved row without repairing it. |
| Settlement amounts differ from the financial document | Check the linking or financial posting workflow. FWO posting preserves the stored amounts and does not recalculate them. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Create and Invoice Your First Forwarding Job](first-forwarding-job.md)
- [Services](services.md)
- [Charges](charges.md)
- [Freight Agreements](freightagreement.md)
- [Purchase Assignment](purchaseassignment.md)
- [Pricing](pricing.md)
- [TMS Setup](setup.md)
