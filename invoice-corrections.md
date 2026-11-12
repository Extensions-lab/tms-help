---
title: "Invoice Corrections and Corrective Forwarding Orders"
description: "Correct or cancel posted sales and purchase invoices, follow their Settlement links, and review the financial result of active and posted Forwarding Orders."
---

# Invoice Corrections and Corrective Forwarding Orders

Use **Correct**, **Cancel**, or **Create Corrective Credit Memo** on a posted sales or purchase invoice whose lines are linked to Settlement. TMS links the resulting financial documents to the affected Forwarding Orders (FWOs).

Sales corrections create **Income** rows; purchase corrections create **Cost** rows. The original invoice rows retain their document references and amounts. Separate rows record the credit and, for **Correct**, the replacement invoice.

## Before you start

- Check the original invoice's line-level Settlement assignments. One invoice can include lines belonging to different FWOs.
- For corrections involving posted FWOs, select **TMS Setup > Forwarding Orders > Correction FWO Type Code**. The type must have **Financial Correction = Yes**, a working **No. Series**, and a default status that allows Settlement, the required Income or Cost lines, and posting.
- Check the usual Business Central eligibility, permissions, posting dates, and financial setup. TMS does not replace the standard invoice correction checks.

See [corrective FWO setup](setup.md#corrective-forwarding-orders) for the type requirements.

## Choose a correction action

| Action | What Business Central creates | What appears in Settlement |
|---|---|---|
| **Correct** | A credit memo that is posted and applied to the original invoice, followed by a new **unposted invoice** for the user to revise. | Separate **Credit** rows linked to the posted credit memo and **Standard** rows linked to the replacement invoice. Editing and posting the replacement updates its own links. |
| **Cancel** | A full credit memo that is posted and applied to the original invoice. No replacement invoice is created. | **Credit** rows linked to the posted credit memo. The original invoice allocations remain visible alongside the reversal. |
| **Create Corrective Credit Memo** | An **unposted credit memo** with the applicable original-invoice reference. The user reviews, edits, and posts it. | **Credit** rows linked to the unposted credit memo. Changes to its quantities, prices, discounts, or amounts update the linked Settlement. Posting moves those links to the posted credit-memo lines. |

Use **Correct** to replace an incorrect invoice, **Cancel** to reverse it in full, and **Create Corrective Credit Memo** to prepare an editable full or partial credit.

## How to correct an invoice

1. Open **Posted Sales Invoices** or **Posted Purchase Invoices** and select the original invoice.
2. Choose the required correction action from the invoice's **Correct** menu.
3. If TMS asks to create corrective FWOs, review the configured type and confirm. Declining stops the correction action.
4. Review the credit memo and any replacement invoice. TMS creates links for the copied lines that had an original Settlement assignment.
5. For an editable credit or replacement invoice, make changes in the **financial document** and post it when ready. Review its updated Settlement allocations.
6. Open each affected FWO and review its Settlement and [financial result](#financial-result). Post a separate corrective FWO when its financial work and posting checks are complete.

New financial-document lines without an inherited original allocation need their own supported Settlement assignment. Do not import a copied line again when TMS has already linked it through the correction action.

## Where the Settlement rows are created

| State of the original FWO | Result for all three actions |
|---|---|
| **Active, not posted** | The new credit and replacement rows are added to the **same FWO**. Its existing invoice rows remain in place. Its ordinary Settlement totals include all these rows. |
| **Posted** | TMS asks to create a separate **active financial correction FWO**, using the configured correction type and the original FWO's **Ordering Party**. New Settlement belongs to that corrective FWO. The original posted FWO and its Settlement and Freight history remain unchanged. |

TMS creates **one corrective FWO per original FWO and correction operation**. Several invoice lines for the same original order use that same corrective order. **Correct** puts both its credit and replacement invoice allocations on the same corrective FWO.

A financial correction FWO records financial adjustments. Its layout hides cargo, logistic units, planning, stages, and transport actions. Creation does not add transportation stages, cargo, or automatic Service Set and agreement charges.

Each corrective FWO refers to one original FWO. An original FWO can have several corrective orders from separate operations. The financial document's customer or vendor remains on its Settlement lines; a purchase vendor does not become the corrective FWO's Ordering Party.

If the original has **Ordering Party Type = Company**, the corrective FWO retains Company. You can still import an existing sales invoice or credit memo: its Sell-to Customer is saved on the imported Income without changing the corrective FWO's Ordering Party. This applies to both open and eligible posted sales documents.

### Invoices involving several FWOs

TMS follows the **line-level assignments**. Different lines of one sales or purchase invoice can belong to different FWOs. Active allocations stay on their active orders; allocations belonging only to posted history are directed to their corresponding corrective orders.

A **purchase line** can also be distributed between several FWOs. Its correction keeps the saved allocation factors. For example, a partial credit of **200** against a **60/40** allocation produces credit shares of **120/80**. Quantity, amount, discount, and currency rounding preserve the source totals. An unchanged automatic full reversal keeps the exact original shares, including their rounding remainders.

One sales invoice line has one original Settlement allocation. Multiple sales invoice lines can still belong to different FWOs. Identical historical copies of an allocation do not multiply its correction; a matching active copy takes precedence.

## Edit or post an open corrective document

Edit quantities, prices, discounts, and amounts in the credit memo or replacement invoice. TMS synchronizes its managed Settlement rows from the current financial document, using the original allocation factors where the source is distributed.

This also applies when **Cancel** or **Correct** leaves a credit memo unposted after an unsuccessful posting attempt. The retained credit can be edited and posted manually. TMS validates its current amounts and links; it does not require its price or quantity to remain equal to the original invoice.

Manual posting of a retained credit memo **does not resume Correct**: it does not create a replacement invoice or complete that action's invoice-cancellation tracking. Review the original invoice and any further documents through the normal Business Central workflow.

Deleting a supported unposted corrective-credit or replacement-invoice line removes its managed active Settlement rows. Deleting a replacement invoice does not undo a credit memo already posted by **Correct**.

## Financial result

Settlement uses **P/L Amount (LCY)** for the financial result:

- **Income / Standard** increases profit; **Income / Credit** reduces it.
- **Cost / Standard** reduces profit; **Cost / Credit** increases it.
- Enter document quantities and prices as they belong on the financial document. A credit memo's contribution is reversed through **Financial Effect = Credit**; do not add a minus sign solely to identify a credit.

On an **active original FWO**, the new rows are part of its own Settlement. Its ordinary income, cost, and profit totals already include the correction.

When separate corrective FWOs exist, **Settlement > Result Including Corrections** shows:

| Field | Meaning |
|---|---|
| **Order Profit (LCY)** / **Saved Profit (LCY)** | The current active FWO's own result, or the saved result of the exact posted version opened on the card. |
| **Corrections (LCY)** | The combined financial effect of its separate corrective FWOs, including later corrections in the chain. Choose the value to open the corrective-order list. |
| **Profit Including Corrections (LCY)** | The original order's own result plus the correction effect. |

Each corrective order is counted once: TMS uses its active record when present, otherwise its latest posted version. Filtering Settlement lines does not change these order-wide totals.

**The result includes unposted financial documents linked to active Settlement.** Editing an open credit or replacement invoice changes the financial assessment before posting. The combined result is therefore not limited to posted ledger entries.

### Example: replacing a sales invoice

All figures below are in LCY, excluding VAT.

| Item | Profit effect |
|---|---:|
| Original income 1,000 less costs 700 | 300 |
| Credit of the original sale | −1,000 |
| Replacement sale of 1,200 | +1,200 |
| **Net correction effect** | **+200** |
| **Profit including corrections** | **500** |

For an active original FWO, its own Settlement profit becomes **500**. For a posted original, the saved profit remains **300**, the corrective FWO contributes **200**, and **Profit Including Corrections (LCY)** shows **500**. The replacement invoice contributes while it is still unposted.

## Create a corrective FWO manually

You can also create a financial adjustment without starting one of the invoice correction actions:

1. Create a new Forwarding Order and select a type with **Financial Correction = Yes**.
2. In **General > Correction**, select **Corrects Forwarding Order**. TMS saves the latest posted version of the selected original and copies its saved Ordering Party details, currency, Responsibility Center, Salesperson, and Dimensions. The usual customer and status checks apply.
3. Enter or assign the required Income and Cost Settlement lines and financial documents. Selecting an original FWO does not copy its Settlement or create a credit memo.
4. Complete the applicable controls and post the corrective FWO.

For a manual correction, the original can be selected, changed, or cleared with existing Settlement. Selecting an original updates the customer on unlinked Income and the inherited Dimensions on editable Income drafts; linked and posted rows and Cost rows remain unchanged. An original is required when posting the corrective FWO. An automatically created corrective FWO keeps the original selected by its invoice operation. See [manual financial corrections](forwardingorder.md#create-a-financial-correction-manually).

## Fields that matter most

| Field or action | Where to find it |
|---|---|
| **Correction FWO Type Code** | TMS Setup; selects the financial type for automatic corrective orders. |
| **Financial Correction** | FWO Type and the saved FWO classification. Also available as a column in Posted Forwarding Orders. |
| **Corrects Forwarding Order** | General > Correction on the corrective FWO; identifies its original posted order. |
| **Has Corrections** | General > Correction on the **original posted FWO**; opens its corrective orders. This field is not shown on active FWO cards. |
| **Financial Effect**, **Pricing Mode**, **Customer/Vendor No.** | Settlement; also shown as read-only saved values in Posted FWO Settlement. |
| **Result Including Corrections** | Below the usual Settlement totals when the order has separate corrections. |

## Good to know

- Managed correction Settlement must reference posted financial sources before the FWO can be posted, even if **Posted Documents Control** is disabled. Other status, date, and posting controls still apply.
- A financial correction FWO must have an original posted FWO selected before posting. It cannot be deleted while Settlement rows remain.
- Ordinary **Copy Document** and independently created credit memos need explicit Settlement assignment or import. They do not automatically inherit this invoice-action integration.
- The workflow applies to new correction operations. It does not rebuild earlier corrections or change the original posted history.

## Troubleshooting

| Problem | What to check |
|---|---|
| A corrective FWO cannot be created | Check Correction FWO Type Code, Financial Correction on the type, its number series, and default-status permissions. |
| A copied financial line has no automatic allocation | Check whether the original invoice line had a Settlement assignment. Newly added lines need their own assignment. |
| Settlement amounts differ from the edited credit | Check the current financial line and its allocation group. Edit the source document and use the supported synchronization workflow. |
| A retained credit posted without a replacement invoice | Manual posting does not resume Correct. Review what was created and complete the required financial documents separately. |
| Profit changed before the replacement invoice was posted | Unposted linked documents contribute to active Settlement totals. Review the open documents in the corrective-order list. |
| The original FWO cannot be changed on a corrective order | Automatic orders keep their original. Manual orders allow changes with Settlement; check customer and status permissions and choose a posted order without a circular correction link. |
| The corrective FWO cannot be posted | Check its original-order reference, managed financial documents, completed links, and the usual FWO posting controls. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Settlement](settlement.md)
- [Purchase Assignment](purchaseassignment.md)
- [Posted History](postedhistory.md#invoice-cancellation-in-posted-history)
- [TMS Setup](setup.md#corrective-forwarding-orders)
