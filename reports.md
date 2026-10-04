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

The goods table is a **planned shipment assignment**. It reads positive **Item** and **Product** lines from the Forwarding Orders whose stages are assigned to the selected FO and delivery Drop. It works before warehouse picking, logistic-unit/container creation or lot selection. Loaded LU contents, inventory tracking and reservations do not determine its goods.

One FWO is counted once within a Drop even if several of its stages lead to that Drop. Matching goods from different FWO can be combined only when their item type, number, variant, descriptions, customer reference and units of measure agree. The same FWO assigned to different Drops contributes its full planned goods to each Drop. Item-level allocation between partial stages is not inferred from reserved weight or volume.

Item codes, descriptions, quantities/UOM and **Customer Item No.** come from the saved FWO lines. The customer code is **Item Reference No.**; legacy FWO lines without that snapshot print a blank reference. Gross weight and volume use the line's saved base-unit totals, including any manual corrections. These are planning values, not weighed or packed container totals. SO/PO references and unlabeled Shipper/Consignee Notes remain available.

For a Sales Order source, **PO Number** is **Sales Header.External Document No.**. If the order has been deleted after posting, it is recovered from Sales Shipment Lines matched by the original order number and line number. A direct Sales Shipment source uses its shipment header's external document number. Multiple nonblank PO values for one source are listed without duplicates; a missing PO is left blank.

