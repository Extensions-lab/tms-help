---
title: "Settlement"
description: "Understand Settlement signs, Financial Effect and Pricing Mode, enter income, and connect sales and purchase documents."
---

# Settlement

Use **Settlement** on a Forwarding Order to plan customer income and vendor costs, connect them to Business Central documents, and review profit.

Start with [Settlement principles](#settlement-principles), then follow [Add an Income line](#add-an-income-line) or [Purchase Assignment](purchaseassignment.md) for the task you need.

> **Entering an ordinary cost:** in **Per Unit**, enter positive **Quantity** and **Price**; TMS calculates a negative **Amount**. In **Fixed Amount**, enter the negative **Amount** yourself. Do not apply the same input rule to both modes.

## Before you start

Make sure that:

- the Forwarding Order status allows Settlement changes;
- **Ordering Party** identifies the customer when you will create sales documents;
- [Services](services.md) have the required **Invoice Line - Type**, **Invoice Line - No.**, and unit of measure;
- [Charges](charges.md) have purchase mappings when you will create or link purchase lines;
- customers, vendors, posting groups, VAT setup, currencies, and exchange rates are ready;
- [Freight Agreements](freightagreement.md) are configured if carrier rates come from agreements;
- posting controls are configured in [TMS Setup](setup.md).

Enter and edit lines in the Forwarding Order **Settlement** section. The separate **Settlement Lines** list is read-only and is useful for reviewing document links and verification status.

## Settlement principles

### What a line represents

A Settlement line is one income or cost contribution to a Forwarding Order. It can start as a planned amount before an invoice exists. Creating a Settlement line does not post a sales or purchase invoice.

| Setting | Question it answers | Values |
|---|---|---|
| **Type** | Which side of the job does this line belong to? | **Income** for customer services; **Cost** for vendor or carrier charges. |
| **Code** | What is being billed or costed? | A Service for Income; a Charge for Cost. |
| **Financial Effect** | Is this normal billing or a credit? | **Standard** for an Order or Invoice; **Credit** for a Credit Memo. |
| **Pricing Mode** | Which values do I enter? | **Per Unit**: Quantity and Price. **Fixed Amount**: the signed total Amount. |
| **Document Type / Document No.** | Which financial document is connected? | Filled by creating, importing, or linking a document line. |

A credit does not change the side: a customer credit remains **Income**, and a vendor credit remains **Cost**.

### Financial Effect

Choose **Financial Effect** before entering the financial values.

- **Standard** is the normal direction: customer income increases profit; vendor cost reduces profit.
- **Credit** reverses that direction: a customer credit reduces income; a vendor credit reduces cost and increases profit.

Financial Effect selects the direction and compatible document family. It does not choose between an Order and an Invoice. Use the exact action under **Create**, for example **Sales Invoice**.

The sign of Amount alone does not identify a Credit Memo. Negative quantities or prices can produce a negative line on an ordinary invoice. Select **Credit** when the intended document is a Credit Memo.

### Pricing Mode

| Mode | Enter | TMS calculates | Use when |
|---|---|---|---|
| **Per Unit** | **Quantity**, **Unit of Measure Code**, and **Price** | **Amount** from quantity, price, Type, and Financial Effect. | A rate per trip, pallet, hour, kilometre, or weight unit. |
| **Fixed Amount** | The signed **Amount**, plus the quantity and unit appropriate to the service. | **Price** from the total and quantity. Changing quantity preserves Amount. | A negotiated total that must stay fixed when distance, weight, or quantity changes. |

In **Per Unit**, Amount is calculated and cannot be edited directly. In **Fixed Amount**, Price is calculated and cannot be edited directly.

For a simple fixed fee, use **Quantity = 1** and the appropriate unit. A deliberately entered **Fixed Amount** with **Quantity = 0** is also supported: Settlement keeps quantity zero, while the created financial document uses quantity 1 to carry the total. **Unit of Measure Code** is still required for Income. This does not allow arbitrary existing document lines with zero quantity and a nonzero amount to be imported.

When changing modes, check the result again. Switching to Per Unit makes quantity and price control the total.

### Quantity, Price, and Amount signs

**Amount** is the signed contribution to the job result. The following examples use **Quantity = 1**, **Price = 100**, one currency, and no VAT or discounts.

| Type | Financial Effect | Quantity entered in Per Unit | Price entered in Per Unit | Calculated Amount | Intended document |
|---|---|---:|---:|---:|---|
| Income | Standard | 1 | 100 | +100 | Sales Order or Sales Invoice |
| Income | Credit | 1 | 100 | -100 | Sales Credit Memo |
| Cost | Standard | 1 | 100 | -100 | Purchase Order or Purchase Invoice |
| Cost | Credit | 1 | 100 | +100 | Purchase Credit Memo |

For an unlinked Per Unit draft:

**Amount = Quantity × Price × direction**

The direction is **+1** for Income Standard and Cost Credit, and **-1** for Cost Standard and Income Credit.

For **Fixed Amount**, enter the value in the **Calculated Amount** column yourself. The same economic signs apply, but Amount is your input.

Examples:

- Ordinary carrier cost of 700, Per Unit: **Cost**, **Standard**, **Quantity = 1**, **Price = 700** → **Amount = -700**.
- The same cost as a fixed total: **Cost**, **Standard**, **Fixed Amount**, **Quantity = 1**, **Amount = -700** → derived **Price = 700**.
- Vendor credit of 100: **Cost**, **Credit**, **Quantity = 1**, **Price = 100** → **Amount = +100**.

> Do not enter **Price = -700** or **Quantity = -1** merely because a Per Unit line is a cost. With Cost + Standard, either change makes Amount positive and increases profit. Negative quantity and price are supported for deliberate signed adjustments; they are not the usual way to record a cost or choose a credit document.

In Fixed Amount, changing **Financial Effect** preserves the entered Amount and recalculates Price. It does **not** automatically reverse the total. For example, changing an Income fixed fee of +100 from Standard to Credit leaves Amount at +100. To grant a credit of 100, enter **Amount = -100**, or use **Create Credit Line** from the original Standard line.

### How profit is calculated

Totals use **Amount (LCY)** so that lines in different currencies can be compared.

| Example line | Amount (LCY) |
|---|---:|
| Customer income | +1,000 |
| Carrier cost | -700 |
| **Total Profit (LCY)** | **300** |

**Total Cost (LCY)** is already signed. Profit is **Total Income (LCY) + Total Cost (LCY)**. Do not subtract the negative cost a second time.

Planned and linked lines both affect the result. If an invoice is for a cost already planned, link it to that existing Cost line; importing it as another line can count the cost twice.

## How Settlement relates to documents

A link connects a Settlement row to a specific Sales or Purchase **line**, not just to an invoice number. A purchase line can also be allocated across several Forwarding Orders.

| Stage | What the amounts mean | Where to make changes |
|---|---|---|
| Planned, unlinked | Expected income or cost. | Edit a manual or Default Service Set draft in Settlement. |
| Linked to an open document | Values from the actual document line, including its line discount and currency context. The document is not yet posted. | Make supported price changes on the open financial document. |
| Linked to a posted document | Values and references from the posted invoice or credit memo. | Use the supported correction workflow. Posted amounts are not editable. |
| Posted Forwarding Order | A read-only Settlement snapshot in transport history. | Review the posted records; do not edit history. |

After linking, Amount comes from the document's net amount excluding VAT, adjusted for Income/Cost, document type, and any allocation share. It can differ from Quantity × Price because of a line discount or allocation. Do not overwrite it to force the planned formula.

An ordinary document quantity is represented as **Per Unit** after linking, even if the draft was Fixed Amount. The explicit zero-quantity fixed-fee case retains Fixed Amount.

These actions have different purposes:

| Starting point | Action | Result |
|---|---|---|
| Settlement has the lines; no financial document exists. | **Create** → the required Sales or Purchase document. | Creates a new document and links its new lines. |
| Settlement has the lines; an open financial document exists but needs new lines. | **Add to Document** → **Select Sales Document** or **Select Purchase Document**. | Adds new financial lines to that document and links them. |
| A purchase invoice already contains the cost line; Settlement has its planned Cost. | **Import to Settlement** → **Single Purchase Document Line**. | Connects the existing purchase line to the current Cost line. |
| A financial document contains lines that are not yet in Settlement. | **Import to Settlement** → the appropriate import action. | Creates Settlement rows from eligible existing document lines. |

**Add to Document** does not match an existing invoice line. Use the single purchase line link when both sides already exist.

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
10. Check **Amount**, **Amount (LCY)**, and the order totals. Confirm the sign before creating a financial document.

### Choose values for your Income scenario

All examples below are in one currency and exclude VAT.

| What the customer agreed | Financial Effect | Pricing Mode | Values to enter | Expected Amount |
|---|---|---|---|---:|
| One transport service at 1,000 | Standard | Per Unit | Quantity 1; the service unit; Price 1,000. | +1,000 |
| 10 pallets at 25 each | Standard | Per Unit | Quantity 10; the pallet unit; Price 25. | +250 |
| A total fee of 1,000 regardless of quantity changes | Standard | Fixed Amount | Quantity 1; the service unit; Amount +1,000. | +1,000 |
| Credit the customer 100 | Credit | Per Unit | Quantity 1; the service unit; Price 100. | -100 |
| A fixed customer credit of 100 | Credit | Fixed Amount | Quantity 1; the service unit; Amount -100. | -100 |

For distance- or weight-based billing, use Per Unit and the correct unit and rate. On verified, unlinked Income lines, **Recalculate Settlement** updates the quantity from transportation data when **Calc. Qty. as Distance** or **Calc. Qty. as Weight** is configured. With Fixed Amount, it preserves the agreed total and adjusts the derived Price. Linked, posted, and Needs Review lines are not recalculated this way.

For automatic pricing, use **Get Prices** when enabled and review the generated lines. Price-list or agreement-controlled lines must be maintained through their source workflow. A line copied from a Default Service Set remains an editable draft until linked.

### Credit an existing Income or Cost line

1. Select the original line with **Financial Effect = Standard**.
2. Choose **Line** → **Create Credit Line**.
3. Review the new independent draft. It keeps the original Type, has **Financial Effect = Credit**, and its Amount is the exact opposite of the original Amount.
4. Adjust the credit draft to the agreed value while it is editable. In Per Unit, adjust quantity or price. In Fixed Amount, adjust the signed Amount.
5. Create the appropriate **Sales Credit Memo** or **Purchase Credit Memo**.

Create Credit Line preserves quantity and price where the pricing mode allows it. If a source discount prevents those values from reproducing the exact credit total, the new draft uses Fixed Amount.

This action creates a draft; it does not post a correction or modify the original document. For an exact reversal, **Create Full Reversal** is available for a fully open posted invoice containing exactly one line. It reverses the complete Settlement group linked to that line, including all purchase allocations. Supply a correction reason. The data must be verified, the FWO must not be archived, and the original group must not already have a Full Reversal.

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

To bring existing open Sales Order or Invoice lines into Settlement, use **Import to Settlement** → **Sales Order or Invoice**. The document must match the FWO customer and invoice currency.

## Purchase documents

Enter an expected vendor charge as **Type = Cost**, select its **Code**, confirm **Customer/Vendor Name**, and choose Financial Effect and Pricing Mode using the sign rules above.

When the vendor invoice arrives, decide whether its lines already exist in Business Central:

| Situation | What to do |
|---|---|
| The invoice has an existing line for your planned Cost. | [Link that purchase line to the existing Cost](purchaseassignment.md#link-one-purchase-line-to-an-existing-cost). |
| An open invoice has several costs that are not yet recorded in this FWO. | [Import the whole purchase document](purchaseassignment.md#import-a-whole-purchase-document). |
| One vendor line covers several Forwarding Orders. | [Allocate the purchase line across orders](purchaseassignment.md#allocate-one-purchase-line-across-forwarding-orders). |
| Settlement contains costs and you want TMS to create a new purchase document. | Use **Create** → **Purchase Invoice**, **Purchase Order**, or **Purchase Credit Memo**. Creation uses compatible unlinked Cost lines for the selected vendor and Financial Effect. |
| An open purchase document needs new lines from Settlement. | Select the Cost lines and use **Add to Document** → **Select Purchase Document**. |

See [Purchase Assignment](purchaseassignment.md) for prerequisites, matching rules, Freight Order assignment, step-by-step linking, and removing an incorrect assignment.

Link or allocate purchase lines **before posting**. Attaching an already posted purchase invoice or credit memo is not supported by the current workflow.

## Currencies and dates

| Field | How to use it |
|---|---|
| **Currency Code** | Currency of the planned service or cost. Blank means LCY. |
| **Amount (LCY)** | Converted signed amount used in the FWO totals. |
| **Exchange Rate Date** | Date used for the planned exchange rate. Linking adopts the financial document's exchange-rate context. |
| **Posting Date** on Settlement | The line's own date. Changing the FWO date does not replace an existing line date. |
| **Document Posting Date** | The Posting Date of the linked financial document. |
| **Currency Code** in the FWO **Ordering Party** section | Invoice currency used when creating a sales document. It may differ from planned Income line currencies. |

For example, a planned service of 100 USD can be invoiced in EUR. Creation converts the service using the sales document's Posting Date and Currency Factor. After linking, Settlement shows the actual EUR values and currency context from that sales line.

A new Sales document takes its Posting Date from the current FWO; Income line dates can differ. Purchase document creation requires a common Posting Date across the selected Cost lines. A combined Sales Invoice requires a common Posting Date across the selected FWOs.

Before Income lines are linked, eligible drafts can use a different FWO invoice currency without rewriting their service prices. Once any Income is linked or posted, the FWO **Order Party Type**, customer, and **Currency Code** in **Ordering Party** are protected. Changing the customer is also blocked by customer-specific agreement/price-list lines or data requiring review.

Business Central controls **Bill-to Customer** and **Pay-to Vendor** on document headers. Settlement identifies the operational **Sell-to Customer** or **Buy-from Vendor**.

## Change or remove a document link

A linked Settlement line takes its financial values from the document. On an open Sales line, supported repricing uses **Unit Price**, line discount, or **Line Amount**. On an open Purchase line, use **Direct Unit Cost**, line discount, or **Line Amount**. TMS updates the linked Settlement values and the complete purchase allocation group.

Other protected changes, such as quantity, unit, or source identity, require detaching first. A supported currency change on an open document can update the linked currency and exchange rates automatically; review the result in Settlement.

| Action | Result |
|---|---|
| **Detach Settlement** on an open Sales line | Removes its direct Income link. Keeps the document line and Settlement row. |
| **Delete Charge Allocation** on an open Purchase line | Removes the whole direct allocation group for that source line. Keeps the document line and all Settlement rows. |
| **Import to Settlement** → **Delete Allocation** in Settlement | Removes the direct purchase allocation group referenced by the selected Cost line. |

After direct detach, Settlement keeps its last signed amount and transaction currency as an unlinked **Fixed Amount** plan. Detach does not delete the financial document line. Delete it separately only if it is no longer needed; leaving it and creating another invoice can duplicate billing.

Posted links, legacy links, and Freight allocation history cannot be removed by the direct detach actions. Use the appropriate source workflow. See [Remove or change an assignment](purchaseassignment.md#remove-or-change-an-assignment).

## Posting and supported scope

Review Settlement before posting the Forwarding Order. If **Posted Documents Control** or **FWO Posting Date Control** is enabled in TMS Setup, financial documents and dates must satisfy those controls first.

The supported document workflow covers Sales Orders, Invoices, and Credit Memos; Purchase Orders, Invoices, and Credit Memos; and full posting where each open source line produces one posted line.

For a linked Sales Order, post the full quantity with **Ship and Invoice**. For a linked Purchase Order, use **Receive and Invoice** for the full quantity. Posting only the shipment or receipt first is not supported.

Use compatible customer/vendor, currency, and dimension settings. Creating a document normally works within one FWO; the dedicated combined Sales Invoice workflow is the exception. Adding to an existing document has its own compatibility checks.

The following are outside the supported Settlement workflow:

- partial shipment, receipt, or invoicing, separate shipment/receipt posting, and combining sources into posted lines;
- prepayments, deferrals, warehouse handling, reservations, and item tracking;
- Purchase Return Orders for physical return and item application;
- importing or attaching already posted Sales or Purchase lines;
- corrections after the FWO is archived, or split/merge correction chains.

A row marked **Needs Review**, **Unresolved**, or legacy requires review before it can be used as a verified financial source. Do not change signs or recreate rows merely to bypass that state.

## Troubleshooting

| Problem | What to check |
|---|---|
| An ordinary Cost increases profit | In Per Unit, check for a negative Quantity or Price and confirm Standard. In Fixed Amount, enter a negative Amount for the ordinary cost. |
| A credit has the wrong sign | Confirm Type and Credit. In Fixed Amount, changing Financial Effect does not reverse Amount; enter the intended signed total. |
| Amount is read-only | The draft is Per Unit, so edit Quantity and Price. Linked, posted, and source-controlled rows have additional protection. |
| Price is read-only | The draft is Fixed Amount, so edit Amount. |
| A fixed total changed after invoice linking | The document now supplies the actual net amount, discounts, and currency. A normal nonzero quantity is shown as Per Unit after linking. |
| Income cannot be invoiced | Complete Unit of Measure Code on every affected Income row; check service invoice mapping, customer, status, VAT/posting setup, and currency. |
| A different customer cannot be selected | Income belongs to the FWO Ordering Party. Change the FWO customer while all Income rows are eligible drafts. |
| More lines were invoiced than the selected row | Create uses all compatible unlinked lines for that effect. Add to Document uses the current or selected lines. |
| The same vendor cost appears twice | A whole-document import creates new rows. If a planned Cost existed, use a single-line link instead and review the duplicate before posting. |
| A purchase invoice cannot be found or linked | Follow [Purchase Assignment troubleshooting](purchaseassignment.md#troubleshooting); check open status, Buy-from Vendor, Charge mapping, effect, and existing allocations. |
| A linked document line cannot be deleted | Detach the direct link first. Posted or Freight history needs its own workflow. |
| Forwarding Order posting is blocked | Check required document posting, dates, and Settlement verification status. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Create and Invoice Your First Forwarding Job](first-forwarding-job.md)
- [Services](services.md)
- [Charges](charges.md)
- [Freight Agreements](freightagreement.md)
- [Purchase Assignment](purchaseassignment.md)
- [Pricing](pricing.md)
- [TMS Setup](setup.md)
