---
title: "TMS Setup"
description: "Understand every setting shown on TMS Setup, with practical examples for orders, finance, planning, maps, and attachments."
---

# TMS Setup

Use **TMS Setup** to configure how TMS works in the current company. **FWO** means Forwarding Order; **FO** means Freight Order.

This guide follows the sections and field names shown in the **TMS Setup** window. It covers the visible settings, including fields that become available when you select a particular option, and the actions on this page.

Use an account with permission to maintain TMS Setup. Select the intended company before changing values. The tables below explain which settings supply defaults for new documents and which affect checks or calculations on existing documents.

## How to open the page

Use one of these options:

- search for **TMS Setup**,
- open it from **Manual Setup**,
- open it from the TMS role center if your role center includes setup actions.

## Read setup through the API

Integrations can read the company's existing settings through the read-only
`tmssetups` API. It returns the canonical setup record and its modification time,
with business descriptions for the exposed fields. Credentials, obsolete fields
and wizard media are excluded. GET does not create missing setup or execute any
configuration changes; POST, PATCH and DELETE are disabled. See
[Reading TMS Setup](api.md#reading-tms-setup) for request examples and access rules.

## Minimum setup checklist

Complete these items before users start working:

1. Fill **Number Series > MAP Location Nos.** and the number series on the Forwarding Order Types and Freight Order Types you use.
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

<a id="settings-most-teams-review"></a>

## Settings shown on TMS Setup

| Section in the window | What it controls |
|---|---|
| [General](#general) | Status checks and instructions. |
| [Control](#control) | Weight, volume, and footage used in planning. |
| [Finance and Invoicing](#finance-and-invoicing) | Dates, credit checks, posting rules, price lists, rounding, currencies, Settlement, and profit. |
| [Forwarding Orders](#forwarding-orders) | Default order types, source-specific type selection, corrections, and chargeable weight. |
| [Freight Order](#freight-order) | Default type, charge editing, equipment, allocation, execution statuses, and Bill of Lading numbering. |
| [Number Series](#number-series) | Automatic numbering of MAP Locations. |
| [Map Provider Settings](#map-provider-settings) | Map provider, credentials, and Azure geographic scope. |
| [Role Center Settings](#role-center-settings) | Captions and status filters for Forwarding Order and Freight Order tiles. |
| [Azure Blob Storage for Attachments](#azure-blob-storage-for-attachments) | Attachment storage destination and access. |
| [Emergency Response](#emergency-response) | Emergency contacts used with dangerous-goods documents. |

For report setup, instructions, AI settings, and maintenance tools, see [Useful actions](#useful-actions).

## General

| Field | How to use it |
|---|---|
| **Status Control** | Turn on to enforce the action permissions and extended checks configured for an order's status. Its Status Profile must also have **Action Control Enabled** selected. Turn off to stop these status-based restrictions; normal document validation and user permissions still apply. See [Statuses and Status Profiles](statuses.md). |
| **Instructions Enabled** | Turn on to show instruction sections and allow users to open instruction cards and editors on supported pages. Turn off to hide these sections. Saved instructions remain, and reports can still print them. Administrators can prepare **Instructions** and **Instruction Types** while the feature is off. Reopen affected pages after changing this setting. See [Instructions](instructions.md#enable-instructions). |

## Control

Select the measurements your planners use to reserve Freight Order capacity. These switches control the corresponding planning columns and reservation fields, and enable the matching capacity checks during Freight Order creation and assignment.

| Field | How to use it |
|---|---|
| **Weight** | Turn on to plan and reserve gross weight. For a capacity restriction, also enable **Limit Filling Weight Control** and set a positive **Limit Filling Weight** on the equipment type. TMS checks the cargo or reservation against that limit. |
| **Volume** | Turn on to plan and reserve volume. For a capacity restriction, also enable **Limit Filling Volume Control** and set a positive **Limit Filling Volume** on the equipment type. |
| **Footage** | Turn on to plan and reserve loading length, using **Estimated Footage** and reserved footage. For a capacity restriction, also enable **Limit Filling Footage Control** and set positive **Footage** on the equipment type. |

Turning a control off hides its related planning fields and skips that capacity check in these workflows. Enable at least one of **Weight**, **Volume**, or **Footage** before using the **Freight Order Assignment Wizard**. For example, enable Weight and Volume when both mass and space limit your loads. Set the matching limits, units of measure, and control switches on the [equipment type](logisticunittype.md) as well. When assigning to an existing Freight Order, the final check takes its already reserved capacity into account.

## Finance and Invoicing

Use these settings to choose document dates, check customer credit, control order posting, and calculate prices and profit percentages.

### Invoicing

| Field | How to use it |
|---|---|
| **Invoicing Today** | Turn on to use today's calendar date as **Document Date** on new Sales Orders, Sales Invoices, and Sales Credit Memos created from Settlement. Turn off to copy the Forwarding Order's **Document Date**. **Posting Date** is copied from the FWO when filled; otherwise the new sales document keeps its standard default. For a combined Sales Invoice, the first processed FWO supplies the document date when this is off. Changing the setting does not update existing documents. |
| **Credit Limit Warnings** | Turn on to request the standard Business Central credit warnings when selecting the FWO's customer and creating a Sales Order or Sales Invoice from Settlement. The messages depend on **Sales & Receivables Setup > Credit Warnings**, the customer's credit information, and the user's notification settings. Turn off to skip these additional TMS warning checks. Use warnings when users should review credit exposure before continuing; use **Credit Limit Control** when TMS must block the operation. |
| **Credit Limit Control** | Turn on to block selection of the FWO's customer, or creation of a Sales Order or Sales Invoice from its Settlement, when that customer's **Balance (LCY)** is greater than or equal to a positive **Credit Limit (LCY)**. For example, a balance of 5,000 against a limit of 5,000 is blocked. A zero credit limit is not checked by this control. The comparison uses the existing customer balance; it does not add the planned FWO income. Turn off to skip this TMS balance check. Customer blocking and normal Business Central checks still apply. |

The two credit settings are independent. Standard warnings follow Business Central's billing setup, which can use a separate **Bill-to Customer**. **Credit Limit Control** checks the customer selected as the FWO's **Ordering Party**. Changing these settings does not rewrite existing orders; the checks apply when users next select a customer or create the relevant sales document.

### Post

These controls apply when users post an order, including an order created before the setting changed. Turning on a control does not post or change existing documents.

| Field | How to use it |
|---|---|
| **Posted Documents Control** | Turn on when an FWO must remain open until all its Settlement Income and Cost lines are marked **Posted**. An unposted planned line can therefore prevent posting the FWO. Turn off to omit this general check. Financial corrections created from document corrections still require their managed financial sources to be posted and their links complete. See [invoice corrections](invoice-corrections.md). |
| **FWO Posting Date Control** | Turn on to require the FWO's **Posting Date** to be on or after every Settlement line's **Document Posting Date**. Equal dates are allowed. For example, a financial document posted on 15 November prevents posting the FWO with 14 November; 15 November satisfies this date comparison. Turn off to omit this comparison. Use it to keep the FWO's closing date consistent with its financial documents. |
| **Allow Posting Range Control** | Turn on to require the **Posting Date** of both Forwarding Orders and Freight Orders to fall within **General Ledger Setup > Allow Posting From / Allow Posting To**. Each blank boundary leaves that end of the range unrestricted. For example, a November range prevents posting an order with an October date. Turn off to omit this TMS order-date check; standard financial-document posting rules remain separate. |

### Price List

| Field | How to use it |
|---|---|
| **Price List Enabled** | Turn on when users should generate Settlement Income from configured price lists. This makes **Get Prices** and **Show Price List** available in Settlement and enables the price-list calculation. Reopen Settlement after changing the setting. Turn off when the company enters prices manually or uses other supported pricing processes. Turning it off preserves existing Settlement lines. See [Pricing](pricing.md) for the required services and pricing rules. |
| **Default Price List** | Select the price list to use when the FWO's customer has no price list assigned. A price list on the customer takes precedence. For example, use a company-wide standard list here and assign a negotiated list to a particular customer. **Get Prices** requires a usable list from one of these sources. Changing this default does not reprice existing Settlement automatically; review the order and run **Get Prices** when recalculation is required. |

### Additional

| Field | How to use it |
|---|---|
| **Amount Rounding Precision** | This field remains visible, but the current LSP Settlement and sales-document creation workflows do not use it. Changing it does not change their rounding. Keep the existing value, normally **0.01**. To investigate amount rounding, review **General Ledger Setup** for local currency and the relevant **Currency** card for foreign currency; converted unit prices also use the currency's **Unit-Amount Rounding Precision**. |
| **Additional Reporting Currency** | Select a currency when users need TMS income, cost, and profit reporting in another currency. **P/L Amount (ACY)** converts **P/L Amount (LCY)** to that currency, and the ACY column becomes visible in Settlement. Leaving the setting blank makes the stored ACY amounts equal LCY. After confirmation, changing or clearing the setting recalculates reporting amounts in existing active and posted Settlement. Prepare exchange rates for the affected dates before selecting a currency. Transaction-currency and LCY amounts stay unchanged. See [Settlement reporting amounts](settlement.md#how-profit-is-calculated) for rounding examples. |

### Settlement

| Field | How to use it |
|---|---|
| **Sales Document Linking Mode** | Choose which status an existing unposted sales document must have before users import or link its lines to Settlement. The choices are **Open and Released**, **Open Only**, and **Released Only**. For example, choose **Released Only** when sales documents must complete release before import. See [Source document linking status](#source-document-linking-status) for exclusions, affected actions, and the effect on existing links. |
| **Purch. Document Linking Mode** | Choose the permitted status for new links to existing unposted purchase documents, including purchase-line linking and allocation. It is independent of the sales setting and offers the same three choices. For example, allow **Open and Released** if users allocate costs before purchase invoices are released. See [Source document linking status](#source-document-linking-status). |
| **Make Settlement Input Positive** | Turn on to convert newly entered negative **Quantity**, **Price**, and **Amount** values to positive values, with an explanation. Use it when users accidentally enter minus signs for costs or credits. Turn off when a document line deliberately needs a negative value. It is off by default and preserves existing rows and imported source values. See [Settlement input signs](#settlement-input-signs) for examples and API behavior. |

### Profit Calculation

| Field | How to use it |
|---|---|
| **Profit Calculation Enabled** | Turn on to show **Profit % (LCY)** and **Profit % (ACY)** and their calculation actions in Forwarding Order lists. For active orders, choose **Profit Calculation** to calculate and save the percentages for the current filtered list. Posted orders show percentages calculated from the totals of each posted version. Turn off to hide these percentages and actions; income, cost, and profit amounts remain available. Reopen the list after changing this setting. |
| **Profit Calculation Method** | Choose **Profit/Cost * 100%** to measure profit against cost, or **Profit/Income * 100%** to measure margin against income. Both use the absolute value of the denominator. For income of 1,000 and cost of 800, profit is 200: the first method gives **25%**, the second **20%**. Results are rounded to whole percentage points; a zero denominator gives **0%**. After changing the method, run **Profit Calculation** again for active orders and **Refresh Profit Calculation** in the posted list. |

See [Forwarding Order profit percentage](forwardingorder.md#profit-percentage) for recalculation scope and [Posted Forwarding Order profit percentage](postedforwardingorder.md#profit-percentage) for how the current method applies to posted versions.

## Forwarding Orders

Choose [Forwarding Order Types](forwardingordertype.md) with the numbering, stages, and status rules your process requires. Defaults apply when the relevant creation process needs them; changing a setup default does not change the type on existing orders.

| Field | How to use it |
|---|---|
| **Def. Forwarding Order Type** | Select the type initially suggested in **New Forwarding Order Wizard**. This type also supplies the number-series rules if a manual FWO number is entered before a type is selected. Source-document creation uses its own type selection and the sales, purchase, or transfer defaults below. |
| **Default FWO Type for Sales** | Select the fallback type for creation from sales sources when the source document's **FWO Type** is blank. Standard creation from a Sales Order or Sales Return Order can ask an interactive user to choose a type if this default is blank; other sales-source paths can require the configured default. See [source default priority](#default-forwarding-order-type-by-customer-vendor-and-address). |
| **Select FWO Type for LSP** | Applies to sales **Create FWO as LSP** when the source document's **FWO Type** is blank. Turn on to open the type selection; cancelling stops creation. Turn off to use **Default FWO Type for Sales**, which must then be filled. A type already saved on the source document takes priority in both cases. |
| **Default FWO Type for Purchases** | Select the fallback type for creation from a Purchase Order, Purchase Return Order, Posted Purchase Receipt, or Posted Return Shipment when the source's **FWO Type** is blank. Configure this default before using those creation paths. See [Business Central Integration](integration.md#default-fwo-type-from-source-documents). |
| **Default FWO Type for Transfers** | Select the type used when creating an FWO from a Transfer Order, Posted Transfer Shipment, or Posted Transfer Receipt. These creation paths require this setup value. |
| **Correction FWO Type Code** | Select a type with **Financial Correction** enabled for automatic corrective FWOs created from financial-document corrections involving posted orders. Configure its number series and appropriate status permissions. See [Corrective Forwarding Orders](#corrective-forwarding-orders). |
| **Chargeable Weight Precision** | Set the rounding increment for calculated chargeable weight in the base weight unit. TMS takes the greater of the line's gross weight and volume weight, then rounds it to this increment. The initial value is **1**; a saved zero also uses **1** when calculating. For example, a calculated base weight of **180.26** becomes **180** with precision **1**, or **180.26** with precision **0.01**. |

**Chargeable Weight Precision** affects subsequent chargeable-weight calculations and the rounded chargeable-weight total used for output. It does not change the weight unit or round the actual net and gross weights entered on the line. Changing the setting does not recalculate all existing FWO lines automatically.

## Freight Order

| Field | How to use it |
|---|---|
| **Def. Freight Order Type** | Select the [Freight Order Type](freightordertype.md) used when a new Freight Order has no type supplied by the creation process. Its settings provide numbering, status, and cost behavior. If no type is supplied and this default is blank, creation stops. |
| **Freight Order Charges Are Editable** | Turn on to allow manual editing in the Freight Order's **Charges** section, subject to the other document and field restrictions. Turn off to make the section read-only. Reopen the Freight Order after changing the setting. **Assignment > Delete Assignment** has its own checks and does not require this switch. See [Purchase Assignment](purchaseassignment.md). |
| **Default Vehicle Unit Type** | Stores an equipment default from the Logistic Units type catalogue. In the current creation and assignment flows, verify the actual **Vehicle Unit Type** in the wizard: the choice can come from a stage or require manual selection, and this setup value does not automatically populate every new Freight Order. See the equipment guidance below. |
| **Charge Distribution Precision** | Set the increment used to round **Charges Distribution Ratio** when distributing Freight charges in **Select Allocation**. The initial value is **0.01**; zero also uses **0.01**. This is a share of the allocation, so **0.01** means one percentage point. See the rounding example below. |
| **Stage Execution Status Profile** | Select the Status Profile that supplies execution statuses for stages and [Stage Entries](stageentries.md). Configure the required execution statuses in that profile. This is separate from the Freight Order header's status profile, which comes from its Freight Order Type. |

For equipment, review the selected stages and the actual Freight Order header value. A filled Stage **Vehicle Unit Type Code** applies to that leg; a blank stage value uses the Freight Order header. The current **Create Freight Order Wizard** for selected FWOs can present an empty equipment choice even when **Default Vehicle Unit Type** is filled. Select the appropriate type explicitly before creating the order. The stage-based **Create Freight Order** and **Assign Freight Order** flows use the stage and wizard choices described in [Order Wizards](orderwizards.md).

For **Charge Distribution Precision**, splitting a Freight charge equally between three FWOs with precision **0.01** gives allocation ratios **0.33**, **0.33**, and **0.34**; the final order receives the remainder so the shares total **1**. Smaller increments allow more precise shares. Direct allocation of a purchase invoice line uses higher-precision ratios and separate currency rounding, so this setting does not force its shares to 33%, 33%, and 34%. See [purchase allocation rounding](purchaseassignment.md#allocate-one-purchase-line-across-forwarding-orders).

### Bill of Lading fields

| Field | How to use it |
|---|---|
| **Numbering Method** | Leave blank for manual Bill of Lading numbers, choose **Prefix + Freight Order No.**, or choose **No. Series**. Automatic numbering fills a blank number when a Freight Order is created. |
| **Prefix** | Enter the text placed directly before the Freight Order number, including any separator, such as **BOL-**. Editable only with **Prefix + Freight Order No.** A blank prefix uses the Freight Order number alone. |
| **Bill of Lading Nos.** | Select the independent number series used with **No. Series**. The series must allow **Default Nos.** and have a usable line for the order date. Editable only with that numbering method. |

Existing Bill of Lading numbers are preserved. Changing these settings does not renumber existing orders. See [Bill of Lading numbering](#bill-of-lading-numbering) for examples, date handling, and creation errors.

## Number Series

| Field | How to use it |
|---|---|
| **MAP Location Nos.** | Select the number series for new [MAP Locations](maplocation.md). When a location is created without a number, TMS takes the next available number from this series using today's date. For example, a series can produce `MAP00001`, `MAP00002`, and so on. Configure a usable series before creating locations automatically from customers, vendors, or addresses. Changing this field affects subsequent numbering; it does not renumber existing locations. |

Forwarding Order and Freight Order number series are configured on their respective [order types](forwardingordertype.md), not in this section. Bill of Lading numbers have their own [numbering settings](#bill-of-lading-numbering).

## Map Provider Settings

Choose a provider when you need address coordinates, route display, or calculated distances and durations. Complete that provider's account and API setup first; see [Map Providers](mapproviders.md).

| Field | How to use it |
|---|---|
| **Map Provider** | Select **Google Map** or **Azure Maps**. Leave it blank if your process does not use map services; operations that require a provider will then ask for setup. Selecting a provider enables its credential section and disables the other provider's section. |
| **Google Api Key** | Enter the Google Maps key used for both service requests and map display. The field is available when **Map Provider = Google Map**. A masked placeholder means a key has been saved. Enter a replacement key to update it, or clear the field to remove it. |
| **Azure Maps Subscription Key** | Enter the Azure Maps key used for both service requests and map display. The field is available when **Map Provider = Azure Maps**. A masked placeholder means a key has been saved. Enter a replacement key to update it, or clear the field to remove it. |
| **Azure Maps Geo Scope** | Choose **Global**, **Europe**, or **United States** for the Azure Maps geographic endpoint. This field is available with Azure Maps. Choose the scope agreed with your administrator for the company's data-processing requirements. It is not a filter on the countries included in a transport route. |

The keys are stored encrypted for the current company. They cannot be revealed from this page. Permission to modify TMS Setup is required to enter, replace, or remove them. The same key is also supplied to the browser for map display; apply the provider's access restrictions and usage limits described in [Map Providers](mapproviders.md#credential-model).

The disclosure and **Map provider privacy and terms** link explain which address, coordinate, and route data goes to the external provider. Saving a provider or key alone does not send business addresses; map operations do. Review the [outbound data-transfer information](mapproviders.md#privacy-and-outbound-data-transfer) before use.

After configuration, open a known MAP Location and verify both geocoding and map display. This checks the service and browser uses of the credential.

## Role Center Settings

These settings configure up to five Forwarding Order tiles and five Freight Order tiles in the **TMS Dispatcher** Role Center. Each tile shows an order count and opens the matching list when selected.

Each numbered pair has two fields:

- **Caption** is the text on the tile. A blank caption hides that tile.
- **Value** is a Business Central filter on the order's **Status Code**. Enter the codes used in your status profiles, not their descriptions. A blank value applies no status restriction, so a visible tile counts orders of every status.

### Forwarding Order tiles

| Tile | Label field | Status filter field |
|---|---|---|
| 1 | **FWO Filter 1 Caption** | **FWO Filter 1 Value** |
| 2 | **FWO Filter 2 Caption** | **FWO Filter 2 Value** |
| 3 | **FWO Filter 3 Caption** | **FWO Filter 3 Value** |
| 4 | **FWO Filter 4 Caption** | **FWO Filter 4 Value** |
| 5 | **FWO Filter 5 Caption** | **FWO Filter 5 Value** |

### Freight Order tiles

| Tile | Label field | Status filter field |
|---|---|---|
| 1 | **FO Filter 1 Caption** | **FO Filter 1 Value** |
| 2 | **FO Filter 2 Caption** | **FO Filter 2 Value** |
| 3 | **FO Filter 3 Caption** | **FO Filter 3 Value** |
| 4 | **FO Filter 4 Caption** | **FO Filter 4 Value** |
| 5 | **FO Filter 5 Caption** | **FO Filter 5 Value** |

For example, if your FWO status codes include `1.New` and `2.Confirmed`:

1. Enter **Awaiting planning** in **FWO Filter 1 Caption**.
2. Enter `1.New|2.Confirmed` in **FWO Filter 1 Value**. The vertical bar means **or**.
3. Reopen the Role Center, select the tile, and verify the orders shown.

For a Freight Order tile that includes an empty status and `1.Plan`, use `''|1.Plan` as the value. Replace these example codes with the codes in your company. To show all statuses, leave the value empty and keep a caption. To remove a tile from view, clear its caption.

Filters do not change order statuses. They are not restricted to one status profile, so the same status code in several profiles is included. An order can appear in more than one tile if their filters overlap. Changing these company settings changes the corresponding tiles for all users of this Role Center; reopen the Role Center to load the updated values.

## Azure Blob Storage for Attachments

Use this section when you want TMS attachment files stored in Azure instead of only in the Business Central database. Prepare the storage account and container first.

| Field | How to use it |
|---|---|
| **Enabled** | Turn on Azure storage for new TMS attachments and enable the transfer and cleanup actions. Turning it off stops new uploads, transfer, and **Free Space**. Existing Azure attachments still open from Azure, so keep their destination and key configured. |
| **Account Name** | Enter the Azure storage account that holds the company's attachments. Select the intended account before uploading files. Once Azure attachments exist, TMS prevents changing this value, even if **Enabled** is off. |
| **Container** | Enter the container within that account. Once Azure attachments exist, TMS prevents changing this value. Changing setup does not move existing files to another destination. |
| **Account Access Key** | Enter the access key for the selected account. TMS stores it encrypted for the current company and displays a masked placeholder. Enter a new key to replace it, or clear the field to remove it. You need permission to modify TMS Setup. Keep a valid key while any attachments are stored in Azure. |

Choose **Azure Blob Storage > Check Connection** before transferring files. To move existing local files, choose **Transfer Attachments to ABS**, review the transferred attachments, and then use **Free Space** to verify the Azure copies and remove matching local file references. Enabling storage alone does not transfer existing local files.

See [Azure Blob Storage for Attachments](azureblobstorage.md) for the complete procedure, destination restrictions, cleanup behavior, and troubleshooting.

## Emergency Response

Maintain the company contact details used for dangerous-goods assistance. These are shared company values; the report reads the current setup rather than a historical copy stored on each shipment.

| Field | How to use it |
|---|---|
| **Emergency Response Provider** | Enter the name of the emergency response service your company uses. The bundled **Dangerous Goods Bill of Lading** prints this name with the emergency phones. |
| **Emergency Response Phone** | Enter the primary emergency phone number for that provider or service. It prints on the bundled Dangerous Goods Bill of Lading. |
| **International Emergency Response Phone** | Enter the emergency number suitable for international transport, including the country code and any necessary dialing instructions. It also prints on the bundled report. |
| **Emergency Response Contact** | Enter the registered person, department, or company recognized by the provider. Available to custom Dangerous Goods Bill of Lading layouts; the bundled layout does not print it. |
| **Emergency Response Contract ID** | Enter the contract, subscription, or customer identifier the provider uses to recognize your company. Available to custom Dangerous Goods Bill of Lading layouts; the bundled layout does not print it. |
| **Emergency Response Notes** | Record company-wide emergency instructions or reference information. These notes are not included in the Dangerous Goods Bill of Lading dataset. Maintain cargo classification and special provisions on the Transport Profile. |

See [Transport Profiles](transportprofiles.md) for the related setup and [Reports](reports.md) for layout behavior.

## Corrective Forwarding Orders

Set **TMS Setup > Forwarding Orders > Correction FWO Type Code** before correcting financial documents assigned to posted Forwarding Orders.

1. Enable **Financial Correction** on the [Forwarding Order Type](forwardingordertype.md#types-for-financial-corrections), configure its **No. Series**, and select it in **Correction FWO Type Code**. This setup field only accepts financial correction types.
2. If the type has a status profile, its default status must allow **Settlement**, the affected **Income** or **Cost** lines, and **Posting**. A blank status profile follows the usual unrestricted status behavior.
3. Start **Cancel**, **Correct**, or **Create Corrective Credit Memo** from a linked Posted Sales Invoice or Posted Purchase Invoice.
4. When posted FWO allocations are involved, confirm the single prompt showing the number of corrective orders and the configured type.

TMS creates one active corrective FWO per original order and correction operation. **Correct** uses the same corrective FWO for its credit memo and replacement invoice. Its header stores the original posted FWO number and version and the document-correction operation ID; these values remain on the posted corrective order. The new order keeps the original FWO's **Ordering Party** and saved header dimensions. Purchase corrections keep that customer; the credit memo's vendor belongs to the Cost rows.

The new order copies **Financial Correction** from its type. Creation uses the configured type's numbering and status, but does not generate stages, cargo, logistic units, Service Set lines, or agreement charges. Current customer defaults do not add a new forwarding agreement or overwrite the saved ordering-party identity. The corrective order's type cannot be changed afterward. Its linked financial documents supply the Settlement quantities, prices, currency, and dimensions.

The original posted FWO and its Settlement remain unchanged. On an active financial correction FWO, **General > Correction** shows the saved **Financial Correction** flag and **Corrects Forwarding Order**. **Has Corrections** is shown only on posted FWO cards. The result including corrections appears in **Settlement**, below its usual totals. Financial orders hide cargo, logistic units, Shipper & Consignee, Plan, Stages, and transport actions. See [posted correction history and profit](postedhistory.md#invoice-cancellation-in-posted-history).

Users can also create a [manual financial correction](forwardingorder.md#create-a-financial-correction-manually) with **New**, a financial correction type, and a selected original posted FWO. This does not require a sales or purchase correction operation. The original customer is copied, while Settlement is entered or assigned separately. The original can be changed only before Settlement exists; automatic corrections cannot be reassigned. Every financial correction must identify an original posted FWO before posting. Type setup changes do not rewrite existing orders or their posted snapshots.

A missing type or unusable numbering/status setup stops creation. Creating these orders requires interactive confirmation; a background or API request cannot silently accept it. Active original FWOs keep their correction rows on the existing order and do not require a new corrective FWO.

## Source document linking status

Under **Finance and Invoicing > Settlement**, set **Sales Document Linking Mode** and **Purch. Document Linking Mode** independently. They control the status of an existing, unposted Business Central document when its lines are newly linked to Settlement.

| Mode | Source statuses allowed for a new link |
|---|---|
| **Open and Released** | **Open** or **Released**. This is the default for both settings. |
| **Open Only** | **Open** only. |
| **Released Only** | **Released** only. |

**Pending Approval** and **Pending Prepayment** are excluded in every mode. The selection lists use the setting, and TMS checks the current source status again when performing the operation. A status error identifies the document, its current status, and the setting that prevented the link. TMS does not automatically release or reopen the source.

The sales setting applies to importing existing sales lines into Settlement, including import while creating an LSP Forwarding Order or adding lines to an existing FWO. The purchase setting applies to **Import Purchase Document**, **Link to Purchase Line**, **Charge Allocation**, and assigning an existing purchase invoice through **Freight Order Charges**. Other source, mapping, allocation, permission, and FWO/FO status checks still apply.

These settings do not control **Create** or **Add to Document**: creating new financial documents or lines retains the normal **Open** requirement. Changing a linking mode does not detach existing links or stop their supported source refresh. **Detach Settlement**, financial posting, and import of already posted documents retain their separate checks.

When upgrading from a version without these fields, both modes start as **Open and Released**. This permits Released sales imports and Open purchase allocations when otherwise eligible; direct purchase linking now excludes Pending Approval and Pending Prepayment. Review the two settings if your company requires a narrower policy. Existing links and source statuses are not rewritten.

See [Settlement](settlement.md#how-settlement-relates-to-documents) and [Purchase Assignment](purchaseassignment.md) for the corresponding actions.

## Settlement input signs

Under **Finance and Invoicing > Settlement**, **Make Settlement Input Positive** is **off by default**. Enable it to prevent accidental minus signs when users enter **Quantity**, **Price**, or **Amount** in Settlement.

When enabled, a new negative input is changed to its positive value, for example **−100 → 100**. An informational Message identifies the field and change, explains that TMS calculates the P/L sign automatically and that credit memos use **Financial Effect = Credit**, and points back to this setting. The user can continue with the positive value; this is not a validation error.

If the intended document line really needs a negative quantity, price or amount, turn the setting off temporarily, enter the intended value, and enable it again when appropriate. The setting does not rewrite existing rows or make P/L positive. Linking, synchronization, migration, template generation and copying retain their source/calculated values. Editing another field on a row with an existing negative value does not normalize that saved value.

The same input rule applies to Settlement API writes. Interactive sessions show the Message; API/background sessions receive no popup, so integrations should use the normalized values returned by the API. See [Settlement document values and signs](settlement.md#quantity-price-and-amount-signs).

## Default Forwarding Order Type by customer, vendor, and address

When creating an FWO from a sales or purchase source, TMS first uses **FWO Type** saved on that document. Customer, vendor, and address defaults can supply this value when the source document is prepared. They are configured on their own cards; see [Business Central Integration](integration.md#default-fwo-type-from-source-documents) for how those suggestions work.

If the source document's **FWO Type** is blank, the settings on TMS Setup apply as follows:

| Creation process | Setting to review |
|---|---|
| Standard creation from a Sales Order or Sales Return Order | **Default FWO Type for Sales**. If blank, an interactive session asks for a type; cancelling stops creation. Without an interactive session, configure the default. |
| Sales **Create FWO as LSP** | **Select FWO Type for LSP**. On asks for a type; off requires **Default FWO Type for Sales**. |
| Posted sales documents and combined shipments | **Default FWO Type for Sales** must be configured when the source has no type. |
| Purchase Order, Purchase Return Order, Posted Purchase Receipt, or Posted Return Shipment | **Default FWO Type for Purchases** must be configured when the source has no type. |

For example, keep a standard sales type in TMS Setup and select a different **FWO Type** on a sales document for a special transport process. The saved document type takes priority when that FWO is created.

Transfers use **Default FWO Type for Transfers**. The **New Forwarding Order Wizard** initially suggests **Def. Forwarding Order Type**. These are separate choices from the sales and purchase fallbacks.

Changing a setup default does not change existing FWO types. Review the type before creating the order, because it determines the numbering, stages, and status rules of the new FWO.

## Bill of Lading numbering

Open **TMS Setup > Freight Order > Bill of Lading Numbering**. This group contains three fields:

| Field | Use it for |
|---|---|
| **Numbering Method** | Leave blank for no automatic numbering, choose **Prefix + Freight Order No.**, or choose **No. Series**. |
| **Prefix** | Enter the text to place directly before the Freight Order number. Editable only for **Prefix + Freight Order No.**. |
| **Bill of Lading Nos.** | Select an independent Bill of Lading number series. Editable only for the **No. Series** numbering method. |

Switching methods retains the saved **Prefix** and **Bill of Lading Nos.** values. Selecting a method does not require you to have already filled its series; the required settings are checked when a Freight Order is created.

With **Prefix + Freight Order No.**, the prefix is joined literally to the **Freight Order No.** Include any separator in the prefix. For example, **BOL-** plus **FO-00001** produces **BOL-FO-00001**. A blank prefix uses the Freight Order number alone. The complete result must fit the existing 20-character Bill of Lading No. field; a longer result causes a clear error and is never truncated.

With **No. Series**, fill **Bill of Lading Nos.** with a series that has **Default Nos.** enabled. Business Central takes the next number from the series line valid for the Freight Order's **Document Date**, or the work date if Document Date is blank. For example, a configured series can produce **BOL00001**. A missing series, disabled Default Nos., or an unusable date-specific series line prevents creation with a clear error.

If **Bill of Lading Nos.** is blank for the **No. Series** method, the creation error identifies the missing Bill of Lading series and directs you to this setup group. Select the series, or leave **Numbering Method** blank if you want to enter Bill of Lading numbers manually, then create the Freight Order again.

Automatic numbering runs only when a Freight Order is created, after its own number exists. This covers creation from the card, wizards, and the API, including API requests that supply an explicit Freight Order number. An already populated Bill of Lading No., including a manually supplied number, is preserved. Temporary records do not consume Bill of Lading sequence numbers.

Changing setup does not fill or renumber existing orders. Editing or renaming an order and printing reports do not generate or recalculate its Bill of Lading No. Reports continue to read the stored value, and posting copies that value to the Posted Freight Order.

## Useful actions

Use the **Reports** menu on **TMS Setup** to configure printed forms. **Report Selection** opens **Report Selection - Transportation Management System**, where you maintain the TMS document/report mapping and email settings. **Report Layouts** opens the current Business Central layout-management page, where you can import or update user-defined templates. **Report Layout Selection** sets a report's default layout for a company, and **Printer Selections** configures report/user printer assignments. These standard layout and printer pages can include reports from other installed extensions as well as TMS.

**Set default reports** is also in this menu. It adds missing built-in report rows without replacing configured Report IDs or layouts. In the new Packing List and Bill of Lading usages it creates three Sequence rows; standard BC prints every remaining row. Delete unwanted alternatives to use a single form. See [Report selections by document usage](reports.md#report-selections-by-document-usage). Opening a setup page does not initialize or migrate selections.

| Action | Use it for |
|---|---|
| **Home > Extension Upgrade Log** | Review installation, upgrade, and version-check messages. See [TMS log retention](#tms-log-retention). |
| **Home > Check TMS Settings** | Check that General Ledger Setup exists. If the company's TMS Setup record is missing, confirm installation of default setup and master data. When TMS Setup already exists, this action does not validate individual settings; use the [verification checklist](#verify-setup). |
| **Home > Assisted Setup** | Open the TMS wizard, configure a map provider, and reconcile MAP Locations for customers, vendors, ship-to addresses, order addresses, and locations. |
| **Reports > Report Selection** | Select reports and email settings for TMS document usages. |
| **Reports > Report Layouts** | Manage built-in and user-defined report layouts. |
| **Reports > Report Layout Selection** | Choose default report layouts for a company. |
| **Reports > Printer Selections** | Assign printers to reports and users. |
| **Reports > Set default reports** | Add missing default report selections and grouped alternatives without overwriting existing target rows. |
| **Azure Blob Storage > Check Connection** | Test access to the configured Azure container. This does not verify individual attachment files. |
| **Azure Blob Storage > Transfer Attachments to ABS** | Upload existing local TMS attachments to Azure Blob Storage. Keep their local copies until **Free Space** verifies them. |
| **Azure Blob Storage > Free Space** | Verify transferred files in Azure and remove matching local file references. See the [storage guide](azureblobstorage.md#free-space) for verification failures and the separate cleanup of attachments whose active order no longer exists. |
| **Data Migration > Settlement Amount Model Upgrade** | Preview and manually convert existing Settlement on active and posted Forwarding Orders to document amounts, discounts, and P/L amounts. See the [upgrade procedure](#upgrade-existing-settlement-amounts). |
| **Data Migration > Repair Settlement Drafts** | Restore normal editing of eligible unlinked imported lines, including their FWO owner link and missing Income customer, while preserving financial amounts. See the [draft repair procedure](#repair-blocked-settlement-drafts). |
| **Data Migration > Repair** | Preview and repair supported document links after data import. Review repaired, skipped, and failed links individually; see [Repair GUID links](#repair-guid-links-after-data-import). |
| **AI & Copilot Setup > AI & Copilot Settings** | Open AI Setup to configure AI availability and the model deployment, endpoint, and API key. See [Copilot](copilot.md) for supported tasks. |
| **AI & Copilot Setup > AI Task Entries** | Review recorded AI task results, messages, duration, token counts, and request/response details when investigating an AI result. |
| **Instructions > Instructions** | Maintain reusable instruction content for documents. See [Instructions](instructions.md). |
| **Instructions > Instruction Types** | Maintain the types that group instructions and define where they apply. See [Instruction Types](instructions.md#create-instruction-types). |

Google Maps and Azure Maps receive address, coordinate, route, and travel-mode data when map operations run. Review [privacy and outbound data transfer](mapproviders.md#privacy-and-outbound-data-transfer), then record the organization-level decision on the standard Business Central **Privacy Notices Status** page when required by company policy.

## TMS log retention

Use **TMS Setup > Log > Extension Upgrade Log** to review diagnostic events such as installation, upgrades, and version checks. To limit how long these events are kept, configure a standard Business Central **Retention Policy** in each company. Installing or upgrading TMS makes the log table selectable; cleanup starts after an administrator configures and enables a policy.

1. Search for **Retention Policies**, choose **New**, and select **TMA Log**, table ID **70611618**. Its table caption is **Log**. If it is missing from the lookup, choose **Refresh Allowed Tables** and try again.
2. Enable **Apply to all records** for one period covering the whole log. Check that **Date Field No.** selects **SystemCreatedAt**, the creation timestamp suggested by TMS.
3. Choose a **Retention Period** suitable for your diagnostic needs. Use **Retention Periods** to define a period if required.
4. Choose how to apply the policy: enable **Manual** for manual cleanup, or leave **Manual** off for scheduled cleanup.
5. Enable the policy when its period and scope are ready. For manual cleanup, use **Apply Manually**. For scheduled cleanup, review the standard **Apply Retention Policy** job queue entry created when the policy is enabled and its schedule.

Applying the policy deletes expired events according to their creation date and keeps newer events. Review the outcome in **Retention Policy Log**.

Setting up policies requires **SUPER** or **Retention Policy Setup** permissions. The user applying cleanup, including the job queue user, needs **Read** and **Delete** permissions on **TMA Log**. Reinstalling, upgrading, or refreshing the allowed tables preserves your selected period, filters, and application mode.

See [Business Central retention policies](https://learn.microsoft.com/dynamics365/business-central/admin-data-retention-policies) for standard period, filter, and scheduling options.

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

A shared source line that still exists is converted as a complete allocation group. This can include active orders, posted orders, and historical versions outside the selected filter. The preview lists these additions. Consistent historical copies of the same FWO and Settlement line count as one share; different lines in the same FWO remain separate shares. Allocation follows a stable order by FWO and Settlement line, regardless of which order was selected first. Saved model-1 and model-2 shares are reserved before allocating the remaining source values to model-0 rows. Their allocation factors remain fixed; in a complete group, normalization adjusts only the remaining model-0 factors. The last new share receives the remaining balance only for a complete group; a valid saved share is not recalculated to absorb rounding.

Gross amount, line discount, invoice discount and net amount are allocated together when the source uses the relationship Gross minus discounts equals Net. Rounding preserves this relationship and the source component totals without reversing their signs. If the remaining values cannot support consistent shares within the source currency rounding precision, Preview reports an unresolved allocation.

When saved allocation factors are missing or invalid, the wizard first tries to recover them from stored quantities, then from financial amounts in the source document currency. It checks the totals against the source and rejects zero or incompatible-sign weights. It does not divide arbitrary quantities by their sum: several copies of the full document quantity or amount are not evidence of equal shares. LCY amounts are not used as foreign-currency weights. Preview explains whether quantities or amounts were used, and recovered shares pass through the same document calculation as ordinary linking.

If the allocation cannot be recovered and **every FWO in the group is posted**, the wizard converts each physical historical row from its own saved financial amounts without redistribution or current exchange-rate conversion. References remain, and the source SystemIds are refreshed. Each version keeps its saved financial contribution, even when different historical versions disagree. Preview shows an informational diagnostic. A group containing active FWOs still requires a reliable allocation and remains unresolved when none can be established. Already converted rows remain skipped.

Linked model-0 rows with an available source are refreshed from their actual open or posted financial document through the same source-reading mechanism used by Settlement. Source values take precedence over the old plan. Model-1 rows transfer their saved monetary values to the canonical fields without revaluing their financial result. For both versions, conversion resolves the source by Settlement side, document type, document number and line number, and replaces Source Header SystemId and Source Line SystemId with the identifiers in the current company. Old identifiers from configuration-package imports do not block conversion. The identifiers found during Preview are rechecked under locks before saving; if the source changes after Preview, preview again. Rows with no current document reference are converted from their saved financial values without prior verification; previous LCY contributions are preserved. Ambiguous allocations involving active FWOs still require review. The preview totals include every selected historical version, so they are conversion checks rather than a consolidated financial report.

Active model-1 and already converted model-2 rows with an available source also undergo a comparison with its actual document-currency amounts, discounts, currency context, posting date and source identifiers. Natural-quantity rows additionally check unit price and unit of measure. An inconsistent saved share is unresolved; the wizard does not silently replace its financial result. Convert compares the source facts read during Preview with a fresh read under locks, even if the changed source would produce the same projected Settlement values. Model-2 rows participate in these checks but are never rewritten. Missing or incomplete source references on active model-2 rows require review; the missing-source detachment described below applies only to rows awaiting conversion.

For active model-1 and model-2 rows with Natural source quantities, Preview also checks the complete source allocation group. During conversion it can restore a missing **Quantity (Base)** only on active model-1 rows. Related active and posted rows are included; consistent versions of the same FWO and Settlement line count once. Saved historical and already converted quantities remain fixed. For example, when the source base quantity is 3 and a posted share already contains 2, the remaining active share receives 1. A model-0 row also receives the remaining 1 without replacing the fixed 2 with a newly calculated 1.5. Preview Details explains each proposed model-1 base-quantity repair. Saved model-1 monetary amounts, exchange rates and allocation factors are preserved.

An incomplete or conflicting group cannot supply a missing base quantity and remains unresolved when restoration is needed. A valid partial group with known quantities retains its saved shares. The original saved model-1 base quantity determines whether it is missing; an intermediate model-0 group calculation cannot turn a guessed quantity into historical evidence. Posted financial lines do not retain their original base rounding precision: if several missing quantities cannot be established from the saved shares, or pending model-0 shares compete with unknown historical quantities, Preview asks for review instead of inventing individual historical quantities. A zero base quantity in a fixed historical share can also require review when restoring another share; valid saved zeros are preserved when the group already reconciles. For a complete group with an open source, fixed base quantities are also checked against its available rounding precision and allocation factors. An invalid fixed quantity cannot be compensated by assigning a wrong remainder to another row. Already converted model-2 rows and posted history are not rewritten by this base-quantity repair.

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

Source availability is rechecked before saving using the original document reference, including when the proposed active row has already been prepared with an empty reference. If a previously missing source reappears or an existing source disappears after Preview, preview the selection again. This repair applies only to the conversion wizard. Already converted model-2 rows are skipped when writing; the wizard is not a general link-repair operation.

Preview also checks that each Settlement row belongs to an existing active FWO or an existing posted FWO version. Rows with a missing FWO owner remain visible in Preview Details with **Skipped (Missing FWO)** and a diagnostic identifying the preview entry, original Settlement line number, and original Settlement SystemId. **Skipped Rows (Missing FWO)** counts them separately. They do not block conversion and are excluded from Rows to Convert, Unresolved Rows, Already Converted Rows, and the financial totals. Convert leaves these rows unchanged, including their amount-model version and document references. Posted version 0 is accepted if that FWO version exists.

When a shared source-document group includes a skipped FWO share, the converter retains reliable shares of the remaining rows without increasing them to cover the skipped share. A partial group receives no remaining balance from the full source document. Missing allocation evidence for a real FWO still follows the existing unresolved-allocation rules. A missing FWO owner is separate from a missing sales or purchase document; the source-document conversion rules above still apply.

All rows in one run are saved together. Already converted rows are checked and skipped, making reruns safe. The wizard changes only the permitted Settlement amount and source-snapshot fields; it does not create, post, or reverse Business Central financial documents. Posted Settlement remains read-only outside this controlled conversion and the existing reporting-currency process.

## Repair GUID links after data import

Configuration Packages can import document numbers and line numbers while leaving GUID links from the source company. **TMS Setup > Data Migration > Repair**, also available through search as **TMS Data Repair**, rebuilds supported links from the corresponding records in the current company. It is available to **TMS Administrator**.

Import the related orders, master records, and financial documents first. The repair covers active and posted TMS order owners, Ordering Party, supported stage relationships, and supported financial source links. It does not assign new SystemIds, recreate missing records, or change financial amounts. Logistic Units remain outside this wizard and use their existing migration tools.

1. Choose **Repair**, then select **Active Orders**, **Posted Orders**, or both.
2. Enter a **Forwarding Order No. Filter** or **Freight Order No. Filter**. A filter such as `JFK.03162` includes related orders only where the stored natural relationships identify them. When both filters are filled, both must match. Leave both blank to check the selected active and posted scope.
3. Choose **Preview**, then **Details**. Review each source record, field, current GUID, proposed GUID, target record, and diagnostic. Preview does not write changes.
4. Choose **Repair** and confirm. Eligible links on one record are repaired together. Each record is saved separately, so successful repairs remain saved if another record fails or the run is interrupted.
5. Review **Repaired Links**, **Skipped Links**, **Failed Links**, and **Details**. Import missing targets or resolve other diagnostics, then run a fresh Preview.

Missing or ambiguous references are skipped and do not block other eligible records. Posted FWO and FO versions are separate sequences; the repair does not assume that equal version numbers identify the same historical relationship. Unsupported historical, generation, and provenance links remain for separate review. If a record or its target changes after Preview, that record is not silently repaired against the old preview.

This wizard repairs GUID references. Use **Repair Settlement Drafts** below for the separate draft-direction issue, and **Settlement Amount Model Upgrade** for old amount representations.

During Preview and Repair, the progress window shows the operation, numbered stage, current record, processed/total count, percentage bar, elapsed time, and approximate time remaining in the current stage. Preview also shows ready and skipped links; Repair shows successful and failed records and repaired links. Estimates appear after at least three completed records and two seconds of work. Preview estimates cover the current table or preparation stage; the Repair saving stage estimates the remaining selected records. Database waits can pause updates and change the estimate. If Repair is cancelled, completed records remain saved; run Preview again for the remaining work.

## Repair blocked Settlement drafts

Imported unlinked Settlement lines can retain old editing restrictions or an FWO owner GUID from the source company after their prices and amounts have been converted. An Income line can also have an empty **Customer/Vendor No.** while the FWO already has a customer in **Ordering Party**. **Repair Settlement Drafts**, available to **TMS Administrator**, checks these lines and restores normal draft editing when their ownership, independent status, and stored financial values can be verified.

1. Open **TMS Setup > Data Migration > Repair Settlement Drafts**, or search for **Repair Settlement Drafts**.
2. Enter a **Forwarding Order No. Filter**, such as `FRA.02883`. Leave it blank to check all active orders.
3. Choose **Preview**, then **Preview Details**. Review each row and its diagnostic, including proposed FWO owner-link changes and the customer number for an Income line with no customer. The before/after P/L amounts must match.
4. Review **Rows to Repair** and **Skipped Rows**. Rows marked **Ready** can be repaired even when other rows are skipped; their diagnostics explain what still needs attention.
5. Choose **Repair** and confirm the ready row count. Each row is checked and saved separately. If a row changed since Preview or fails another check, its repair is rolled back and the remaining ready rows are processed. Successful repairs remain saved if another row fails or the run is interrupted.
6. Review **Repaired Rows**, **Skipped Rows**, **Failed Rows**, and **Preview Details**. The **Repair Status** column shows the result for each row. Resolve remaining diagnostics and run a new Preview as needed. Refresh the affected Forwarding Order with **F5** to reload the repaired lines before retrying the edit.

The repair verifies that an unlinked line can safely become an ordinary editable draft. This includes converted imported lines still marked **Needs Review** with a legacy quantity representation, as well as an incorrect draft direction marker. It restores the required draft state only after the checks pass and verifies the saved result is editable. When needed, it also fills an empty **Customer/Vendor No.** on Income from the FWO's nonblank customer Ordering Party. An existing different customer remains a conflict and is not overwritten. Cost lines do not inherit the FWO customer.

For eligible unlinked **Income and Cost**, the same Preview and Repair restores **FWO System ID** and **FWO Type Code** from the existing active Forwarding Order found by **Forwarding Order No.** This covers a blank or stale imported owner GUID and an outdated stored FWO type, including an old GUID that still points to another existing order from which the row was copied. The row's Forwarding Order No. determines its owner. A separate GUID Repair run is not required for this owner-link case. A missing parent order remains a diagnostic.

Prices, quantities, discounts, currencies, amounts, P/L, and row identity are preserved. The repair applies only to eligible independent drafts already using amount-model version 2. Linked or posted rows, protected generated rows, and archived orders remain outside this repair. Rows still requiring amount-model conversion use [Settlement Amount Model Upgrade](#upgrade-existing-settlement-amounts).

Normal order-status and document restrictions continue to apply. A skipped or failed Income can still prevent changing Ordering Party on its FWO until that row is resolved, even when other rows were repaired successfully. If Preview finds no candidates, review the reported Settlement line's document links, verification status, and pricing source; the error can have another cause.

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
| Settlement cannot create an invoice | Check service mapping, customer and credit limits, Settlement line eligibility, and status permissions. Choose the required document with the explicit Sales Order or Sales Invoice action. |
| Posting is blocked | Review posted document control, settlement document posting, and FWO posting date control. |
| A customer or new sales document is blocked by the credit limit | Check the Ordering Party's **Balance (LCY)** and **Credit Limit (LCY)**, and review **Credit Limit Control**. Reaching the limit exactly also blocks the operation. |
| A Role Center tile is missing or counts unexpected orders | Check the numbered Caption/Value pair. An empty caption hides the tile; an empty value includes all statuses. Use actual Status Codes and reopen the Role Center. |
| A map provider key cannot be entered | Select the matching **Map Provider** and confirm permission to modify TMS Setup. |
| Changing Amount Rounding Precision has no effect | Current Settlement and sales creation use currency rounding settings. See [Additional](#additional). |
| The equipment field is blank despite Default Vehicle Unit Type | Review the stage and wizard equipment choices and select the type explicitly. See [Freight Order](#freight-order). |
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