Drop gross-weight and volume totals are calculated from those planned FWO goods. The bundled layout does not print **Lot Nos.** or Expected Cost. Existing lot columns remain in the dataset for compatibility but are empty in Load Summary. **Shippers / Pick-Pack Instructions** follows the Drop goods, and **Carrier instructions** follows all Drops. See [Instructions](instructions.md#instructions-in-reports) for selection and grouping rules. The published **Header/Groups/DropItems** structure and its existing column names/types remain unchanged. Packing List by Container and DG Bill of Lading continue to use their actual loaded-cargo logic.

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

Before printing, use **Load Logistic Units to Freight Order** to load the actual logistic units. The report takes cargo only from concrete **Type = Unit** rows in **Freight Order Content** for the selected Freight Order. Planned **Unit Type** rows are ignored, and goods on FWO lines that are not inside the loaded units are excluded. If no concrete units have been loaded, printing stops with an explanatory error.

The report produces **one BoL per Freight Order**, including loaded goods from all of its FWOs and stages. It uses the **first stage of the whole Freight Order** for the stage-based header details and transport conditions. The order is the same as in the Freight Order stage list: **Priority**, then **Forwarding Order No.**, then **Line No.** The stage is selected from all stages assigned to the FO, even if it has no loaded-unit rows of its own. The loaded Unit rows identify the physical cargo; their individual FWO/stage references do not determine its transport profile. Each referenced logistic unit must exist.

For example, if the first stage has container A and later stages have containers A and B, the BoL includes A and B once. This also applies when the units are assigned through different FWOs in the same FO. Differences between their stage addresses, dates, and selected profiles do not split the BoL or prevent printing. For a multistage journey, the document describes the cargo under the first stage's transport conditions; it does not combine the classifications of later sea or road stages.

Before printing, enter **Bill of Lading No.** on the Freight Order. All number fields in the Word layout use that value through **Header/BillofLadingNo**, without a fixed prefix or automatic generation from the FO number. An empty Bill of Lading No. stops the report before its cargo is collected. **BOL Date** uses **Header/DocumentDate**, taken from the Freight Order **Document Date**. **DELIVERY DATE/WINDOW** uses the Freight Order **Unloading Date Time** and the duration **Unloading Window**, displayed as date/time / duration through **Header/DeliveryDateText**. A defined date/time with a zero window displays **0 min**; a window without a date displays its duration only; both values empty/zero leave the field blank.

The report reads the full Item/Product contents of the loaded units, including their nested units, into the temporary **TMA BOL Cargo Buffer**, printed through **CargoLine**. It does not limit quantities to matching FWO goods or allocate a fraction using source-document link quantities. Repeated goods are consolidated by source type, item or product number, variant, quantity UOM, resolved transport profile, and package. Different quantity UOMs remain separate rows.

A **Transport Profile is selected separately for each Item/Product, variant, and UOM** from its profile assignments. The first stage supplies **Mode of Transport**, **Transport Regulation Code**, **Package Type Code**, **Vehicle Unit Type Code**, and the assignment date: its start date, otherwise its FWO document date, otherwise the work date. The profile already selected in **Stage.Transport Profile Code** is not used by this report. A blank package on the first stage allows the selected profile to supply its package. The existing variant/UOM fallback determines the first group with eligible profiles. Within that group, **Default** assignments rank first, then the higher **Priority**. A unique best profile is used; equally ranked best profiles cause an error asking the user to resolve the assignments.

If relevant assignments exist but none is eligible for the first stage's conditions and date, printing stops with an error. If the goods have no relevant assignments at all, their cargo row uses an ordinary description with blank transport-profile and hazardous-material fields. This includes mixed loads: different goods in the same container can receive different profiles matching the same first-stage conditions. A resolved profile must still pass the existing transport-data and hazardous-material checks.

For regulated cargo, the report uses the current profile's Transport ID, Proper Shipping Name, technical names, ERG guide, primary/subsidiary hazard classes, Packing Group, STCC, additional shipping description, and marks. Detail descriptions marked **Print on Transport Document** are included except for Conditional Rules and Subsidiary Hazard Class rows; subsidiary class values form part of the regulated classification separately, regardless of that flag. **TMS Setup > Emergency Response** supplies the provider and primary/international phones printed by the bundled layout. Contact and contract ID are available in the dataset for custom layouts; Emergency Response Notes are not included.

A blank hazardous-material value for unconfigured goods is not an automatic **Not Regulated** determination. The report also does not perform the full temperature and Conditional equipment confirmation checks. Run **Check Vehicle Unit Type** separately before issuing the document. See [Transport Profiles and Dangerous Goods](transportprofiles.md) for profile setup, review, assignments, and the differences between Stage checks and report selection. Preview and retain the issued document when a historical record is needed: later printing reads current master data.

Links on the included unit contents provide document references only. An exact link to an FWO line is followed to that line's source document; matching unrelated FWO goods does not add references. The Header dataset exposes **SourceDocumentNos** for distinct internal business-document numbers and **ExternalDocumentNo** for their customer PO references (**External Document No.** on sales documents). The Word **PO#** field uses ExternalDocumentNo; an empty external number is not replaced with the internal document number. SourceDocumentNos is available in XML Mapping for a separate document-reference field. **PRODUCT(S) SHIPPED** lists descriptions of unique included goods. The adjacent area is split into **PRODUCT CODE** and **CUSTOMER CODE**. **PRODUCT CODE** displays the unique internal Item/Product **No.** values through **Header/CustomerProductCodes**. **CUSTOMER CODE** displays distinct nonempty **Item Reference No.** values from the exact linked sales or purchase document lines through **Header/CustomerCodes**, joined with a semicolon. This preserves the customer- or vendor-specific reference saved in each source line, including supported posted documents and source lines resolved through warehouse documents. No reference is looked up from the item card or substituted from the internal item number. The per-cargo references remain available as **CargoLine/CustomerItemReference**. **CargoLine/TariffNo** exposes **Item.Tariff No.** from the item card; the existing **HTSCode** column retains the same value. For TMS Product rows, these fields remain blank because Product has no corresponding tariff field. Missing document links leave these references empty and do not exclude the loaded goods.

Repeated assignments of the same unit are counted once across the whole Freight Order, including different FWOs and stages. If both a container and its nested pallet are loaded, the container is the outer counting unit and its goods are not counted again through the pallet. Their assigned stages are not compared for header or profile conflicts. An unassigned parent container is never added just because a loaded pallet belongs to it.

The report counts distinct loaded outer units containing each good. A mixed unit can appear in the package count of several goods; its contribution to the total number of physical units is counted once per BoL. For example, a loaded container with three nested pallets counts as one package, while three separately loaded pallets count as three. Cargo net/gross weights and volume are calculated from the included Item/Product cargo rows; the tare of an outer transport unit is not allocated across its goods. Choose the output weight UOM on the request page.

**Seals** collects seal text only from the loaded outer units that contribute goods to this BoL, retaining each unit number. Seals on nested units or on an unloaded parent are not added. **Carrier Instructions** combines the instructions saved on the selected Freight Order with **Comment Group = Carrier** and **For Bill of Lading** selected, including additional instruction types within that group. Both conditions are required. Instructions copied from an FWO are included on the same basis as other saved FO instructions; their source FWO is not additionally filtered by the loaded Unit rows. Printing uses the saved FO instruction text and does not refresh it from the FWO or master instruction.

Both **CONSIGNOR** and **REMIT FREIGHT BILL TO** print the same **Header/ConsignorAddressText** in compact lines: shipper name, street address, then city/region/postcode/country. When the first stage has a pickup street, its saved pickup address is used, with a distinct pickup-site name if supplied. Otherwise, the report uses the corresponding Company Information, Customer, Vendor or Contact postal address. A saved Shipper Address that differs from the current master address is retained without mixing in the master's locality; if the master is unavailable, the saved shipper street lines are used. Empty parts are skipped, and the full country name is used when available. If a saved pickup postcode is missing, the report takes it from its FWO only when the other saved pickup-address fields and shipper identity match. These fallbacks do not modify the stage. Address text in **CONSIGNOR**, **CONSIGNEE** and **REMIT FREIGHT BILL TO** is aligned with its heading.

**General Instructions** in the text after **DELIVERY NOTES:** comes directly from the **Comment** box on the Freight Order through **Header/GeneralInstructions**. Its line breaks are preserved. The separate **DeliveryNotes** dataset column remains available for consignee notes from the selected first stage.

The copied Word layout contains a repeating cargo row, dynamic page numbers, and a repeating cargo-table heading. Freight payment basis, actual-carrier confirmation, signatures, and VGM remain manual fields where the current data model does not provide a confirmed value. The displayed cargo gross weight is a calculated value, not a certification of verified container mass. The final **Cargo gross weight for vehicle/container no.** line prints **Header/TransportUnitNos**: the distinct **No.** values of the loaded outer units whose cargo is included in this BoL. A loaded container contributes its own number; nested pallets and unloaded parents are not added. These are LU identifiers, not a separate container-registration classification. This field has no vehicle-registration fallback. The separate **VEHICLE/CAR NO** field in the upper header continues to use the vehicle registration number.

Missing loaded logistic units, a missing first stage, unresolved profile assignments, and conditional hazardous-material classifications must be resolved before printing. The report does not compare the header or transport-profile contexts of other assigned stages. Negative quantities are rejected; unit-content lines with zero base quantity are skipped. If the loaded units provide no printable goods, the report stops instead of using unrelated FWO content.

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

Pallet breakdown, net weight, and tare are omitted because the current data does not reliably support the sample's separate values. In particular, existing item LU creation can copy the same source weight to both gross and net fields. The Word layout provides a blank signature area; it does not copy the example's certification or signature. The Freight Order **Comment** appears as instructions. The loaded outer logistic unit's **Comment** is available directly as **Container/ContainerComment** and prints once in **Description**, after the product details and before the package totals. Use it for free-text packing details, such as pallet information. Line breaks are retained. Comments of nested pallets or other child units are not substituted for the container comment. This packing list does not require a dangerous-goods profile or a Bill of Lading number.

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
