---
title: "TMS Setup"
description: "Configure the core TMS settings for forwarding, settlement, maps, reports, attachments, and role center filters."
---

# TMS Setup

Use **TMS Setup** to define default behavior for the Logistics Service Provider workflow.

This page controls Forwarding Order defaults, Freight Order defaults, finance behavior, price list behavior, map provider settings, role center filters, Azure Blob Storage for attachments, and report defaults.

![TMS Setup page](resources/setup/pics/setup3.png)

## How to open the page

Use one of these options:

- search for **TMS Setup**,
- open it from **Manual Setup**,
- open it from the TMS role center if your role center includes setup actions.

## Minimum setup checklist

Complete these items before users start working:

1. Fill the number series used by TMS.
2. Select the default [Forwarding Order Type](forwardingordertype.md).
3. Create status profiles for Forwarding Orders and Freight Orders.
4. Create stage profiles for the transportation process.
5. Set up [Services](services.md) for customer billing.
6. Set up [Charges](charges.md) for vendor and carrier costs.
7. Configure posting controls for settlement and Forwarding Order posting.
8. Configure [Freight Order Types](freightordertype.md).
9. Configure map provider settings if distance or route calculation is used.
10. Configure [Azure Blob Storage for Attachments](azureblobstorage.md) when external file storage is used.
11. Configure [Transport Profiles](transportprofiles.md) and Emergency Response when your process uses dangerous-goods, temperature, or equipment compatibility controls.
12. Choose **Set default reports** if report selections are missing.

## Setup by process

| If your company uses | Set up these areas |
|---|---|
| Basic forwarding | Forwarding Order Types, number series, statuses, and stages. |
| Carrier execution | Freight Order type, carriers, vehicles, drivers, and stage rules. |
| Dangerous goods and equipment compatibility | [Transport Profiles](transportprofiles.md), Transport Regulations, Modes of Transport, Package Types, Vehicle Unit Types, and Emergency Response. |
| Customer invoicing | Services, service mapping, customer currency, and posting setup. Settlement now chooses Sales Order or Sales Invoice through an explicit action. |
| Carrier cost allocation | Charges, purchase line mapping, settlement controls, and vendor setup. |
| Price automation | Price List Enabled, default price list, services, and price records. |
| Map and route calculation | Map Provider, API key, map locations, and coordinates. |
| Attachment governance | Attachment categories, Azure Blob Storage, and status requirements. |
| Guided order creation | Forwarding Order Types, Freight Order Types, number series, and [Order Wizards](orderwizards.md). |
| Role Center monitoring | Filter captions and values for role center cues. |

## Settings most teams review

| Area | What to review |
|---|---|
| **General** | Global status control and core default behavior. |
| **Instructions** | Enable instruction sections and open the reusable [Instructions and Instruction Types](instructions.md) setup pages. |
| **Control** | Weight, volume, footage, and logistic unit controls. |
| **Finance and Invoicing** | Date behavior, credit limit warnings, posting controls, and optional positive Settlement input. The former Invoice Document Type is retained only for compatibility and is no longer used by the new Settlement actions. |
| **Price List** | Whether pricing is enabled and which price list is used by default. |
| **Forwarding Orders** | Default type and source defaults. |
| **Freight Order** | Default type, unit type, charge distribution precision, execution status profile, and Bill of Lading numbering. |
| **Emergency Response** | Company-level provider, primary/international phones, contact, contract ID, and notes. The bundled Dangerous Goods Bill of Lading prints the provider and phones; contact and contract ID are available to custom layouts. |
| **Map Provider Settings** | Google Maps or Azure Maps, with one masked provider key used by server requests and browser visualization. Review the displayed external-service disclosure before entering a key. |
| **Role Center Settings** | Cue filter captions and values. |
| **Azure Blob Storage** | External storage for TMS attachments. The account key is masked and stored encrypted with company scope. |
| **Reports** | Menu for TMS report selections, report layouts, default layouts, printer selections, and default report initialization. |

## Settlement input signs

