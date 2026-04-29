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
2. Add one income line with a service code.
3. Add one cost line with a charge code or assign a vendor purchase line through [Purchase Assignment](purchaseassignment.md).
4. Review currency, quantity, price, and amount.
5. Recalculate settlement totals if needed.

Expected result: settlement shows income, cost, profit, and margin.

## Step 4. Create the customer invoice

1. Confirm that the Forwarding Order status allows invoicing.
2. Choose **Create Sales Invoice** from settlement or the Forwarding Order action.
3. Open the created invoice.
4. Review customer, lines, tax, currency, and amount.
5. Post the invoice if your test process allows posting.

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
| Posting is blocked | Check required attachments, posted document control, and settlement posting dates. |
| Totals are wrong | Review service, charge, quantity, currency, price, and allocation. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Settlement](settlement.md)
- [Purchase Assignment](purchaseassignment.md)
- [Posted History](postedhistory.md)
