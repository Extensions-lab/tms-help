---
title: "Posted Forwarding Order"
description: "Review read-only history after a Forwarding Order has been posted."
---

# Posted Forwarding Order

Use **Posted Forwarding Orders** to review completed transportation jobs.

The list opens with the most recently posted records first, using the date and time each posted version was saved. This order also applies within the same day and when an order is posted with an earlier **Posting Date**. A saved view or a manually selected column sort can use a different order.

Posted records preserve operational, document, and financial history after the active Forwarding Order is posted.

The **Financial Correction** column identifies corrective orders in the list and can be filtered to show them. It uses the flag saved with each posted order; later changes to the Forwarding Order Type do not change this historical value.

## Before you start

Make sure that:

- the Forwarding Order has been posted,
- the user has permission to open posted TMS records,
- related sales or purchase posted documents are available if you need financial detail.

## What you can review

| Area | Use it for |
|---|---|
| **General** | Posted order number, original references, dates, status, and parties. |
| **Content** | Cargo, quantities, unit types, and logistics details. |
| **Stages** | Historical transportation legs linked to the Freight Orders posted before this job. |
| **Settlement** | Posted income, cost, allocation, and margin information. |
| **Attachments** | Files stored with or linked to the posted record. |
| **Related documents** | Posted customer invoices, credit memos, and vendor documents when linked. |

## How to work in this page

1. Search for **Posted Forwarding Orders**.
2. Open the posted order.
3. Review general information and references.
4. Open content, stage, settlement, or attachment details as needed.
5. Use related document actions to trace invoices or credit memos.
6. Print historical documents when your process allows it.

## Related Freight Order history

Every Freight Order assigned to the job's stages is posted before the Forwarding Order, after its linked purchases are posted and its charges are fully allocated. The stage links identify the exact Posted Freight Order for each completed leg. Posting the customer job completes its historical links without creating another trip archive or rewriting the trip's saved operational and financial facts.

A Posted Freight Order can be shared by several Forwarding Orders and contains the complete trip's stages, content, charges, instructions, and Freight Order attachments. Open that linked history to review shared carrier documents. The **Attachments** on the Posted Forwarding Order contain files that belonged directly to this customer job and moved when it was posted.

## Invoicing Detail

The **Invoicing Detail** FactBox shows the order amount and profit for the selected posted version, together with current invoicing and payment information linked to the order. The invoice and payment information is not a snapshot of the posting date.

Invoice and payment totals use the company's local currency (LCY), so invoices in different currencies can be added together.

| Field | What it shows |
|---|---|
| **Invoiced Amount (LCY)** | The full amounts of posted customer documents linked to the order, in LCY. |
| **Paid Amount (LCY)** | The settled portion of those amounts in LCY, including payments, applied credit memos, and payment discounts. |
| **Currency Code** | The company's local currency code from General Ledger Setup. |

These totals use the LCY amounts recorded in the customer ledger, without converting documents at today's exchange rate. They show full linked document amounts, which can differ from the share allocated to this order in **Settlement**.

Check **Calculation Status** before using the displayed amounts:

- **Calculating...** means the values are being refreshed.
- **Calculated** means the calculation completed, including when the amounts are zero.
- **Unavailable. Choose Update to retry.** means no complete result is available. Choose **Update** to try again and review any error notification.

Changing the order or its posted version clears the previous values and starts a new calculation.

## Profit percentage

When **Profit Calculation Enabled** is on in **TMS Setup**, the posted list shows **Profit % (LCY)** and **Profit % (ACY)**. Each percentage uses the Settlement totals of that exact **Posted Version** and the current **Profit Calculation Method** in Setup. It is an analysis of that version using today's selected method, rather than a percentage fixed at the posting date.

Choose **Refresh Profit Calculation** to reread Setup and refresh the displayed percentages. Opening or refreshing the list does not save new percentages or change posted income, cost, or profit amounts. Previously stored percentages remain available in the posted record, but they are not the percentages displayed in this list.

LCY and ACY are calculated separately, using the absolute income or cost base selected by the method. The result is rounded to a whole percentage point; a zero base gives 0%. For example, a posted version with income of 1,000, cost of 800, and profit of 200 shows 25% with **Profit/Cost * 100%**, or 20% with **Profit/Income * 100%**. Changing the method changes the displayed percentage, while that version's financial amounts stay the same.

The displayed percentages cannot be sorted or filtered using the normal list commands. Remove any saved **Profit % (LCY)** or **Profit % (ACY)** filters from older views before comparing the refreshed results: those filters can still restrict the list using the previously stored percentages, rather than the values now displayed. Use the income, cost, and profit amount columns to review and filter the historical results.

## Good to know

- Posted Forwarding Orders are read-only history.
- Corrections normally happen through credit memos, new documents, or controlled adjustment processes.
- Keep posted history available for audit, customer service, and profitability reporting.

## Troubleshooting

| Problem | What to check |
|---|---|
| Posted order is not visible | Check filters, posting result, and user permissions. |
| Linked invoice is missing | Review settlement posting and related document references. |
| Attachment is not available | Check storage setup and user permission to open attachments. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Posted History](postedhistory.md)
- [Settlement](settlement.md)
- [Reports and Documents](reports.md)