Under **Finance and Invoicing > Settlement**, **Make Settlement Input Positive** is **off by default**. Enable it to prevent accidental minus signs when users enter **Quantity**, **Price**, or **Amount** in Settlement.

When enabled, a new negative input is changed to its positive value, for example **−100 → 100**. An informational Message identifies the field and change, explains that TMS calculates the P/L sign automatically and that credit memos use **Financial Effect = Credit**, and points back to this setting. The user can continue with the positive value; this is not a validation error.

If the intended document line really needs a negative quantity, price or amount, turn the setting off temporarily, enter the intended value, and enable it again when appropriate. The setting does not rewrite existing rows or make P/L positive. Linking, synchronization, migration, template generation and copying retain their source/calculated values. Editing another field on a row with an existing negative value does not normalize that saved value.

The same input rule applies to Settlement API writes. Interactive sessions show the Message; API/background sessions receive no popup, so integrations should use the normalized values returned by the API. See [Settlement document values and signs](settlement.md#quantity-price-and-amount-signs).

## Default Forwarding Order Type by customer, vendor, and address

Use **Default FWO Type** on the **Customer Card**, **Vendor Card**, **Ship-to Address**, or **Order Address** card to suggest a transportation process on sales and purchase documents.

The document stores that suggestion in **FWO Type**, in its **Transportation Management System** section. You can change **FWO Type** manually before creating the Forwarding Order, including on a released Sales Order. Once a Forwarding Order is linked, the field becomes read-only.

| When selecting or changing | Suggested FWO Type |
|---|---|
| Sell-to Customer or Ship-to Code on a sales document | Selected Ship-to Address default → Sell-to Customer default → blank. |
| Buy-from Vendor or Order Address Code on a purchase document | Selected Order Address default → Buy-from Vendor default → blank. |

For example, set a domestic type on a customer and a multimodal type on its overseas Ship-to Address. Selecting that address fills the sales document's **FWO Type** with the multimodal type. For a particular shipment, you can choose a different type directly on the document.

Before an FWO is linked, changing the customer, vendor, or selected address refreshes the suggestion. Clearing the address uses the customer or vendor default. If neither default is set, the document field becomes blank. Unrelated edits and revalidating an unchanged customer/address combination preserve your manual value. Changing a default on a master-data card does not update existing documents.

When creating the Forwarding Order, TMS uses **FWO Type** stored on the source document. It does not read customer or address defaults again. If the document field is blank, the existing behavior remains:

- Standard creation from a Sales Order or Sales Return Order uses **Default Sales FWO Type**. If that setting is blank, an interactive session opens the type selection; cancelling stops creation. Without an interactive session, the setup default is required.
- Creation from purchase documents, posted sales or purchase documents, and combined shipments requires the corresponding default in TMS Setup.
- For sales **Create FWO as LSP**, **Select FWO Type for LSP** controls selection when the document's **FWO Type** is blank. A populated document field takes priority.

The stored type is copied to posted and archived sales and purchase headers. Existing documents with a blank field keep the same fallback behavior until you set **FWO Type** or change their customer, vendor, or address. While an FWO is linked, manual changes to **FWO Type** are blocked and customer/address changes keep its saved value. The lock also applies when the linked FWO has been posted.

See [Business Central Integration](integration.md#default-fwo-type-from-source-documents) for supported sources, inventory documents, and returns.

## Bill of Lading numbering

Open **TMS Setup > Freight Order > Bill of Lading Numbering**. This group contains three fields:

| Field | Use it for |
|---|---|
| **Numbering Method** | Leave blank for no automatic numbering, choose **Prefix + Freight Order No.**, or choose **No. Series**. |
| **Prefix** | Enter the text to place directly before the Freight Order number. Editable only for **Prefix + Freight Order No.**. |
| **No. Series** | Select an independent Bill of Lading number series. Editable only for **No. Series**. |

Switching methods retains the saved Prefix and No. Series values. Selecting a method does not require you to have already filled its series; the required settings are checked when a Freight Order is created.

With **Prefix + Freight Order No.**, the prefix is joined literally to the **Freight Order No.** Include any separator in the prefix. For example, **BOL-** plus **FO-00001** produces **BOL-FO-00001**. A blank prefix uses the Freight Order number alone. The complete result must fit the existing 20-character Bill of Lading No. field; a longer result causes a clear error and is never truncated.

With **No. Series**, select a series with **Default Nos.** enabled. Business Central takes the next number from the series line valid for the Freight Order's **Document Date**, or the work date if Document Date is blank. For example, a configured series can produce **BOL00001**. A missing series, disabled Default Nos., or an unusable date-specific series line prevents creation with a clear error.

Automatic numbering runs only when a Freight Order is created, after its own number exists. This covers creation from the card, wizards, and the API, including API requests that supply an explicit Freight Order number. An already populated Bill of Lading No., including a manually supplied number, is preserved. Temporary records do not consume Bill of Lading sequence numbers.

Changing setup does not fill or renumber existing orders. Editing or renaming an order and printing reports do not generate or recalculate its Bill of Lading No. Reports continue to read the stored value, and posting copies that value to the Posted Freight Order.

## Useful actions

Use the **Reports** menu on **TMS Setup** to configure printed forms. **Report Selection - Transportation Management System** opens the existing TMS document/report mapping and email settings. **Report Layouts** opens the current Business Central layout-management page, where you can import or update user-defined templates. **Report Layout Selection** sets a report's default layout for a company, and **Printer Selections** configures report/user printer assignments. These standard layout and printer pages can include reports from other installed extensions as well as TMS.

**Set default reports** is also in this menu. It adds missing built-in report rows without replacing configured Report IDs or layouts. In the new Packing List and Bill of Lading usages it creates three Sequence rows; standard BC prints every remaining row. Delete unwanted alternatives to use a single form. See [Report selections by document usage](reports.md#report-selections-by-document-usage). Opening a setup page does not initialize or migrate selections.

| Action | Use it for |
|---|---|
| **Reports > Report Selection - Transportation Management System** | Select reports and email settings for TMS document usages. |
| **Reports > Report Layouts** | Manage built-in and user-defined report layouts. |
| **Reports > Report Layout Selection** | Choose default report layouts for a company. |
| **Reports > Printer Selections** | Assign printers to reports and users. |
| **Reports > Set default reports** | Add missing default report selections and grouped alternatives without overwriting existing target rows. |
| **Check Connection** | Test Azure Blob Storage connection. |
| **Transfer Attachments to ABS** | Move existing TMS attachments to Azure Blob Storage. |
| **Free Space** | Remove imported file payload after transfer to Azure Blob Storage. |
| **Assisted Setup** | Open the TMS wizard, configure a map provider, and reconcile MAP Locations for customers, vendors, ship-to addresses, order addresses, and locations. |
| **Settlement Amount Model Upgrade** | Preview and manually convert existing Settlement on active and posted Forwarding Orders to document amounts, discounts, and P/L amounts. |

Google Maps and Azure Maps receive address, coordinate, route, and travel-mode data when map operations run. Review [privacy and outbound data transfer](mapproviders.md#privacy-and-outbound-data-transfer), then record the organization-level decision on the standard Business Central **Privacy Notices Status** page when required by company policy.

## Upgrade existing Settlement amounts

The new Settlement model separates document values from their financial contribution. **Amount** is before line discount, **Line Amount** is after line discount, and **P/L Amount (LCY)** is the signed value used for profitability. **P/L Amount (ACY)** is calculated from LCY using the additional reporting currency configured in TMS Setup. When the currency is blank, P/L Amount (ACY) equals P/L Amount (LCY). This rule applies both during conversion and in normal operation.

Source-backed conversion uses the same document snapshot and amount calculation functions as linking and synchronization. The common calculation applies the blank-currency ACY rule; there is no separate ACY command or additional wizard step. The wizard's selection and repeat-processing rules remain unchanged.

Installing or upgrading the extension does **not** automatically convert existing rows to this amount model. New rows use the canonical fields immediately. The current amount-model version is **2**; versions **0** and **1** are converted by this wizard. Existing rows from either earlier amount-model version, including rows already converted to the first document/P/L model, are marked **Settlement Upgrade Required** and need controlled conversion; their new fields and profitability totals are incomplete until conversion. The wizard is available to **TMS Administrator**.

1. Open **TMS Setup** and choose **Data Migration > Settlement Amount Model Upgrade**, or search for **Settlement Amount Model Upgrade**.
2. Select **Active Forwarding Orders**, **Posted Forwarding Orders**, or **Active and Posted Forwarding Orders**. Posted scope includes saved versions.
3. Enter a **Forwarding Order No. Filter**, for example `FWO001..FWO100`, or leave it blank for the selected scope.
4. Choose **Preview**. This reads Settlement and its financial source documents without changing business data.
5. Review **Rows to Convert**, active and posted counts, **Related Rows Outside Selection**, **Already Converted Rows**, **Unresolved Rows**, and the previous/new LCY financial totals. Choose **Preview Details** for each row and diagnostic.
6. Resolve rows marked **Ready = No**, or return with **Back** to narrow the filter. One unresolved row blocks the whole run; no partial conversion is saved. An informational diagnostic on a Ready row does not block conversion.
7. Choose **Convert** and confirm the displayed number of rows, including related rows outside the filter. The wizard rechecks the preview before writing. If source documents, rates, setup, or Settlement have changed, preview again.
8. Review **Converted Rows**, choose **Finish**, and verify the affected orders. Run the wizard again for another selection as needed.

Both **Preview** and **Convert** show a progress window immediately. It displays the operation, current stage, FWO or source document, processed row count, a text progress bar and percentage, elapsed time, and an approximate remaining time for the current stage. The estimate appears after enough rows have completed; it is not an estimate for the entire wizard. While the wizard discovers related rows or obtains locks, the total or remaining time can be unavailable. The stage name shows what is being prepared. A database lock wait can pause updates until that database call returns.

The window updates periodically, including inside large source groups. Cancelling interrupts the run; conversion remains one transaction, so partial converted rows are not saved. Preview again before restarting an interrupted conversion.

A shared source line that still exists is converted as a complete allocation group. This can include active orders, posted orders, and historical versions outside the selected filter. The preview lists these additions. Consistent historical copies of the same FWO and Settlement line count as one share; different lines in the same FWO remain separate shares. The final share receives rounding remainders so source totals are preserved.

When saved allocation factors are missing or invalid, the wizard first tries to recover them from stored quantities, then from financial amounts in the source document currency. It checks the totals against the source and rejects zero or incompatible-sign weights. It does not divide arbitrary quantities by their sum: several copies of the full document quantity or amount are not evidence of equal shares. LCY amounts are not used as foreign-currency weights. Preview explains whether quantities or amounts were used, and recovered shares pass through the same document calculation as ordinary linking.

If the allocation cannot be recovered and **every FWO in the group is posted**, the wizard converts each physical historical row from its own saved financial amounts without redistribution or current exchange-rate conversion. References remain, and the source SystemIds are refreshed. Each version keeps its saved financial contribution, even when different historical versions disagree. Preview shows an informational diagnostic. A group containing active FWOs still requires a reliable allocation and remains unresolved when none can be established. Already converted rows remain skipped.

Linked model-0 rows with an available source are refreshed from their actual open or posted financial document through the same source-reading mechanism used by Settlement. Source values take precedence over the old plan. Model-1 rows transfer their saved values to the canonical fields without revaluing their financial result. For both versions, conversion resolves the source by Settlement side, document type, document number and line number, and replaces Source Header SystemId and Source Line SystemId with the identifiers in the current company. Old identifiers from configuration-package imports do not block conversion. The identifiers found during Preview are rechecked under locks before saving; if the source changes after Preview, preview again. Rows with no current document reference are converted from their saved financial values without prior verification; previous LCY contributions are preserved. Ambiguous allocations involving active FWOs still require review. The preview totals include every selected historical version, so they are conversion checks rather than a consolidated financial report.

When Document Type is blank, Document No. is empty and Document Line No. is zero, the wizard converts both active and posted Settlement regardless of Needs Review, Legacy or other stored technical statuses. It uses the existing stored-data amount conversion and preserves the financial result and metadata. Old GUIDs, previous-reference fields, quantity-mode flags or generation metadata do not require manual cleanup before this conversion. No source document or current exchange rate is needed. Needs Review is retained rather than relabelled Verified; it no longer blocks this amount-model conversion. Preview displays an informational note. This rule does not treat a specified but missing document as an empty reference.

For these rows, conversion also aligns Quantity, Price and Pricing Mode with the new document Amount. For example, a legacy Cost / Standard with Quantity 1, Price -100 and financial Amount -100 becomes Quantity 1, Price 100, Amount 100 and P/L Amount -100. Existing unit values that already match the new Amount are retained, including deliberate negative inputs. The old economic price sign is reversed only when its arithmetic supports that interpretation. A technical negative quantity can be normalized for an independent migrated Cost / Standard with that same evidence; quantities owned by Freight, price lists, corrections or generation are not reinterpreted. Model-1 signs are not inverted again.

An original Per Unit mode is retained when the converted quantity and price reproduce the new gross Amount. Otherwise the amount remains authoritative in Fixed Amount mode, with Price = Amount / Quantity; if Quantity is zero, Price carries Amount. Existing Fixed Amount rows stay fixed. Saved financial results and currency values are preserved. Available currency rounding precision may be used to verify the unit product, but a missing historical currency does not block conversion: without a provable Per Unit calculation, the row uses Fixed Amount. Already converted model-2 rows remain skipped.

For rows that still need conversion, a missing source header or exact line is handled according to the **FWO**, not whether the financial source itself was posted:

- **Active FWO:** if the referenced document header or exact line no longer exists and deletion was not recorded, conversion removes the missing ERP link and returns the Settlement to an unlinked plan using its saved financial amounts. Preview explains that the link will be removed. The wizard clears the document and previous-document references, source SystemIds and posted-link state, and preserves P/L. Independent Freight, correction and generated ownership is retained.
- **Posted FWO:** absence alone is not an error. The wizard preserves the historical document type/number/line, source IDs, stored status and financial result. It does not detach the reference or replace it with another document. Each historical version uses its own saved values; missing-source rows do not expand or recalculate an allocation group.

**Posted Source Deleted** takes precedence for both active and posted FWOs. This flag records an observed deletion of a posted invoice/credit memo or linked line. The wizard preserves that history using saved amounts, including its reference and flag, without trying to relink a new document that later reuses the same number. It does not automatically infer this flag for documents that disappeared before the deletion tracking was installed.

The historical missing-source conversion also applies to posted FWO rows with an incomplete, but not entirely empty, source reference, for example an empty document type/number with a remaining document line number and Link State = Posted. The wizard preserves their status, quantity, price, remaining reference fields, currency context and financial result. Preview shows an informational diagnostic and the row can be Ready. This exception is limited to posted FWOs; an entirely empty current reference follows the unit-value alignment described above.

If the document type is available, it determines Standard/Credit. Otherwise the converter uses a saved direction factor of +1 or -1, or the Settlement Type and saved Financial Effect when no valid factor exists. It never infers Credit merely from the sign of an amount. Model-0 signed amounts become P/L; model-1 P/L is retained. Consistent model-1 document amounts and discounts are preserved. When the available detail cannot support a consistent breakdown, the converter uses a Fixed Amount net representation derived from the saved P/L. It does not reconstruct unknown discounts or VAT, or revalue historical LCY using current exchange rates. Blank reporting currency still means ACY = LCY.

For the oldest archived rows whose referenced source is unavailable, the saved signed Amount and Amount (LCY) become P/L Amount and P/L Amount (LCY). When an additional reporting currency is configured, the saved Additional-Currency Amount becomes P/L Amount (ACY); otherwise P/L Amount (ACY) copies P/L Amount (LCY). The canonical amount uses a **Fixed Amount** net representation with no invented line or invoice discounts. Quantity, price and currency context are retained. Gross, discounts and unavailable VAT facts from the deleted document cannot be reconstructed; the Preview diagnostic explains this. Existing legacy Source fields remain stored. Model-1 history retains its existing document amounts and P/L, subject to consistency of its saved snapshot and the same blank-currency ACY rule. Neither path recalculates exchange rates. These are informational notes on Ready rows, not missing-source errors.

Source availability is rechecked before saving using the original document reference, including when the proposed active row has already been prepared with an empty reference. If a previously missing source reappears or an existing source disappears after Preview, preview the selection again. This repair applies only to the conversion wizard. Already converted model-2 rows are still skipped; the wizard is not a general link-repair operation.

Preview also checks that each Settlement row belongs to an existing active FWO or an existing posted FWO version. Rows with a missing FWO owner remain visible in Preview Details with **Skipped (Missing FWO)** and a diagnostic identifying the preview entry, original Settlement line number, and original Settlement SystemId. **Skipped Rows (Missing FWO)** counts them separately. They do not block conversion and are excluded from Rows to Convert, Unresolved Rows, Already Converted Rows, and the financial totals. Convert leaves these rows unchanged, including their amount-model version and document references. Posted version 0 is accepted if that FWO version exists.

When a shared source-document group includes a skipped FWO share, the converter retains reliable shares of the remaining rows without increasing them to cover the skipped share. A partial group receives no remaining balance from the full source document. Missing allocation evidence for a real FWO still follows the existing unresolved-allocation rules. A missing FWO owner is separate from a missing sales or purchase document; the source-document conversion rules above still apply.

All rows in one run are saved together. Already converted rows are checked and skipped, making reruns safe. The wizard changes only the permitted Settlement amount and source-snapshot fields; it does not create, post, or reverse Business Central financial documents. Posted Settlement remains read-only outside this controlled conversion and the existing reporting-currency process.

## Verify setup

1. Search for **Forwarding Orders** and confirm the page opens.
2. Create a test Forwarding Order.
3. Confirm a Forwarding Order Type can be selected.
4. Confirm stages and statuses appear as expected.
5. Add one settlement income line and one cost line.
6. Confirm the expected invoice and charge actions are available.
7. Create a Freight Order from a stage.
8. Run the [first forwarding job tutorial](first-forwarding-job.md) in a sandbox.

## Troubleshooting

| Problem | What to check |
|---|---|
| Forwarding Order cannot be created | Check default Forwarding Order Type, number series, and permissions. |
| Freight Order action is disabled | Check the Forwarding Order status and status profile permissions. |
| A Transport Profile is not suggested or reviewed | Check [Transport Profiles](transportprofiles.md), cargo assignments, Mode/Regulation, required classifications, and permissions. |
| Settlement cannot create an invoice | Check service mapping, invoice document type, customer, and status permissions. |
| Posting is blocked | Review posted document control, settlement document posting, and FWO posting date control. |
| Attachments do not move to Azure Blob Storage | Check account name, container, account key, and **Check Connection** result. |
| Upgrade cannot migrate the Azure Blob Storage key | Rotate or re-enter a supported account key, retry the upgrade, and run **Check Connection** afterward. The old plaintext value is not cleared when migration fails. |

## Related

- [Forwarding Order Types](forwardingordertype.md)
- [Freight Order Types](freightordertype.md)
- [Statuses and Status Profiles](statuses.md)
- [Stages](stages.md)
- [Settlement](settlement.md)
- [Azure Blob Storage for Attachments](azureblobstorage.md)
- [Map Providers](mapproviders.md)
- [Google Maps Integration](googlemapintegration.md)
- [Attachment Control](attachmentcontrol.md)
- [Transport Profiles and Dangerous Goods](transportprofiles.md)
- [Instructions](instructions.md)
