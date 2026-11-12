---
title: "Reports and Documents"
description: "Print or email Forwarding Order, Freight Order, settlement, cargo, and carrier documents."
---

# Reports and Documents

Use **Reports and Documents** to produce customer, carrier, cargo, and internal documents from TMS.

Documents should be printed only after the required order, stage, carrier, cargo, and settlement data has been reviewed.

## Before you start

Make sure that:

- report selections are configured in [TMS Setup](setup.md),
- customer and carrier contact details are filled,
- Freight Orders contain carrier, route, and cargo data,
- stage entries are reviewed when reports include execution event details,
- settlement lines are reviewed before financial documents are created.

## Report selections by document usage

Open **TMS Setup > Reports > Report Selection - Transportation Management System**. **Usage** lists the new document purposes from **TMA Document Report Usage** and maps them to the corresponding standard **Report Selection Usage** values.

The six Freight Order purposes are **Freight Order**, **Freight Order - Details**, **Freight Order - Load Summary**, **Freight Order - Packing List**, **Freight Order - Bill of Lading**, and **Freight Order - Booking Confirmation**. Forwarding Order purposes are **Forwarding Order**, **Forwarding Order - Confirmation**, **Forwarding Order - Invoice**, and **Forwarding Order - Load Summary**.

Each row is a standard Report Selections row. **Sequence** controls the output order; it does not select a preferred alternative. **Printing runs every configured report for that Usage.** To print only one form, delete the other rows or replace the Report ID on the remaining row. There is no additional TMS printing flag.

Initial alternatives are:

| Usage | Sequence 1 | Sequence 2 | Sequence 3 |
|---|---|---|---|
| Freight Order - Packing List | TMA Packing List | TMA Packing List By Drop | TMA Packing List by Container |
| Freight Order - Bill of Lading | TMA Bill of Lading | TMA Bill-of Lading By Drop | TMA DG Bill of Lading |

The other supported purposes receive their corresponding standard report in Sequence 1. **Forwarding Order** and **Forwarding Order - Load Summary** remain empty until an appropriate report is selected; no unrelated report is assigned automatically. **Forwarding Order - Invoice** uses the existing invoice workflow and its Sales Header input.

Use **Report Layout**, shown after **Email Body Layout** as on the standard Report Selection - Sales page, to choose a modern layout for the printed form/PDF. Select the value in that column to open the standard layout selector for the row's Report ID. **Email Body Layout** selects the email text template separately. Leaving the document layout unassigned uses the standard report/company default. The company choice is stored in **Report Selections**; no additional fields in **Custom Report Selection** are required for this feature. That separate standard table holds individual customer/vendor document settings. **Use for Email Attachment** and **Use for Email Body** retain their standard meanings. Multiple attachment rows can produce multiple email documents; vendor/customer-specific settings continue to follow standard Business Central behavior.

The Freight Order card exposes one print/email group per purpose. Variant-specific actions are hidden. Print availability depends on configured Report IDs; email availability follows the effective vendor attachment selection. Reopen the card after changing selections in another window.

To configure an individual partner, open **Document Layouts** from the Customer or Vendor card. Vendor layouts include the six Freight Order purposes listed above and apply to the Freight Order's **Pay-to Vendor No.** Customer layouts include **Forwarding Order - Confirmation**, **TMS Sales Invoice**, **TMS Sales Periodic Invoice**, **TMS Sales Posted Invoice**, **TMS Sales Posted Credit Memo**, and **TMS Sales Posted Periodic Invoice**. Confirmation uses **Order Party No.**; Sales documents use **Bill-to Customer No.**

Use the standard **Copy from Report Selection** action to add configured standard and applicable TMS reports, or select a TMS purpose directly in **Usage**. On customer layouts, **Copy from TMS Report Selection** adds only the current customer TMS purposes. Copying preserves existing selections for the same purpose and Report ID. Configure the report, attachment/body layouts, and email recipients on the individual rows. If no applicable individual selection exists, Business Central uses the company report selections. Previous-architecture partner selections are retained; configure the new purposes explicitly because these actions do not migrate old usage IDs.

New model defaults are initialized once during install/upgrade. Existing target rows and layouts are preserved. Later upgrade/reinstall runs do not restore deleted selections. **Set default reports** is an explicit action that adds missing built-in alternatives again without overwriting existing target rows. Previous-architecture records are retained, but are not automatically migrated into the new purposes; review company and counterparty choices for the new usages.

Credit Memo, Posted Invoice, Periodic Invoice, warehouse, sticker, and Delivery Order scenarios retain their previous usages and defaults until their new purposes are defined.

## Common documents

