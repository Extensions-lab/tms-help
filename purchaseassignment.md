---
title: "Purchase Assignment"
description: "Link vendor invoices to planned Settlement costs, import new costs, and allocate one purchase line across Forwarding Orders."
---

# Purchase Assignment

Use purchase assignment to connect a vendor's Business Central purchase line to the transportation cost it supports. A link identifies the **document type, document number, and line number**. Entering a vendor invoice reference or attaching an invoice file alone does not create this financial link.

You can match an existing planned **Cost** line, import new Cost lines, or distribute a purchase line across several Forwarding Orders. After linking, the purchase document supplies the actual quantity, price, discounts, currency, and amount. Posting the purchase document updates the existing link to the posted document.

For the meaning of **Financial Effect**, **Pricing Mode**, and positive or negative amounts, start with [Settlement principles](settlement.md#settlement-principles).

## Before you start

Make sure that:

- the Forwarding Order exists and its status allows Settlement changes;
- the purchase document is an **Order**, **Invoice**, or **Credit Memo**, is unposted, and has **Status = Open**;
- **Buy-from Vendor**, **Pay-to Vendor**, posting date, currency, and the purchase lines are complete;
- each relevant [Charge](charges.md) has **Purchase Line - Type** and **Purchase Line - No.** matching the purchase line's **Type** and **No.**;
- the source line has not already been assigned through Settlement or a Freight Order;
- the source is within the [supported Settlement document scope](settlement.md#posting-and-supported-scope).

For linked Settlement, use prices excluding VAT and no invoice discount, prepayment, or deferral. Complete the link before posting or partially processing the purchase line. Comment lines cannot carry a cost allocation.

For direct Settlement assignment, **Customer/Vendor Name** on a Cost line identifies the purchase document's **Buy-from Vendor**. **Pay-to Vendor** remains on the purchase document; it can differ from Buy-from Vendor. Matching only Pay-to Vendor is not enough.

## Choose the right workflow

| Your situation | Action and result |
|---|---|
| The vendor invoice line already exists, and you already planned its cost in Settlement. | Use [Single Purchase Document Line](#link-one-purchase-line-to-an-existing-cost). The current planned Cost line receives the existing purchase line. |
| The purchase document contains costs that have not yet been entered in Settlement. | Use [Whole Purchase Document](#import-a-whole-purchase-document). TMS creates new Cost lines for its eligible, unallocated lines. |
| One vendor invoice line covers several Forwarding Orders. | On the purchase line, use [TMS > Charge Allocation](#allocate-one-purchase-line-across-forwarding-orders) and choose the exact planned Cost line for each order. |
| You have planned Cost lines, but the purchase document or its lines do not exist yet. | Use [Create or Add to Document](#create-purchase-lines-from-planned-costs). These actions create purchase lines. |
| The carrier invoice belongs to a Freight Order and its related Forwarding Orders. | Use the [Freight Order assignment workflow](#assign-a-carrier-invoice-through-a-freight-order). |

**Add to Document** adds new purchase lines. To connect an invoice line that has already been entered, use **Import to Settlement** or **Charge Allocation**. Otherwise you can duplicate the vendor charge.

## Link one purchase line to an existing cost

Use this when, for example, you planned a carrier cost of 500 and now have the vendor's invoice for 520. The link replaces the planned values with the actual values on the same Settlement row.

1. Open the **Forwarding Order** and expand **Settlement**.
2. Select the existing unlinked **Cost** line. Check its **Code** and **Customer/Vendor Name**.
3. Set **Financial Effect** to **Standard** for a Purchase Order or Purchase Invoice, or **Credit** for a Purchase Credit Memo.
4. Choose **Import to Settlement > Single Purchase Document Line > Purchase Invoice Line**. For another document type, choose **Purchase Order Line** or **Purchase Credit Memo Line**.
5. In **Purchase Document Assignment**, find the document using **Document No.** and **Vendor Invoice No.**. The list contains open documents for the Cost line's Buy-from Vendor.
6. Select the one purchase line that belongs to this cost and confirm the lookup. Check **Type**, **No.**, **Quantity**, **Direct Unit Cost**, **Line Amount**, and currency before confirming. Do not select a comment or an already allocated line.
7. Review the updated Settlement row. Confirm its document reference, **Financial Effect**, quantity, price, amount, currency, and dimensions.

The selected Charge must map to the purchase line's exact **Type** and **No.** Matching the description or amount is not sufficient. If several Charges use the same purchase posting object, selecting the required **Code** on the planned Cost line makes the intended Charge explicit.

This action works on the **current Cost line**, not a batch of selected Settlement rows. Repeat it for other invoice lines. A Cost row already linked to another source must first be detached through the supported workflow.

For a normal quantity-based line, TMS copies the purchase quantity and **Direct Unit Cost**, changes **Pricing Mode** to **Per Unit**, and takes the actual amount after discounts. It also copies the purchase description, unit of measure, currency, exchange-rate context, and direct line dimensions. The old estimate is replaced; it is not retained as an additional cost.

**Example:** a planned Cost of -500 linked to a Purchase Invoice line with quantity 1 and amount 520 becomes a linked Cost of **-520**. TMS does not add another -520 row when you use this single-line action.

## Import a whole purchase document

Use this when the purchase costs do not already have planned Settlement rows and all free lines on the selected document belong to the current Forwarding Order.

Whole-document import requires a **User Setup** record with **TMS Allow Purchase Document Allocation** enabled for your user. An administrator can enable this option. It controls the **Whole Purchase Document** action group.

1. Open the **Forwarding Order** and expand **Settlement**.
2. Choose **Import to Settlement > Whole Purchase Document > Purchase Invoice**. Use **Purchase Order** or **Purchase Credit Memo** for those document types.
3. Select the open document. For invoices and credit memos, review **Buy-from Vendor**, the vendor reference, **Currency Code**, **Allocation Status**, and **Free Lines**.
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

## Allocate one purchase line across Forwarding Orders

Use this when one vendor invoice line is shared by several jobs. TMS splits the line's quantity and amounts across the selected Settlement rows; their total remains equal to the source line.

1. In each affected **Forwarding Order**, prepare an unlinked planned **Cost** row with the same Buy-from Vendor and the required Charge **Code**. Use **Standard** for an invoice or order, or **Credit** for a credit memo.
2. Open the unposted **Purchase Invoice**, **Purchase Order**, or **Purchase Credit Memo**.
3. Select the one purchase line to distribute.
4. In the line actions, choose **TMS > Charge Allocation**.
5. In **Select Allocation**, choose **Distribution Type**. Options include **Equal**, **By Number Of Logistic Units**, **By Gross Weight**, **Selected Forwarding Orders**, and **By Footage**. Choosing a distribution type resets the selection, so review the **Select** checkboxes afterward.
6. Select only the Forwarding Orders that should receive this cost. The list contains orders with compatible planned Cost rows.
7. Check **Settlement Line No.**, **Charge Code**, and **Settlement Description** for every selected order. If several compatible rows exist in one order, use the **Settlement Line No.** lookup to select the exact row. TMS fills it automatically only when there is one compatible row.
8. Review **Charges Distribution Ratio**. Every selected order must receive a positive share and the complete allocation must total 1 (100%). For weight, footage, or unit-based distribution, check that the underlying values are complete.
9. Confirm the allocation and review **Allocated Amount** on the purchase line and the linked Cost rows in each Forwarding Order.

The dialog selects **one Settlement target per Forwarding Order** for this purchase line. It does not automatically select the first Cost row or create missing planned rows. Repeat the process separately for other purchase lines.

**Example:** a Purchase Invoice line for 1,000 split equally between two Forwarding Orders creates actual Cost contributions of **-500** on each selected row. It does not assign the full -1,000 to both orders. Rounding differences are included in the final share so the total stays exact.

A purchase line cannot simultaneously have a direct Settlement allocation and a Freight Order charge allocation. An existing allocation must be removed through its own workflow before assigning the line again.

## Cost signs and vendor credits

Enter a normal purchase invoice or credit memo using the usual Business Central quantity and direct unit cost. TMS derives the Settlement direction from the source document type.

| Source line, with positive quantity and price | Financial Effect in Settlement | Settlement Amount for a source amount of 100 |
|---|---|---:|
| Purchase Order or Purchase Invoice | **Standard** | **-100**: increases cost and reduces profit. |
| Purchase Credit Memo | **Credit** | **+100**: reduces cost and increases profit. |

Do not negate **Quantity** or **Price** merely because a row is a Cost. For a planned **Per Unit** Cost with **Financial Effect = Standard**, quantity **1** and price **100** produce amount **-100** automatically. For a planned **Fixed Amount** Cost, **Amount** is already the signed financial contribution: enter **-100** for an ordinary cost, or **+100** for a vendor credit. See [quantity, price, and amount signs](settlement.md#quantity-price-and-amount-signs).

Negative quantities or prices reverse the arithmetic as well. They are not a substitute for selecting **Credit**, and changing the sign of a linked Settlement row is not how you create a vendor credit.

To record a new vendor credit, create or use an open **Purchase Credit Memo** and link it to a separate **Cost / Credit** row, or import its free lines. This records the cost reduction separately from the invoice. To reverse an already posted linked invoice, follow the supported Settlement correction workflow; do not detach or overwrite the posted original.

## Create purchase lines from planned costs

Use these actions only when the required purchase lines have not been entered yet:

- **Create > Purchase Order** or **Create > Purchase Invoice** creates a new document from compatible planned **Standard** Cost lines.
- **Create > Purchase Credit Memo** creates a new document from compatible planned **Credit** Cost lines.
- **Add to Document > Select Purchase Document** adds the current or selected Cost rows as new lines to a compatible open document. **Standard** offers orders and invoices; **Credit** offers credit memos.

For **Add to Document**, select only the intended unlinked Cost rows. They must match the selected document's Buy-from Vendor, Financial Effect, currency, and currency factor. Do not mix Standard and Credit in one selection. Creating a new Purchase document also requires compatible posting dates and dimensions across its Cost rows.

If the invoice line is already present, return to [single-line linking](#link-one-purchase-line-to-an-existing-cost) instead of adding it again.

## Review, edit, and post linked documents

After assignment, check the document reference and Settlement totals before posting. Linking is not posting: an open linked invoice remains unposted until you post it in Business Central.

For an open linked purchase line, standard edits to **Direct Unit Cost**, **Line Discount %**, **Line Discount Amount**, or **Line Amount** update the complete Settlement allocation. Review every affected Forwarding Order when a line is shared. The purchase document remains the source of these actual values.

Changes to protected details, such as quantity, unit of measure, posting object, or allocation membership, require the supported detach and reassignment workflow. An ordinary deletion of a linked purchase line is blocked. Supported standard header changes that recreate purchase lines preserve and refresh the links automatically.

Post the linked source in full. For a linked **Purchase Order**, choose **Receive and Invoice** for the full quantity in one posting operation. Separate receipt posting is unsupported even if you receive the full quantity.

Partial receipts, partial invoicing, combined receipts, item tracking, warehouse processing, prepayments, and deferrals are outside the supported Settlement flow. After successful posting, TMS changes the existing source reference to the **Posted Purchase Invoice** or **Posted Purchase Credit Memo** and marks the linked Settlement rows as posted. Open the related document from Settlement to verify it.

**Assign before posting.** You cannot newly import or attach a Posted Purchase Invoice or Posted Purchase Credit Memo. A posted document reference appears because an already linked open document was posted. It does not mean that posted documents can be matched afterward.

## Remove or change an assignment

For a direct allocation from an open purchase line:

1. Select the linked Cost row in **Settlement** and choose **Import to Settlement > Delete Allocation**. Alternatively, open the source purchase document, select its line, and choose **TMS > Delete Charge Allocation**.
2. Review the affected purchase line and confirm removal.
3. Check all affected Forwarding Orders. The action detaches the **complete allocation of that purchase line**, including rows in other orders.
4. Confirm that the document references are cleared. The Settlement rows and their latest signed amounts remain as planned costs, with **Pricing Mode = Fixed Amount**.
5. Edit the source or planned rows as required, then link or allocate the purchase line again. If the source line itself must be deleted, delete it separately after detaching.

Detach does not delete the purchase line and does not restore an earlier estimate. For example, a linked actual Cost of -520 remains planned at -520 after detaching, even if its original estimate was -500.

Detach and reassignment are separate actions. Until you complete the new assignment, the Cost rows are planned and unlinked. Direct detach cannot change posted history, unresolved legacy links, or Freight Order allocations.

## Assign a carrier invoice through a Freight Order

Use this route when the purchase cost should first appear in **Freight Order Charges** and then be allocated to that Freight Order's related Forwarding Orders.

1. Open the **Freight Order** and check its **Pay-to Vendor No.** and assigned Forwarding Order stages.
2. In **Charges**, choose **Assignment > Purchase Document**.
3. In **Purchase Document Assignment**, select the unallocated invoice lines for this Freight Order. This entry point lists open Purchase Invoices whose Buy-from and Pay-to Vendor both match the Freight Order's Pay-to Vendor.
4. Confirm the selection, then choose the distribution type and participating Forwarding Orders in **Select Allocation**.
5. Confirm and review the new Freight Order charges and the allocated Cost rows in each Forwarding Order's Settlement.

This workflow creates Freight charge records and their Settlement allocations. Do not also import the same purchase line directly into Settlement. If Buy-from and Pay-to Vendor differ, use the direct Settlement workflow appropriate to the cost.

The direct **Delete Allocation / Delete Charge Allocation** procedure does not detach Freight allocations. A linked Freight charge, or a charge with Settlement children, is protected from ordinary deletion. The older **Delete Link To Freight Order** action is also not a supported way to detach this source. If a Freight assignment is wrong, review the complete Freight allocation with the responsible accounting or TMS administrator before making a correction.

## Troubleshooting

| Problem | What to check |
|---|---|
| The invoice is not listed for single-line linking. | It must be unposted and Open, and its Buy-from Vendor must match **Customer/Vendor Name** on the Cost row. Use the action for the correct document type. |
| **Whole Purchase Document** actions are disabled. | Ask an administrator to check your **User Setup** record and **TMS Allow Purchase Document Allocation**. |
| An order is missing from **Select Allocation**. | Prepare an eligible unlinked planned Cost row with matching Buy-from Vendor, Financial Effect, and Charge mapping. Already linked, posted, or unverified rows cannot be selected. |
| TMS asks for **Settlement Line No.** | More than one compatible Cost row exists. Choose the intended row with the lookup. |
| The Charge does not match the purchase line. | Compare the Charge's **Purchase Line - Type / Purchase Line - No.** with the source **Type / No.**. Similar descriptions or equal amounts do not establish a match. |
| Whole-document import reports several matching Charges. | Use a planned Cost row with the intended **Code** and single-line linking, or make the Charge mapping unambiguous. |
| Cost appears twice after import. | Whole-document import creates new rows. Check whether the same cost was already planned; use single-line linking when replacing a plan with an actual source. |
| A source line is already allocated. | Review its existing Settlement or Freight allocation. Do not create another allocation for the same source line. |
| Ordinary costs increase profit. | Check **Financial Effect** and the signs of Quantity, Price, and Amount. Ordinary Per Unit Cost uses Standard with positive quantity and price; a fixed ordinary Cost uses a negative Amount. |
| Allocation fails with a zero share. | Check selected orders and the values used by Distribution Type. Every selected share must be greater than zero. |
| A linked purchase line cannot be changed or deleted. | Standard price and line-discount changes are supported. For protected changes or deletion, detach the complete eligible allocation first. |
| The document has already been posted. | New assignment after posting is unsupported. Review the financial correction process with the responsible accounting user. |
| Delete Allocation does not apply to the row. | Check whether it is posted, linked through Freight Charges, or has unresolved legacy data. Use the source workflow rather than forcing a direct detach. |

## Related

- [Settlement](settlement.md)
- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Charges](charges.md)
- [Freight Agreements](freightagreement.md)
- [Posted History](postedhistory.md)
