---
title: "Create and Invoice Your First Forwarding Job"
description: "Walk through the first end-to-end LSP flow from Forwarding Order creation to customer invoice and posted history review."
---

# Create and Invoice Your First Forwarding Job

Use this tutorial to prove the basic LSP process in a sandbox.

The goal is to create one Forwarding Order, assign carrier execution, add income and cost, create a customer invoice, and review history.

## Before you start

Make sure that:

- [TMS Setup](setup.md) is complete,
- a Forwarding Order Type exists,
- a Freight Order Type exists,
- one customer and one carrier exist,
- at least one service and one charge exist,
- posting setup is ready in Business Central.

## Step 1. Create the Forwarding Order

1. Open **Forwarding Orders**.
2. Choose **New** or use the [Order Wizard](orderwizards.md).
3. Select the Forwarding Order Type.
4. Fill the ordering party.
5. Fill origin party and consignee details.
6. Add cargo or content details.
7. Review the stages.

Expected result: the Forwarding Order has parties, content, stages, and a status that allows operational work.

## Step 2. Create the Freight Order

1. Select the stage that needs carrier execution.
2. Choose **Assign Freight Order**.
3. Select or confirm the Freight Order Type.
4. Select the carrier.
5. Add vehicle and driver when your process tracks them.
6. Review content and stage lines on the Freight Order.

Expected result: the carrier execution document is linked to the Forwarding Order stage.

## Step 3. Add income and cost

1. Open [Settlement](settlement.md) from the Forwarding Order.
2. Add a line with **Type = Income**, select a service in **Code**, and keep **Financial Effect = Standard** for a normal customer invoice.
3. Set **Pricing Mode = Per Unit**. Fill **Unit of Measure Code**, **Quantity**, and **Price**. For example, quantity `1` and price `1,000` give income of `1,000`. Review the customer and currency. See [Add an Income line](settlement.md#add-an-income-line) for other pricing cases.
4. Add a line with **Type = Cost** and a charge in **Code**. For a normal cost, keep **Financial Effect = Standard** and **Pricing Mode = Per Unit**, then enter quantity and price as they should appear on the purchase document. For example, quantity `1` and price `700` give **Amount = 700** and **P/L Amount = -700** before discounts.
5. If the vendor invoice already exists, use [Purchase Assignment](purchaseassignment.md#link-one-purchase-line-to-an-existing-cost) to link its purchase line to the Cost row from step 4. Do not import it as a second cost.
6. Review currency, quantity, price, discounts, and Line Amount. **Fixed Amount** follows the same document sign rule: enter the amount before line discount, normally `700` for a cost of `700`. Settlement applies the financial direction to P/L automatically. Deliberately negative quantities, prices, and amounts remain available when those are the intended document values. See [Quantity, price, and amount signs](settlement.md#quantity-price-and-amount-signs).

Expected result: settlement shows income and cost contributions. With both example lines in local currency, **Total Profit (LCY)** is `300`.

## Step 4. Create the customer invoice

1. Confirm that the Forwarding Order status allows invoicing.
2. Confirm that every Income line to invoice has **Unit of Measure Code** filled and **Financial Effect = Standard**.
3. In Settlement, choose **Create > Sales Invoice**, or choose **Create Sales Invoice** on the Forwarding Order.
4. Open the created invoice.
5. Review customer, lines, tax, currency, and amount.
6. Post the invoice if your test process allows posting.

Expected result: the settlement line stores the linked sales document reference.

## Step 5. Complete and review history

1. Attach required files if status control requires them.
2. Confirm settlement posting controls are satisfied.
3. Post the Forwarding Order when the job is complete.
4. Open [Posted Forwarding Order](postedforwardingorder.md).
5. Review posted parties, content, stages, settlement, and related documents.

Expected result: the job is available in posted history for audit and reporting.

## Troubleshooting

| Problem | What to check |
|---|---|
| Forwarding Order cannot be created | Check TMS Setup, Forwarding Order Type, number series, and permissions. |
| Freight Order cannot be created | Check stages, status controls, Freight Order Type, and carrier setup. |
| Invoice action is disabled | Check status profile, settlement permissions, customer, and service setup. |
| Invoice creation asks for a unit of measure | Fill **Unit of Measure Code** on every Income line being invoiced, including **Fixed Amount** lines. |
| Posting is blocked | Check required attachments, posted document control, and settlement posting dates. |
| Totals are wrong | Review **Financial Effect**, **Pricing Mode**, and [amount signs](settlement.md#quantity-price-and-amount-signs), then check currency and duplicate or missing costs. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Settlement](settlement.md)
- [Purchase Assignment](purchaseassignment.md)
- [Posted History](postedhistory.md)