| Document | Use it for |
|---|---|
| **Forwarding Order Confirmation** | Customer-facing confirmation of the transportation job. |
| **Freight Order** | Carrier instruction for execution work. |
| **Freight Order Details** | Detailed cargo and stage information. |
| **Load Summary** | Pre-shipment assignment grouped by Drop: planned FWO goods, item and customer codes, quantities, calculated gross weights, and instructions marked **For Load Summary**. |
| **Packing List** | Cargo packing and handling information. |
| **Packing List by Container** | A separate document for each loaded container, with products, lots, source references, quantities in the linked sales document UOM, and cargo gross weights. |
| **Bill of Lading** | Transport handover documentation when required. Instructions marked **For Bill of Lading** are available to the report dataset. |
| **Dangerous Goods Bill of Lading** | One BoL per Freight Order, with item/variant cargo rows, cargo-specific Transport Profiles selected for the first FO stage, logistic-unit seals, Carrier instructions, and calculated cargo totals. |
| **Booking Confirmation** | Carrier or customer booking confirmation. |
| **Settlement Summary** | Internal review of income, cost, and profit. |

For instruction selection, see [Instructions in reports](instructions.md#instructions-in-reports). **Freight Order**, **Load Summary**, and **Dangerous Goods Bill of Lading** print selected saved FO instructions as plain text, without embedded pictures. The ordinary **Bill of Lading** and **Bill of Lading by Drop** datasets include eligible instructions, but their bundled Word layouts do not display them; a custom layout must map them.

The Load Summary stage dataset includes Vehicle Unit Type Code, Transport Regulation Code, Transport Profile Code and Description, Transport Cargo Description sourced from the profile's Proper Shipping Name, and Package Type Code for every Forwarding Order Stage executed by the Freight Order.

The bundled **Load Summary** layout shows the Freight Order's **Load Date And Time** and **Unloading Date Time** beside the company information. Company phone, email and website appear below the company address. Each Drop has one repeated goods table: **Item No.**, **Customer Item No.**, **Description**, **Quantity** with its unit of measure, and **Gross Weight** with its weight unit. Items are printed directly under the Drop, without separate FWO goods tables.

Each Drop's delivery address prints on three lines: **Address**; **City**, **County** and **Post Code** separated by spaces; then **Country/Region**. This places the street first and the country last. The gray heading uses bold 12 pt text for the Drop number and bold 11 pt for the address, with vertical padding for readability. County comes from the Forwarding Order Stage's **Delivery County**, and Country/Region remains the saved code. Existing custom Word layouts must add the `Header/Groups/UnloadAddressCounty` field to display it and apply the new ordering and formatting separately, or use the updated bundled layout.

The goods table is a **planned shipment assignment**. It reads positive **Item** and **Product** lines from the Forwarding Orders whose stages are assigned to the selected FO and delivery Drop. It works before warehouse picking, logistic-unit/container creation or lot selection. Loaded LU contents, inventory tracking and reservations do not determine its goods.

A Load Summary Drop groups stages with the same saved consignee type and number, delivery address code, street, country/region, city, postcode and county. The Drop address, goods and source FWO instructions all follow this grouping. The same street in different cities remains separate. Different consignees or delivery address codes also remain separate, even at the same physical address. With a blank delivery address code, stages from different FWOs can share a Drop only when the saved street and at least one of country/region, city or postcode are filled; otherwise, different FWOs remain separate.

One FWO contributes its planned goods and **Estimated Footage** once within a Drop even if several of its stages lead to that Drop. Matching goods from different FWO can be combined only when their item type, number, variant, descriptions, customer reference and units of measure agree. The same FWO assigned to different Drops contributes its full planned goods and Estimated Footage to each Drop. Item-level allocation between partial stages is not inferred from reserved weight or volume.

Item codes, descriptions, quantities/UOM and **Customer Item No.** come from the saved FWO lines. The customer code is **Item Reference No.**; legacy FWO lines without that snapshot print a blank reference. Gross weight and volume use the line's saved base-unit totals, including any manual corrections. These are planning values, not weighed or packed container totals. SO/PO references and unlabeled Shipper/Consignee Notes remain available.

For a Sales Order source, **PO Number** is **Sales Header.External Document No.**. If the order has been deleted after posting, it is recovered from Sales Shipment Lines matched by the original order number and line number. A direct Sales Shipment source uses its shipment header's external document number. Multiple nonblank PO values for one source are listed without duplicates; a missing PO is left blank.

Drop gross-weight and volume totals are calculated from those planned FWO goods. The bundled layout does not print **Lot Nos.** or Expected Cost. Existing lot columns remain in the dataset for compatibility but are empty in Load Summary. **Shippers / Pick-Pack Instructions** follows the Drop goods, and **Carrier instructions** follows all Drops. See [Instructions](instructions.md#instructions-in-reports) for selection and grouping rules. The published **Header/Groups/DropItems** structure and its existing column names/types remain unchanged. Packing List by Container uses actual loaded cargo. DG Bill of Lading uses actual loaded cargo when LU assignments exist, with an FWO fallback described below when none exist.

## Freight Order

The bundled **Freight Order** Word layout (report 70611579) places **Carrier Instructions & Notes** below the carrier details, next to customer order and product information. It prints the saved Freight Order instructions with **Comment Group = Carrier**, including manually added instructions. It does not require **For Freight Order**, **For Bill of Lading**, or **For Load Summary**. HTML is converted to plain text, an empty instruction uses its description, and identical text prints once. Printing does not refresh the saved instructions from master data.

**General Notes / Comments** comes directly from the Freight Order's **Comment** field. Line breaks are retained. **Expected Cost** is the Freight Order's **Cost Amount (Expected)** in its **Currency Code**, with the company LCY code used when the currency is blank. This is the expected carrier cost, not the value of the goods.

The **Customer Order Information** block uses header fields for the customer PO numbers, product codes, descriptions and estimated weight. Its labels remain visible when a value is empty; the bundled layout does not depend on the nested CustomerOrderLines repeater. Product codes and descriptions come from saved Item/Product lines of the distinct Forwarding Orders assigned through FO stages. Description includes Description 2; an existing Item Variant supplies its description in the same way as the ordinary product description in DG Bill of Lading. For multiple distinct products/descriptions, each description is prefixed by its product code. Repeated stages of one FWO do not repeat its product or PO values.

**Estimated Quantity (Weight)** shows the FO **Planned Reserved Weight**, calculated from its assigned stages, with the Logistic Units base weight UOM. It matches **Total Weight Base** on the FO stage section, including partial reservations. It does not use item Quantity or the gross weight of loaded containers. For example, a planned reservation of **110 kg** prints **110 kg** even if the loaded logistic units have a gross weight of **9,290 kg**. Stage reservations are summed using the same rule as the FO card; weight is not deduplicated by FWO.

The original **CustomerOrderLines** dataset remains available to custom layouts with its original line quantity/UOM fields. Product details describe the FWO plan; this report does not expand LU-only plan lines into physical container contents.

For a Sales Order source, **Customer PO Number** is **Sales Header.External Document No.**. If that order has been deleted after posting, the report uses Sales Shipment Lines matched by the original order number and line number. A direct Sales Shipment source uses its shipment header's external document number. Missing PO values stay blank. Existing lot numbers, transport stages, logistic units, and payment details remain available. Custom layouts need their own mappings for the new dataset fields.

## How to print or email documents

1. Open the Forwarding Order or Freight Order.
2. Review required fields and lines.
3. Choose the document action.
4. Preview the document when the output is new or changed.
5. Print, download, or email according to your process.
6. Attach the final file to the order when document control requires it.

## Dangerous Goods Bill of Lading

Select **Freight Order - Bill of Lading** in report selections. **TMA DG Bill of Lading** is initially registered in Sequence 3. Keep only this row if it is the only BoL form required, then use **Bill of Lading** on the Freight Order. Configure **Use for Email Attachment** for the rows to include when emailing. The former separate Dangerous Goods actions are hidden.

The bundled Word layout uses an explicit white background behind the company logo in both positions. Its cargo table uses fixed widths for all 11 columns within the page margins. If you use a custom layout already imported into Business Central, import the corrected Word template into that layout to apply the width correction; an existing custom layout is not changed automatically.

This report uses active Freight Orders. **VEHICLE/CAR NO** prints the Freight Order's saved **Vehicle Registration No.** through the existing **Header/VehicleRegistrationNo** dataset field. To enter it, show the hidden field in **Vehicle and Driver > Vehicle** through personalization. It accepts up to 50 text characters, including optional extra text, and can be supplied without a Vehicle or Carrier record. Selecting a different Vehicle copies its Registration No.; a manual override is preserved when the same Vehicle is revalidated or Vehicle/Carrier is cleared. The report does not look up the current registration on the Vehicle card. Existing orders are not filled automatically, so this report field remains blank until the value is supplied. Posting saves the value for history, but this report does not print from Posted Freight Orders.

The report chooses one cargo source for the whole Freight Order:

| Freight Order contents | Cargo source |
| --- | --- |
| At least one concrete **Type = Unit** row in **Freight Order Content** | The actual Item/Product contents of the loaded logistic units. Use **Load Logistic Units to Freight Order** to assign them. FWO goods outside those units are excluded. |
| No concrete **Type = Unit** rows | The full planned Item/Product contents of the Forwarding Orders whose stages are assigned to the selected FO. Each FWO contributes once, even when several of its stages belong to this FO. |

These sources are never combined. Planned **Unit Type** rows do not count as loaded units or supply Item/Product details. The FWO fallback does not allocate quantities between partial stages or Freight Orders: if an FWO is split between several FO, its full planned goods are included in each FO that uses the fallback. **ForwardingOrderNos** lists the FWO numbers from loaded Unit assignments in LU mode, or from assigned stages in FWO mode.

The report produces **one BoL per Freight Order**. In both cargo modes, it uses the **first stage of the whole Freight Order** for the stage-based header details and transport conditions. The order is the same as in the Freight Order stage list: **Priority**, then **Forwarding Order No.**, then **Line No.** The stage is selected from all stages assigned to the FO, even if it has no loaded-unit rows of its own. In LU mode, the loaded Unit rows identify the physical cargo; their individual FWO/stage references do not determine its transport profile. Each referenced logistic unit must exist.

For example, if the first stage has container A and later stages have containers A and B, the BoL includes A and B once. This also applies when the units are assigned through different FWOs in the same FO. Differences between their stage addresses, dates, and selected profiles do not split the BoL or prevent printing. For a multistage journey, the document describes the cargo under the first stage's transport conditions; it does not combine the classifications of later sea or road stages.

Before printing, enter **Bill of Lading No.** on the Freight Order. All number fields in the Word layout use that value through **Header/BillofLadingNo**, without a fixed prefix or automatic generation from the FO number. An empty Bill of Lading No. stops the report before its cargo is collected. **BOL Date** uses **Header/DocumentDate**, taken from the Freight Order **Document Date**. **DELIVERY DATE/WINDOW** uses the Freight Order **Unloading Date Time** and the duration **Unloading Window**, displayed as date/time / duration through **Header/DeliveryDateText**. A defined date/time with a zero window displays only the date/time, without **/ 0 min**. A nonzero window without a date displays its duration only; both values empty/zero leave the field blank.

For a custom layout, **Header/LoadDateAndTime** supplies the Freight Order **Loading Date Time** and **Loading Window**, using the same formatting rules as the delivery value. A zero window displays only the date/time, without a slash or zero duration. A nonzero window is appended after ` / ` when the date/time is set; without a date/time, only the duration is displayed. Both values empty/zero leave the field blank. Map this field above **DELIVERY DATE/WINDOW** to show the loading schedule. The dataset field name and existing Word bindings are preserved. The bundled Word layout does not display it automatically.

In LU mode, the report reads the full Item/Product contents of the loaded units, including their nested units, into the temporary **TMA BOL Cargo Buffer**, printed through **CargoLine**. It does not limit quantities to matching FWO goods or allocate a fraction using source-document link quantities. In FWO mode, the same buffer receives the saved Item/Product identity, descriptions, quantities and UOM from the assigned FWO lines. In both modes, repeated goods are consolidated by source type, item or product number, variant, quantity UOM, selected stage transport profile, and package. Different quantity UOMs remain separate rows.

The report uses **Transport Profile Code selected on the first stage** for every included Item/Product row in both LU and FWO modes. Printing does not select another profile from cargo assignments or use their Variant/UOM, Default, Priority, or effective dates. A selected stage profile therefore supplies dangerous-goods data even when the goods have no Transport Profile Assignments. A blank **Package Type Code** on the first stage allows the selected profile to supply its package code; the existing FWO rule still uses the quantity UOM description for printed package text.

The selected profile must exist, be unblocked and reviewed, match the first stage's Mode of Transport, Transport Regulation Code and any nonblank Package Type Code, and pass the existing transport-data and hazardous-material checks. If the first stage has no selected profile, cargo rows retain their ordinary descriptions and blank transport-profile and hazardous-material fields, even if cargo assignments exist. Mixed loads use the same selected stage profile for all cargo rows; review that selection before printing.

For regulated cargo, the report uses the current profile's Transport ID, Proper Shipping Name, technical names, ERG guide, primary/subsidiary hazard classes, Packing Group, STCC, additional shipping description, and marks. Detail descriptions marked **Print on Transport Document** are included except for Conditional Rules and Subsidiary Hazard Class rows; subsidiary class values form part of the regulated classification separately, regardless of that flag. **TMS Setup > Emergency Response** supplies the provider and primary/international phones printed by the bundled layout. Contact and contract ID are available in the dataset for custom layouts; Emergency Response Notes are not included.

A blank hazardous-material value when the stage has no selected profile is not an automatic **Not Regulated** determination. The report also does not perform the full temperature and Conditional equipment confirmation checks. Run **Check Vehicle Unit Type** separately before issuing the document. See [Transport Profiles and Dangerous Goods](transportprofiles.md) for profile setup, review, assignments, and Stage selection. Preview and retain the issued document when a historical record is needed: later printing reads current master data.

In LU mode, links on the included unit contents provide document references only. An exact link to an FWO line is followed to that line's source document; matching unrelated FWO goods does not add references. In FWO mode, each included FWO line supplies its source-document fields directly. The Header dataset exposes **SourceDocumentNos** for distinct internal business-document numbers and **ExternalDocumentNo** for their customer PO references (**External Document No.** on sales documents). The Word **PO#** field uses ExternalDocumentNo; an empty external number is not replaced with the internal document number. SourceDocumentNos is available in XML Mapping for a separate document-reference field. **PRODUCT(S) SHIPPED** lists descriptions of unique included goods. The adjacent area is split into **PRODUCT CODE** and **CUSTOMER CODE**. **PRODUCT CODE** displays the unique internal Item/Product **No.** values through **Header/CustomerProductCodes**. **CUSTOMER CODE** displays distinct nonempty **Item Reference No.** values from the exact linked sales or purchase document lines through **Header/CustomerCodes**, joined with a semicolon. This preserves the customer- or vendor-specific reference saved in each source line, including supported posted documents and source lines resolved through warehouse documents. No reference is looked up from the item card or substituted from the internal item number. The per-cargo references remain available as **CargoLine/CustomerItemReference**. **CargoLine/TariffNo** exposes **Item.Tariff No.** from the item card; the existing **HTSCode** column retains the same value. For TMS Product rows, these fields remain blank because Product has no corresponding tariff field. Missing document links leave these references empty and do not exclude the goods.

Repeated assignments of the same unit are counted once across the whole Freight Order, including different FWOs and stages. If both a container and its nested pallet are loaded, the container is the outer counting unit and its goods are not counted again through the pallet. Their assigned stages are not compared for header or profile conflicts. An unassigned parent container is never added just because a loaded pallet belongs to it.

In LU mode, the default **No. of Packages** text counts distinct loaded outer units containing each good. A mixed unit can appear in the package count of several goods; its contribution to the total number of physical units is counted once per BoL. For example, a loaded container with three nested pallets counts as one package, while three separately loaded pallets count as three. The package description continues to come from the first stage or resolved transport profile.

In FWO mode, the cargo **No. of Packages** column defaults to the FWO line **Quantity** in its line UOM, including fractional quantities, summed within the existing cargo grouping. Whole quantities print without a decimal suffix (**1.0** prints as **1**); meaningful fractions are retained with up to five decimal places. It does not use **Quantity (Base)**. **Type of Package** shows the standard **Unit of Measure.Description** for the FWO line's **Unit of Measure Code**, rather than the code or the stage/profile package description. A missing UOM record or empty UOM description leaves Type of Package blank. For example, a quantity of **2.5** with a UOM described as **Drums** prints **2.5 / Drums**. The total package count remains blank because the actual number of physical LU is unknown and quantities in different UOM are not added into a package total.

In LU mode, cargo **Net Weight** and **Gross Weight** are separate sums of the saved weights on the included LU Item/Product content rows. With Logistic Units 28.6, item UOM **Weight (Net Weight)** supplies the product weight and **Gross Weight** supplies the weight including its packaging. For example, **10 DRUM** with **410 LB net** and **445 LB gross** per drum produces **4,100 LB Net Weight** and **4,450 LB Gross Weight**. The LU content row already holds the weight for its whole quantity, so the report does not multiply it again. The tare of an outer pallet or container is not added to the product weights. Volume also comes from the included content rows. Choose the output weight UOM on the request page.

In FWO mode, cargo weights and volume use the saved FWO line **Gross Weight (base)**, **Net Weight (base)** and **Volume (base)** totals, including manual corrections. They are already calculated for the whole line quantity and are not multiplied by quantity again. They describe the full planned FWO cargo, without a confirmed LU tare or packed-container mass. The same report weight-UOM conversion applies.

Printing uses the saved LU content weights; it does not recalculate them from current Item/UOM settings or repair historical rows. Updating a UOM or upgrading Logistic Units therefore does not change an existing LU whose saved net and gross weights are equal. Correct the source LU content through the applicable packing workflow before printing when its saved weights do not describe the cargo.

**Seals** collects nonempty seal text only from the loaded outer units that contribute goods to this BoL. It prints each unit's saved **Seals** field unchanged, without unit numbers, and separates values from multiple units with a semicolon and a space (`; `). Seals on nested units or on an unloaded parent are not added. **Carrier Instructions** combines the instructions saved on the selected Freight Order with **Comment Group = Carrier** and **For Bill of Lading** selected, including additional instruction types within that group. Both conditions are required. Instructions copied from an FWO are included on the same basis as other saved FO instructions; their source FWO is not additionally filtered by the loaded Unit rows. Printing uses the saved FO instruction text and does not refresh it from the FWO or master instruction.

**Header/LogisticUnitComments** is available in the dataset for custom layouts. It collects the saved **Comment** of each loaded outer unit that contributes goods, skips empty comments, and separates the values with a line break. Existing line breaks within a comment are preserved. Repeated assignments or multiple goods in the same unit do not repeat its comment; identical comments from different units remain separate. Comments of nested units, unloaded parents, and units without printable goods are excluded. The bundled Word layout does not display this field. The Freight Order's **Comment** continues to supply **General Instructions** separately.

In FWO mode, **Seals**, **Header/LogisticUnitComments** and **Header/TransportUnitNos** remain blank. Carrier and general instructions still come from the Freight Order.

**CONSIGNOR** prints **Header/ConsignorAddressText** in compact lines: shipper name, street address, then city/region/postcode/country. When the first stage has a pickup street, its saved pickup address is used, with a distinct pickup-site name if supplied. Otherwise, the report uses the corresponding Company Information, Customer, Vendor or Contact postal address. A saved Shipper Address that differs from the current master address is retained without mixing in the master's locality; if the master is unavailable, the saved shipper street lines are used. Empty parts are skipped, and the full country name is used when available. If a saved pickup postcode is missing, the report takes it from its FWO only when the other saved pickup-address fields and shipper identity match. These fallbacks do not modify the stage. Address text in **CONSIGNOR**, **CONSIGNEE** and **REMIT FREIGHT BILL TO** is aligned with its heading.

**REMIT FREIGHT BILL TO** uses the independent **Header/RemitFreightBillTo** text field. The base report leaves it blank and clears it for each Bill of Lading. A report extension can fill the protected **RemitFreightBillTo** variable in **Header.OnAfterAfterGetRecord**, using the protected **FreightOrder** record for its conditions. The Word layout prints that value, including line breaks. It does not substitute the shipper or consignee address.

**FREIGHT CHARGES:** uses **Header/FreightChargesPaymentTerm**. The base report leaves this text blank and clears it for each Bill of Lading. A report extension can fill the protected **FreightChargesPaymentTerm** variable in **Header.OnAfterAfterGetRecord**, for example with **Prepaid**, **Collect**, or another payment term. The bundled Word layout contains no fixed payment choices or underscore placeholders after the label.

The separate **HTS Comm. Code:** line after the package and weight totals uses **Header/FreightOrderHTSCode**. The base report leaves this text blank and clears it for each Bill of Lading. A report extension can fill the protected **FreightOrderHTSCode** variable in **Header.OnAfterAfterGetRecord**, using the current **FreightOrder**. This field applies to the whole Freight Order. The cargo table continues to print the per-item **CargoLine/HTSCode** values from **Item.Tariff No.** independently.

**General Instructions** in the text after **DELIVERY NOTES:** comes directly from the **Comment** box on the Freight Order through **Header/GeneralInstructions**. Its line breaks are preserved. The separate **DeliveryNotes** dataset column remains available for consignee notes from the selected first stage.

The copied Word layout contains a repeating cargo row, dynamic page numbers, and a repeating cargo-table heading. Actual-carrier confirmation, signatures, and VGM remain manual fields where the current data model does not provide a confirmed value. The displayed gross weight is a calculated value, not a certification of verified container mass.

The final **Cargo gross weight for vehicle/container no.** line uses **Header/VehicleRegistrationNo**, matching **VEHICLE/CAR NO** in the upper header. Both positions read the protected **VehicleRegistrationNo** variable, initially filled from the saved Freight Order **Vehicle Registration No.** A report extension can override it in **modify(Header).OnAfterAfterGetRecord()**, and both positions will print the replacement value. **Header/TransportUnitNos** remains available for custom layouts and contains the distinct numbers of the loaded outer units whose cargo is included in this BoL.

The weight in this final line uses **Header/TotalGrossWeightText**, matching **TOTAL WEIGHT**, with the numeric base-unit value available as **Header/TotalGrossWeightBase**. It sums the saved gross weights of the included Item/Product lines from the selected cargo source. In LU mode, this includes nested-unit contents without adding LU tare or counting the same content twice; in FWO mode, it is the planned cargo gross weight. **Header/TransportGrossWeightText** and **Header/TransportGrossWeightBase** remain available for custom layouts that need the full mass including LU tare; the bundled layout does not print this total. That separate total sums the saved **Gross Weight (Base)** of each loaded outer unit containing included goods, once per unit, and already includes cargo and outer/nested LU tare. A separately loaded nested unit does not add its weight again when its loaded parent is counted; unloaded parents and separately loaded empty units are excluded. The report reads saved weights without recalculating them. For example, **4,450 LB** cargo gross plus **2,000 LB** LU tare prints **4,450 LB** in both the final line and **TOTAL WEIGHT**, while the separate transport total remains **6,450 LB** in the dataset. Both formatted totals use the weight UOM selected on the request page. In FWO mode, **TransportGrossWeightText** is blank because LU tare is unknown; its existing numeric dataset column remains zero and must not be interpreted as a confirmed transport weight.

A missing first stage, an invalid selected profile, and conditional hazardous-material classifications must be resolved before printing in either mode. The report does not compare the header or transport-profile contexts of other assigned stages. Negative quantities are rejected; Item/Product lines with zero base quantity are skipped. If any loaded Unit row exists but its units provide no printable goods, the report stops; empty or zero-quantity units never trigger the FWO fallback. In FWO mode, an absence of printable Item/Product goods also stops printing, including FWOs containing only planned Unit Type lines.

### Cargo-line extension event

External extensions can subscribe to report 70611595 **OnAfterGetCargoLine** to customize the current cargo row before its dataset columns are emitted. This integration event runs in both LU and FWO modes, at the end of **CargoLine.OnAfterGetRecord**, immediately after the report selects the next temporary cargo record.

| Parameter | Meaning |
| --- | --- |
| `var TempCargo: Record "TMA BOL Cargo Buffer" temporary` | Current aggregated cargo row; direct field assignments affect this row's dataset values. |
| `FreightOrder: Record "TMA Freight Order Header"` | Current FO context, passed by value. |
| `HeaderStage: Record "TMA Forwarding Order Stage" temporary` | First stage of the whole FO, passed by value. This is the header and transport-condition context, not an original stage for this cargo row; one row can combine goods from several FWOs. |
| `HasLoadedUnits: Boolean` | `true` for LU cargo; `false` for the FWO fallback. |
| `LogisticUnitNos: List of [Code[20]]` | Distinct loaded outer LU numbers contributing positive goods to this cargo row. Empty in FWO mode. |

**LogisticUnitNos** contains only the roots contributing to the current aggregated row, not every LU in the FO. Nested units are represented by their loaded outer root: if a container and its child pallet are both loaded, the list contains the container; if only the child is loaded, it contains that child. No single LU is chosen as the owner of an aggregated row, and the buffer's **Unit No.** and **Container No.** remain unfilled.

Treat the list as read-only context. The report passes a separate copy, so subscriber edits cannot change the collector's stored associations or a later cargo row's list. AL lists are reference types, however, so other subscribers to the same event invocation can see edits to this copy. Read LU fields using a separate **TMAC Unit** record; do not modify the LU records.

Assign the required fields directly; **Modify is not needed**. Keep **Entry No.**, filters, current key and record position unchanged. Do not call **Find/Next**, insert or delete buffer rows, or write to the FO/stage context. The event customizes an already selected row, rather than rebuilding the cargo collection.

Use the fields behind the intended printed columns:

| Printed column | Current-row field |
| --- | --- |
| Shipping Description (`ShippingDescription`) | `TempCargo."Article Description"` |
| Type of Package (`PackageDescription`) | `TempCargo."Package Description"` |
| No. of Packages (`PackageCount`), both modes | `TempCargo."Package Count Text"` (`Text[250]`) |

**Package Count Text** is prepared after all cargo aggregation: FWO rows use the formatted `Quantity` with zero to five decimal places, while LU rows use the integer `No. of Packages` when `Package Count Known` is true, otherwise blank. The dataset prints this text directly. A subscriber can replace it with any text up to 250 characters or an empty string. Changing `Quantity`, `No. of Packages` or `Package Count Known` alone in the event does not regenerate the text; set `Package Count Text` explicitly to change the printed column. The underlying numeric counters and header package totals remain separate.

The report does not recalculate the transport profile, DG fields or derived descriptions after this event. Changing `Description` alone does not rebuild `Article Description`. Header totals and aggregated text such as **ProductsShipped** and **CustomerCodes** have already been prepared and are not updated by row changes. Extensions that change quantities or weights must account for this difference between the current row and the existing header totals.

For example, an external subscriber can replace the printed package count and wording for a specific FWO UOM, and read each contributing LU to apply its own presentation rules. The illustrative LU rule below replaces the count with a note when any contributing root has a comment. Assign `''` to `Package Count Text` to leave that column blank. Use an object ID from that extension's own range:

```al
namespace MyCompany.Shipping;

using ExtensionsLab.TMS.LogisticsServiceProvider;
using ExtensionsLab.LogisticUnits.Unit;

codeunit 50100 "My DG BoL Subscriber"
{
    [EventSubscriber(ObjectType::Report, Report::"TMA DG Bill of Lading", 'OnAfterGetCargoLine', '', false, false)]
    local procedure CustomizeCargoLine(var TempCargo: Record "TMA BOL Cargo Buffer" temporary; FreightOrder: Record "TMA Freight Order Header"; HeaderStage: Record "TMA Forwarding Order Stage" temporary; HasLoadedUnits: Boolean; LogisticUnitNos: List of [Code[20]])
    var
        LogisticUnit: Record "TMAC Unit";
        LogisticUnitNo: Code[20];
    begin
        if not HasLoadedUnits and (TempCargo."Unit of Measure Code" = 'DRUM') then begin
            TempCargo."Package Count Text" := 'See packing list';
            TempCargo."Package Description" := 'Drums';
        end;

        foreach LogisticUnitNo in LogisticUnitNos do
            if LogisticUnit.Get(LogisticUnitNo) then
                if LogisticUnit.Comment <> '' then
                    TempCargo."Package Count Text" := 'See logistic unit comments';
    end;
}
```

## Packing List by Container

Select **Freight Order - Packing List** in report selections. **TMA Packing List by Container** is initially registered in Sequence 3. Keep only this row if it is the only packing-list form required, then use **Packing List** on the Freight Order. Configure **Use for Email Attachment** for the rows to include when emailing. The former separate By Container actions are hidden.

The report follows the delivery packing-list structure: shipper, customer/order party, forwarding agent, pickup and delivery addresses, Freight Order and source-document references, vessel/vehicle and voyage, carrier/SCAC, booking number, container number, type, seals, destination, and planned loading date/time. The forwarding agent is the company in Business Central. The saved shipper and order-party addresses are kept separate from the full pickup and delivery addresses.

The default Word layout follows the supplied delivery packing-list appearance: A4 portrait, compact Arial text, thin black rules, a dark-gray cargo heading, and a logo/title band. The logo comes from **Company Information > Picture**; an empty picture leaves that space blank. The first-page number is in the upper-right metadata block. Continuation pages retain container identification and page numbering.

Load the actual containers into **Freight Order Content** first. Only **Type = Unit** entries provide the document roots; planned **Unit Type** entries and unrelated FWO goods are excluded. The report reads each loaded outer unit and its nested Item/Product contents. An explicitly loaded child is not counted again when its parent is also loaded. An unloaded parent is never added. Container number means the outer LU **No.**; there is no separate container classification in the current model, so other loaded outer LU types also receive separate documents.

Each container starts a new document and page. A large product or lot list continues on further pages with page numbering; it is not truncated to force one physical page. Repeated Freight Order Content assignments of the same physical unit, including different stages or FWOs, produce one container document and never multiply its cargo. Different route-stage From/To locations do not prevent printing.

Freight Order fields supply the trip and carrier details. For each container, the report gathers the actual assignments of that outer LU and its included child units. Identical customer, shipper, pickup, and delivery address blocks print once; different complete blocks are separated by a blank line in the same header field. Destination countries and FWO numbers are distinct lists. A missing stage link falls back to the corresponding FWO's saved address blocks when available. If no stage or FWO data is available, those header fields stay blank and the physical cargo still prints. No assignments or addresses are changed by printing. Physical hierarchy errors, missing actual units, and invalid cargo quantities must still be corrected.

Within a container, goods are grouped by Item/Product, variant, saved description, source-document line, and quantity UOM, then by lot. Exact LU-content links identify the source documents and lot quantities. The report selects one primary document family to avoid counting the same stock again through its warehouse or invoice links. When supported links cannot account unambiguously for the physical quantity, the unassigned portion prints as **Not specified / unresolved**. The report never distributes whole-order tracking quantities across containers by guesswork.

For linked sales orders, shipments, and invoices, quantity is converted using the exact source line's saved **Qty. per Unit of Measure** and UOM. Without a usable sales line, the report retains the LU content quantity/UOM. Container quantity totals are separate for each UOM, such as **80 DRUM**; quantities in different units are never added together. These are cargo quantities, not counts of physical pallets or containers.

**HS / Tariff Code** uses **Item.Tariff No.**. It stays blank for TMS Products or items without a tariff number. The report does not infer or shorten the tariff classification.

The cargo area follows the sample's five columns: **Reference**, **Number of PKGS**, **Description**, **Quantity**, and **CBM**. Number of PKGS shows the product quantity with its UOM, such as **80 DRUM**. Item descriptions, lot numbers, and gross-weight details appear within Description; lot quantities appear in Quantity. Cargo gross weight comes from the included LU Item/Product rows, excluding outer-container tare. Lot gross weights are proportional to each lot's base quantity, with the final portion retaining the exact remainder. Select the weight UOM on the request page. Weights and CBM display three decimals. **CBM** always means cubic metres: the configured Logistic Units base volume is converted using its metric conversion factor. Existing dataset columns for volume in the configured base UOM remain available for custom layouts.

The bundled Packing List layout continues to show cargo **gross weight only**; it has no separate net-weight, tare, or pallet-breakdown fields. Logistic Units 28.6 supports separate saved net and gross weights, but historical LU content can retain the earlier equal values. Printing reads the saved content and does not repair those historical weights from current Item/UOM settings. The Word layout provides a blank signature area; it does not copy the example's certification or signature. The Freight Order **Comment** appears as instructions. The loaded outer logistic unit's **Comment** is available directly as **Container/ContainerComment** and prints once in **Description**, after the product details and before the package totals. Use it for free-text packing details, such as pallet information. Line breaks are retained. Comments of nested pallets or other child units are not substituted for the container comment. This packing list does not require a dangerous-goods profile or a Bill of Lading number.

## Good to know

- Report output depends on the data available on the source document.
- Email actions depend on Business Central email setup.
- If the wrong report opens or several forms print, review the rows under the document Usage. **Set default reports** adds missing built-in alternatives; it does not choose a single form or overwrite existing selections.
- Instruction visibility depends on the active report layout. See [Instructions](instructions.md) for applicability and layout details.

## Troubleshooting

| Problem | What to check |
|---|---|
| Report action is missing | Check permissions, document status, and page actions. |
| Report prints blank sections | Review carrier, party, cargo, stage, and settlement data. |
| Email does not send | Check Business Central email setup and recipient address. |
| Wrong report layout is used | Review report selections and custom report extensions. |

## Related

- [Forwarding Order](forwardingorder.md)
- [Freight Order](freightorder.md)
- [Stage Entries](stageentries.md)
- [Settlement](settlement.md)
- [TMS Setup](setup.md)
- [Instructions](instructions.md)
- [Transport Profiles and Dangerous Goods](transportprofiles.md)
