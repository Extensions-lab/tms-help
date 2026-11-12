---
title: "Transport Profiles and Dangerous Goods"
description: "Create and review cargo transport profiles, assign them to Items and Products, select them on Stages, and check equipment and dangerous-goods documents."
---

# Transport Profiles and Dangerous Goods

Use **Transport Profiles** to record the conditions under which cargo can travel, make those conditions available to planners, and check the selected transport equipment. Profiles support dangerous goods, nonregulated cargo, and cargo with special temperature or handling requirements.

Start with [setup and review](#how-to-configure-transport-profiles), then follow [daily planning](#how-to-use-transport-profiles-in-daily-planning). Use [Stage selection rules](#selecting-a-profile-on-a-stage) when a profile is missing from the lookup.

## Why use Transport Profiles

A cargo description alone does not tell a planner which regulation, packaging, temperature, or equipment is appropriate. The same Item can need different transport conditions for different modes and shipment arrangements. Missing or inconsistent information can lead to unsuitable equipment, incorrect carrier documents, rejected loads, and unsafe handling.

A profile keeps the classification, source information, and handling requirements together. Assignments connect that reviewed scenario to the goods, and Stage selection connects it to a particular leg of the journey.

| Business need | How to use the feature |
|---|---|
| One Item travels by road, rail, or air | Create separate profiles for each Mode of Transport and Transport Regulation. |
| The same cargo travels in different packages or temperature conditions | Create separate scenarios and use Item Variant/UOM assignments and the Stage Package Type to select the appropriate one. |
| Equipment must meet cargo requirements | Maintain explicit Vehicle Unit Type Rules and check the selected equipment before dispatch. |
| Dangerous-goods information changes | Record SDS and source references, review changes, and replace profiles whose transport context has changed. |
| Several goods travel together | Check whether configured direct cargo lines have a common Stage profile; review unconfigured goods explicitly. |
| Carrier documents need dangerous-goods information | Select the reviewed profile on the first Freight Order Stage and use **Dangerous Goods Bill of Lading**. |

The responsible specialist must determine the actual classification from the current SDS, applicable regulation, shipment conditions, and carrier requirements. **Transport Data Reviewed** records a company review; it is not regulatory certification. TMS does not calculate a legal classification from an Item description or certify a particular vehicle.

## How Transport Profiles work

| Concept | What it means |
|---|---|
| **Transport Regulation** | The applicable framework, such as ADR, DOT, RID, IMDG, IATA, or TDG. The selected regulation must be Active. |
| **Transport Profile** | One complete scenario: mode, regulation, package, bulk status, shipping state, temperature, classification, and handling requirements. |
| **Transport Profile Assignment** | Allows an Item or TMS Product to use a profile, optionally for a Variant, UOM, and date range. One source can have several assignments. |
| **Package Type** | How the cargo is presented or packed. It is required on the profile and is an optional filter on the Stage. |
| **Vehicle Unit Type** | An existing Logistic Units type used to describe transport equipment. Rules connect this type to a profile. It is separate from Package Type and the individual Vehicle No. |
| **Stage Profile** | A template that creates transportation legs and their defaults. It is different from a Transport Profile. |
| **Forwarding Order Stage** | The operational leg where the planner selects a Transport Profile. It can remain blank during preparation; the explicit Freight Order equipment check requires every assigned Stage to have a profile. |
| **Freight Order** | The execution document where the planner selects equipment and checks the related Stage profiles. |
| **Emergency Response** | Company-level contact information maintained in TMS Setup and used by the dangerous-goods report. |

Profiles are live master data. Changing them affects later checks and report generation for existing work. There is no saved profile revision, classification snapshot, or persistent compliance result on the Stage.

## Before you start

1. Identify who maintains the master data, who reviews classification, and who plans shipments.
2. Give the responsible administrator **TMS Administrator** access. Planners normally use **TMS User**; see [Permissions](#permissions).
3. Obtain the current SDS, applicable transport information, and carrier/operating requirements for each scenario.
4. Prepare Transport Regulations, Modes of Transport, Package Types, Vehicle Unit Types, and the relevant Items or TMS Products.
5. Agree a profile code convention, for example cargo + mode + package. Use separate codes for materially different scenarios.
6. Test the complete path in a sandbox: profile, review, cargo assignment, Stage selection, equipment check, and report preview.

## How to configure Transport Profiles

### Open the setup pages

Search for **Transport Profiles**, or use **My Application > TMS > Setup > Cargo & Transport Profiles** on a supported Role Center. On the TMA Dispatcher Role Center, use **My Application > Setup > Cargo & Transport Profiles**.

The group contains **Transport Regulations**, **Modes of Transport**, **Package Types**, **Freight Classes**, **Transport Profiles**, **Vehicle Unit Type Rules**, and **Transport Profile Assignments**. Freight Class is a separate classification and is not required to review a Transport Profile.

### 1. Prepare reference data

1. Open **Transport Regulations** and confirm that each regulation you intend to use is **Active**.
2. Open **Modes of Transport**. Set **Default Transport Regulation** only when that default is appropriate for the work normally done with the mode.
3. Review **Package Types** and the existing [Logistic Unit Types](logisticunittype.md) used as vehicle equipment.
4. If you use Stage Profiles, set the Mode, Vehicle Unit Type, Package Type, and Transport Regulation defaults on the relevant template lines. These flow to newly created operational Stages; the actual Transport Profile is selected on the Stage.

### 2. Create or copy a profile

1. Open **Transport Profiles** and choose **New**, or open a similar profile and choose **Copy Profile**.
2. Enter **Code** and **Description**.
3. Complete the **Transport Context** fields for this one scenario.
4. Fill the source, classification, temperature, and operational sections below.
5. Add repeating information in **Details** and equipment restrictions in **Vehicle Unit Type Rules**.
6. Review the finished profile before making it available to planners.

**Copy Profile** copies the header, Details, and Vehicle Unit Type Rules. The copy is unblocked and **Transport Data Reviewed** is cleared. Cargo assignments are not copied. Leave the copy unreviewed while preparing it; use **Block** when an explicit maintenance hold is needed.

| Card section | What to enter |
|---|---|
| **General** | Code, Description, review status, Blocked, Notes, and a replacement profile when retiring the scenario. |
| **Transport Context** | Mode of Transport Code, Transport Regulation Code, Package Type Code, Bulk Status, Shipping State, and Temperature Condition. |
| **SDS and Source** | SDS No., SDS Revision Date, Source Reference, and Source Notes. Record which information the reviewer used. |
| **Dangerous Goods Classification** | Hazardous Material Status, Transport ID, Proper Shipping Name, technical names, Primary Hazard Class, Packing Group, relevant pollutant/substance/limited-quantity statuses, RQ, ERG, and marks. |
| **Temperature Requirements** | Required minimum, target, and maximum temperatures, their Defined flags, Temperature Unit, Heating Requirement, and Insulation Requirement. |
| **Operational Requirements** | Pressure loading/unloading, ventilation, grounding, dangerous-goods equipment, loading/unloading methods, cleanliness, prior-cargo restrictions, and compatibility notes. |

A real temperature of zero is valid. Use the corresponding **Defined** flag to distinguish a defined zero from an unspecified value. For **Temperature Controlled** or **Elevated Temperature**, both minimum and maximum must be defined; any target must be within the range.

SDS fields and Packing Group are not universally mandatory in the software. The reviewer must still complete all information required for the actual cargo and regulation. A successful system check alone does not establish that the source information is complete.

### 3. Complete Details and conditional classification

Use **Details** for Subsidiary Hazard Class, Hazard Label, Special Provision, Incompatible Material, Handling Instruction, Additional Document Text, and Conditional Rule.

Each Detail needs a description. A subsidiary hazard class must differ from the Primary Hazard Class. Adding, editing, deleting, or renaming Details clears the parent profile's review flag.

Use **Print on Transport Document** for the supported descriptive Details that should appear in **Dangerous Goods Bill of Lading**. Subsidiary hazard classes are included in the regulated cargo classification separately. Conditional Rule descriptions are not printed. See [Reports and documents](#reports-and-documents).

**Conditional** classification and **Conditional** equipment rules serve different purposes:

| Where Conditional is used | What it does |
|---|---|
| Profile classification, such as Hazardous Material Status | Requires a valid Detail with **Detail Type = Conditional Rule** for the same **Condition Target**. Standard Stage actions check the rule structure, but do not resolve the shipment's classification. |
| Vehicle Unit Type Rule | Checks any configured temperature limits and asks the user to confirm nonblank Condition Text during an interactive equipment check. |

When the transport scenario is known, prefer separate reviewed profiles with an explicit classification. For example, maintain separate ambient and heated scenarios when they have different requirements. A Conditional hazardous-material profile cannot be used to print DG Bill of Lading until its classification is resolved in the master data.

The structured classification resolver is available to custom calling processes. It supports actual temperature, quantity, package capacity, bulk status, mode, and manual review. The first rule for a target in Sort Order/Line No. order is authoritative. Missing facts or required manual review leave the result unresolved. Numeric rules require an explicit Condition Unit of Measure Code; temperature supports Celsius/Fahrenheit conversion, while quantity and capacity require matching actual and rule units. Standard Stage actions do not invoke this resolver.

### 4. Configure Vehicle Unit Type Rules

On the profile, open **Vehicle Unit Type Rules** and add a row for each permitted equipment type.

| Compatibility Rule | Meaning |
|---|---|
| **Preferred** | Permitted and ranked ahead of ordinary allowed equipment in the Freight Order lookup. |
| **Allowed** | Permitted, subject to any configured temperature limits. |
| **Conditional** | Permitted only after temperature checks and, when Condition Text is filled, explicit user confirmation. |
| **Blocked** | Prohibited for this profile. |

**No rules means no equipment restriction. As soon as one rule exists, the list is closed: every unlisted Vehicle Unit Type is rejected.** Adding only one Blocked row therefore excludes all other types as well. For a restricted profile, enumerate the permitted types explicitly.

**Priority** supplies the equipment proposal order and never overrides a restriction. The current equipment lookup sorts the displayed priority in ascending order within each Compatibility Rule group. Cargo-assignment Priority has a different order: higher values rank first. A Conditional row must have Condition Text, at least one temperature boundary, or both. Use clear, verifiable text, such as a required cleaning record or equipment inspection.

Temperature boundaries apply to **Preferred**, **Allowed**, and **Conditional** rows. Enter the Temperature Unit and Defined flags. The minimum is inclusive; the maximum is inclusive unless **Maximum Temperature Exclusive** is selected. For example, a maximum of 50 with Exclusive selected accepts values below 50 but rejects 50.

**Verified By User ID** and **Verified Date Time** record who last created or changed the rule. They are automatic change stamps, not a separate approval. Rule and assignment edits do not clear **Transport Data Reviewed**, so include them in your own review procedure.

Requirement fields such as Grounding or Dangerous Goods Equipment describe what is needed. They do not inspect individual vehicles or automatically generate the compatibility rules.

### 5. Review and release the profile

The system's review gate is **Transport Data Reviewed**, shown in **General** immediately after **Blocked**. There is no request/approve workflow or second approver built into this feature.

1. Verify the sources, classification, Details, assignments to be created, and equipment rules.
2. Choose **Mark Transport Data as Reviewed**. An authorized user can also select **Transport Data Reviewed** directly; both routes validate the profile.
3. Correct any reported missing or inconsistent values and repeat the review.
4. Ensure **Blocked** is off before creating assignments or selecting the profile on a Stage.

### Transport Profile validation

| Check | Required for review |
|---|---|
| Identity and context | Code, Description, Mode, Transport Regulation, Package Type, Bulk Status, Shipping State, and Temperature Condition. Select an Active regulation during setup. |
| Hazardous Material Status | Must be assessed: Regulated, Not Regulated, or Conditional with a valid matching rule. |
| Regulated cargo | Transport ID Type, Transport ID No., Proper Shipping Name, and a Primary Hazard Class other than Not Applicable. |
| Transport ID format | The selected prefix followed by exactly four digits, for example **UN3257**. Enter the complete value, including the prefix. |
| Temperature | Valid bounds and target; minimum and maximum defined for Temperature Controlled/Elevated Temperature scenarios. |
| Operational assessment | Insulation, Pressure Loading, Pressure Unloading, Ventilation, Grounding, and Dangerous Goods Equipment must each be assessed rather than left Not Assessed. |
| Reportable Quantity | A positive value requires RQ Unit of Measure Code. |
| Conditional classification | Each Conditional hazardous-material, marine-pollutant, hazardous-substance, limited-quantity, or elevated-temperature status requires a valid rule for that target. |
| Details | Every Detail must pass its validation, including description and any conditional-rule requirements. |

Changes to SDS/source, classification, temperature, operational requirements, or Details clear the review flag. Review the changes again before further operational use.

### 6. Assign profiles to Items and TMS Products

Use any of these entry points:

| Starting point | Action |
|---|---|
| **Item Card** | Choose the **Transport Profiles** action for the Item. Assignment indicators are in the **Transportation Management System > Transport Profiles** field group. |
| **Item List** | Select the Item and choose **Transport Profiles**. |
| **Products** | Select the TMS Product and choose **Transport Profiles**. |
| **Transport Profile Card** | Choose **Assignments** to maintain the profile's cargo links. |
| Setup | Open **Transport Profile Assignments** for all sources. |

1. Enter **Source Type** and **Source No.** when they are not supplied by the starting page.
2. For an Item, set **Variant Code** when the scenario is variant-specific. Leave it blank for a general fallback. TMS Products do not have Item Variants.
3. Set **Unit of Measure Code** when the scenario is specific to that UOM; leave it blank for a fallback. Item assignments use the Item's UOMs; Product assignments use the general UOM list.
4. Select **Transport Profile Code**. The profile must be unblocked, but can still be unreviewed while you prepare the setup.
5. Set **Default** and **Priority** for ranking. Higher Priority ranks first. Default does not force a profile onto a Stage.
6. Fill **Effective From** and **Effective To** when validity is time-limited. Blank boundaries are open-ended.
7. Use assignment **Blocked** to suspend that link and **Comment** to explain its purpose.

The same source, Variant, UOM, and profile combination has one assignment row; dates are not a separate version key. Creating an assignment does not copy dangerous-goods data onto the Item or cargo line.

Use the Item/Product **Transport Profile Count** and **Has Transport Profiles** indicators as setup aids. They count unblocked assignments; they do not prove that a profile is reviewed, effective, or eligible for a particular Stage. The Forwarding Order line count likewise is not a compliance result.

#### Assignment specificity and dates

For an Item, TMS tries these groups in order and uses the first group containing eligible profiles:

1. Exact Variant and exact UOM.
2. Exact Variant and blank UOM.
3. Blank Variant and exact UOM.
4. Blank Variant and blank UOM.

For a TMS Product, exact UOM is tried before blank UOM. If a more specific group has no eligible profiles, a less specific group can be used. Eligibility includes assignment dates and blocking, profile review/blocking, and the Stage context.

The assignment date is the date of **Stage Start DateTime**, then the **Forwarding Order Document Date**, then **Work Date** if both are blank. Effective From and Effective To include their boundary dates.

**No relevant assignments and no eligible assignments are different situations:**

- With no assignment relevant to a line's source/Variant/UOM, that line does not constrain Stage profile selection. The planner must assess it manually.
- If relevant records exist but no group supplies an eligible profile, selection/checking fails. Blocking or expiring all assignments does not turn the cargo into unrestricted cargo.
- Assignments belonging only to another Variant or UOM do not constrain the current line.

For cargo that must be controlled, create deliberate assignments for every applicable scenario. Do not leave a dangerous-goods Item unconfigured and assume that TMS will detect its classification.

### 7. Complete Emergency Response setup

Open **TMS Setup > Emergency Response** and fill the provider, primary phone, international phone, contact, contract ID, and notes as applicable.

The bundled **Dangerous Goods Bill of Lading** prints the primary phone, provider, and international phone. Contact and contract ID are available to a custom report layout; Emergency Response Notes are not in that report dataset. These are live company-level values, not a historical copy on each shipment.

## How to use Transport Profiles in daily planning

1. On the **Forwarding Order**, review the direct Item/Product lines, Variants, UOMs, quantities, and actual packing arrangement. Maintain cargo assignments for goods that must constrain selection.
2. Open **Stages** and select the leg to plan. Check its **Mode of Transport** and **Transport Regulation Code**.
3. Check **Package Type Code** and **Vehicle Unit Type Code**. A filled value limits eligible profiles; use values that reflect the actual leg.
4. Set the planned start date so the intended assignment validity date is used.
5. Choose **Suggest Transport Profile**, or open the **Transport Profile Code** lookup.
6. Review the chosen profile, its handling requirements, and the cargo it applies to. Choose **Check Transport Profile**.
7. Create or assign the **Freight Order**. Review its Vehicle Unit Type and any individual Stage equipment values.
8. Set **Temperature Control** and **Temperature** when an actual operating temperature is to be checked. Choose **Check Vehicle Unit Type**.
9. Confirm a Conditional equipment instruction only after checking the physical equipment and supporting information.
10. Review loaded logistic units and preview the required documents. Use **Dangerous Goods Bill of Lading** when cargo-specific dangerous-goods information is needed.

Repeat the relevant checks after changes to cargo, dates, mode, package, equipment, temperature, assignments, or profile data. A successful check does not save an approval status for the shipment.

## Selecting a profile on a stage

A candidate must satisfy all applicable conditions:

| Condition | Selection rule |
|---|---|
| Review and blocking | Profile is reviewed and unblocked. |
| Mode and regulation | Match the Stage exactly; the Stage needs both values to build its candidate list. |
| Package Type | A nonblank Stage Package Type must match the profile. A blank value does not restrict candidates. |
| Stage Vehicle Unit Type | A nonblank value must be listed as nonblocked when the profile has equipment rules. A blank value does not filter profiles by equipment. |
| Cargo assignments | The profile must be eligible for every configured direct Item/Product line on this Forwarding Order. |
| Assignment validity | Assignment is unblocked and effective for the date described above, in the first eligible specificity group. |

The lookup's equipment filter only checks the permitted type list. A visible profile can still fail temperature or Conditional checks. These run immediately for an unassigned Stage with explicit equipment and during supported linked-Stage checks. Use **Check Vehicle Unit Type** for the complete Freight Order check.

**Suggest Transport Profile** applies the profile automatically only when there is exactly one common candidate. With several candidates it opens a selection list, even if one is Default. The field lookup opens the selection list. Candidates are ranked by Default and then higher Priority; for mixed configured cargo, a candidate is Default only if all participating assignments mark it Default, and its priority is the lowest participating priority.

If no cargo line has relevant assignments, the list contains all reviewed, unblocked profiles matching the Stage context. This also applies when there are no direct Item/Product lines. If configured lines have no common profile, correct the setup or separate incompatible cargo into different Forwarding Orders.

Every Stage considers the configured direct Item/Product lines of the whole Forwarding Order. It does not select a different subset of those lines for each leg. **Logistic Unit contents are not traversed by this Stage check.** Review the loaded-cargo report separately.

| Stage action | What it does |
|---|---|
| **Suggest Transport Profile** | Finds common eligible profiles and applies the only candidate or asks the user to select one. |
| **Check Transport Profile** | Validates the current profile, context, and participating cargo assignments. It is not a complete equipment/temperature check. |
| **Transport Profile Assignments** | Shows the effective, unblocked assignments participating through the same specificity and eligibility rules. |

Changing Mode clears the profile and regulation, then applies the mode's default regulation if configured. Changing Regulation clears a profile that no longer matches. Changing Package Type clears a mismatched profile; changing Vehicle Unit Type clears a profile that prohibits or does not list the new type in its closed list. A profile retained after an equipment change can still fail temperature or Conditional checks. Review the resulting fields and select and check a replacement when necessary.

When the Freight Order split process creates a parallel Stage, the new Stage's Transport Profile is cleared. Select and check a profile for the new leg.

## Checking Vehicle Unit Type and temperature

On the **Freight Order**, the Vehicle Unit Type lookup intersects the equipment lists of profiles on Stages whose **Vehicle Unit Type Code is blank**. These are the Stages that use the header equipment as a fallback. Profiles with no equipment rules add no restriction. Unlisted or Blocked candidates are excluded where a participating profile has a closed list; Preferred candidates rank first. Stages with an explicit equipment type do not restrict the header lookup.

For validation, a filled **Stage Vehicle Unit Type Code** takes precedence; the **Freight Order Vehicle Unit Type Code** is used when the Stage value is blank. Keep both levels consistent with the actual equipment. Selecting a header type alone does not prove that every Stage uses that value.

Choose **Check Vehicle Unit Type** for the explicit complete check. It requires a header Vehicle Unit Type, at least one assigned Stage, and a Transport Profile on every assigned Stage, including Service Stages. It rechecks the profiles and cargo assignments before checking equipment. Automatic field and Stage-link checks allow work in progress: they skip Stages without profiles and skip equipment validation when the Freight Order header Vehicle Unit Type is blank. They are not a substitute for this explicit action. There is no separate Transport Profile compliance gate on release or posting.

Temperature checks against a Vehicle Unit Type Rule use:

1. Freight Order **Temperature**, when **Temperature Control** is on.
2. Otherwise, the profile's defined target temperature.
3. Otherwise, both ends of the profile's complete defined minimum/maximum range.

If a rule has temperature boundaries and neither a target nor a complete range is available, the check fails. If the rule has no temperature boundaries, this rule adds no temperature restriction.

Freight Order has no temperature-unit field. When Temperature Control is on, use profiles with one common Temperature Unit; mixed units can be rejected as ambiguous. Rule boundaries can use Celsius or Fahrenheit, and values are converted for comparison.

A Conditional rule with nonblank Condition Text asks for confirmation during an interactive equipment check. Rejecting the condition stops the operation. A background process that cannot obtain the required confirmation also fails. Confirmation is not stored as a lasting approval and can be requested again.

## Reports and documents

Use **Bill of Lading** on the Freight Order with **TMA DG Bill of Lading** configured in report selections. Complete **Bill of Lading No.** and review the first Stage's selected Transport Profile before printing. The report produces one document per Freight Order from actual loaded **Type = Unit** content, including nested Item/Product goods, or from assigned FWO goods when no units are loaded. Report selections also control email attachments.

The report uses the profile already selected during Stage planning:

| Planning or document | Cargo and profile source |
|---|---|
| Stage selection/check | One common profile for configured direct Item/Product lines of the Forwarding Order. |
| Dangerous Goods Bill of Lading | The Transport Profile selected on the first Freight Order Stage applies to every included Item/Product row, in both LU and FWO modes. Printing does not select profiles from cargo assignments. |

The first Stage is determined by Priority, Forwarding Order No., and Line No., even when that Stage has no loaded units of its own. The report does not compare the transport contexts of later Stages.

Goods without Transport Profile Assignments still receive the selected stage profile's dangerous-goods data. Assignment Variant/UOM, Default, Priority and effective dates do not override that choice during printing. The selected profile must remain unblocked, reviewed, valid, and consistent with the first Stage's mode, regulation and any specified Package Type. A blank Stage Package Type uses the profile's package code. If the first Stage has no selected profile, rows retain their ordinary descriptions and blank profile/hazardous-material values, even when cargo assignments exist. A blank hazardous-material field therefore does not prove that the goods are nonregulated.

For regulated cargo, the report uses the profile's Transport ID, Proper Shipping Name, technical names, ERG, hazard classes, Packing Group, STCC, additional description, and marks. Supported Detail descriptions marked **Print on Transport Document** are included; Conditional Rule text is excluded. Subsidiary hazard classes form part of regulated classification independently of that flag.

**Conditional** or **Not Assessed** Hazardous Material Status prevents printing. The report does not resolve conditional classification or perform the full interactive equipment check. Complete **Check Vehicle Unit Type** separately before issuing the document.

Load Summary also exposes Stage Vehicle Unit Type, regulation, profile code/description, Proper Shipping Name, and package context in its dataset. See [Reports and Documents](reports.md#dangerous-goods-bill-of-lading) for the complete printing prerequisites and layout behavior.

## Maintaining profiles already in use

- Review changes against the current source information and assess their effect on open work.
- Once a live Stage uses a profile, its Mode, Regulation, Package Type, Bulk Status, Shipping State, and Temperature Condition cannot be changed. Create or copy a replacement profile.
- Review the replacement, create its cargo assignments, and update affected Stages deliberately.
- Block the old profile when it must no longer be used. **Replaced By Profile Code** records the successor but does not switch assignments or Stages automatically.
- Blocking or losing review does not remove existing references; their next validation can fail.
- Assignment and equipment-rule changes take effect immediately and do not clear the header review flag. Check affected shipments after such changes.
- Preserve issued documents and review evidence using your company's document procedure. Reprinting uses current master data rather than an archived classification snapshot.

## Recommended setup example

The following codes illustrate planning only; they do not prescribe a dangerous-goods classification.

| Record | Example |
|---|---|
| Item | **CARGO-A**, with a drum UOM and a bulk UOM configured for that Item. |
| Profiles | **A-ROAD-DRUM**, **A-ROAD-BULK**, and **A-SEA-DRUM**, each with its own verified transport context and classification. |
| Assignments | Drum UOM allows the two drum scenarios; bulk UOM allows the bulk scenario. Use effective dates and Default/Priority deliberately. |
| Equipment rules | Each profile explicitly lists the equipment permitted for that scenario. Add Conditional text only for a condition the dispatcher can verify. |
| Road Stage | Set the road mode, its applicable regulation, drum Package Type, and planned equipment; then suggest and check the profile. |
| Sea Stage | Set the sea mode and applicable regulation. Select and check the corresponding profile separately. |

For two configured direct cargo lines, if line A permits profiles P1/P2 and line B permits P2/P3, the common Stage candidate is P2. If their sets do not intersect, a higher Priority does not solve the incompatibility. If line B has no relevant assignments, only line A constrains the list, and the planner must assess B separately.

## Permissions

| Permission set | Access |
|---|---|
| **TMS Administrator** | Includes Transport Setup permissions to maintain profile headers, regulations, related setup, and Emergency Response, and to mark a validated profile as reviewed. |
| **TMS User** | Includes Transport Planner permissions to read profile headers/regulations and perform planning checks. It also grants maintenance rights to Details, Assignments, and Vehicle Unit Type Rules; it does not grant profile-header modification or review. |

The internal Transport Setup and Transport Planner sets are included in the main roles. Consider the related-table maintenance rights when assigning responsibilities. Have an administrator change reviewed Details: clearing the review also requires permission to update the profile header, which the planner role alone does not grant. Effective access depends on the user's combined permission sets.

## Installed example profiles

Install, reinstall, and upgrade create nineteen blocked examples: ten for road transport under DOT, five for rail transport under RID, and four for air transport under IATA.

| Code | Mode / regulation | Example cargo | Package | Class / packing group |
|---|---|---|---|---|
| `EX-DOT-UN1203` | Road / DOT | Gasoline | Bulk liquid (`VL`) | 3 / II |
| `EX-DOT-UN1090` | Road / DOT | Acetone | Steel drum (`1A`) | 3 / II |
| `EX-DOT-UN1789` | Road / DOT | Hydrochloric acid | Plastic jerrycan (`3H`) | 8 / II |
| `EX-DOT-UN1950` | Road / DOT | Flammable aerosols | Aerosol (`AE`) | 2.1 / N/A |
| `EX-DOT-UN3480` | Road / DOT | Lithium-ion batteries | Fibreboard box (`4G`) | 9 / N/A |
| `EX-DOT-UN1978` | Road / DOT | Propane | Cylinder (`CY`) | 2.1 / N/A |
| `EX-DOT-UN1072` | Road / DOT | Compressed oxygen | Cylinder (`CY`) | 2.2 (5.1) / N/A |
| `EX-DOT-UN1942` | Road / DOT | Ammonium nitrate | Woven plastic bag (`5H`) | 5.1 / III |
| `EX-DOT-UN1824` | Road / DOT | Sodium hydroxide solution | Plastic jerrycan (`3H`) | 8 / II |
| `EX-DOT-UN3077` | Road / DOT | Environmentally hazardous solid, N.O.S. | Woven plastic bag (`5H`) | 9 / III |
| `EX-RID-UN1202` | Rail / RID | Diesel fuel | Bulk liquid (`VL`) | 3 / III |
| `EX-RID-UN1203` | Rail / RID | Gasoline | Bulk liquid (`VL`) | 3 / II |
| `EX-RID-UN1090` | Rail / RID | Acetone | Steel drum (`1A`) | 3 / II |
| `EX-RID-UN1978` | Rail / RID | Propane | Cylinder (`CY`) | 2.1 / N/A |
| `EX-RID-UN1072` | Rail / RID | Compressed oxygen | Cylinder (`CY`) | 2.2 (5.1) / N/A |
| `EX-IATA-UN1090` | Air / IATA | Acetone | Steel drum (`1A`) | 3 / II |
| `EX-IATA-UN1789` | Air / IATA | Hydrochloric acid | Plastic jerrycan (`3H`) | 8 / II |
| `EX-IATA-UN1950` | Air / IATA | Flammable aerosols | Aerosol (`AE`) | 2.1 / N/A |
| `EX-IATA-UN3480` | Air / IATA | Lithium-ion batteries | Fibreboard box (`4G`) | 9 / N/A |

The air examples intentionally do not assert aircraft acceptance, packing instructions, quantity limits, state-of-charge limits, operator variations, or passenger-versus-cargo-aircraft eligibility. Those mode-specific conditions must be assessed from the current IATA DGR and the actual shipment data before a copied profile can be reviewed.

These records are deliberately **Blocked**, **not reviewed**, and have no SDS, cargo assignments, or Vehicle Unit Type rules. They are training templates, not legally approved classifications. Copy an example and verify the actual product, concentration, technical name, packaging, quantities, exceptions, operational requirements, transport mode, current SDS, and applicable regulation before use.

The defaults flow is insert-only: if a regulation, mode, package type, profile, or example detail already exists, reinstall and upgrade do not overwrite customer changes.

## Troubleshooting

| Problem | What to check |
|---|---|
| Review fails | Complete required context, assessed operational statuses, regulated-cargo fields, temperature bounds, RQ UOM, and valid Details. |
| Profile is missing on the Stage | Check review/blocking, exact Mode and Regulation, optional Package Type and Vehicle Unit Type, dates, and the first eligible assignment group for every configured direct cargo line. |
| Default was set but selection is still requested | More than one eligible candidate exists. Default ranks the Stage list; it does not force automatic selection. |
| A more general assignment is used | The more specific Variant/UOM group supplied no eligible profiles, so TMS tried a fallback group. |
| A blocked/expired assignment causes an error | Relevant records exist but no group provides an eligible profile. Correct validity/setup; this does not become unrestricted cargo. |
| There is no common Stage profile | Review each configured cargo line. Correct the assignments or separate cargo that genuinely needs incompatible scenarios into different Forwarding Orders. |
| Equipment is absent from the Freight Order lookup | Check the closed lists of profiles on Stages with blank equipment. One Blocked row does not leave unlisted types open. |
| A listed profile or equipment type fails after selection | The lookup's type filter does not prove temperature compatibility or fulfillment of Conditional instructions. Complete those checks. |
| Header equipment seems different from the checked equipment | Review Stage Vehicle Unit Type values, which take precedence during validation. |
| Check Vehicle Unit Type requires another profile | The explicit check requires a profile on every assigned Stage, including Stages left incomplete during planning. |
| Review disappeared | A source/classification/requirement field or a Detail changed. Recheck and review the profile. |
| An existing profile cannot be changed or deleted | Check Stage use and references. Copy the profile and use a reviewed successor. |
| DG Bill of Lading has no dangerous-goods data for an Item | Check the Transport Profile selected on the first Freight Order Stage and its hazardous-material classification. Cargo assignments are not required for printing that profile. |
| DG Bill of Lading uses an unexpected profile | Check the first Stage in Priority, Forwarding Order No., Line No. order. Its selected profile applies to all cargo; later Stage profiles and assignment rankings do not replace it. |
| DG Bill of Lading rejects Conditional classification | Resolve the hazardous-material classification for this scenario; the report does not calculate it automatically. |
| Emergency information is incomplete on the document | Review TMS Setup and the selected report layout. The bundled layout prints provider and primary/international phones. |

## Related

- [TMS Setup](setup.md)
- [Forwarding Orders](forwardingorder.md)
- [Stages](stages.md)
- [Freight Orders](freightorder.md)
- [Products and Item assignments](product.md)
- [Vehicles](vehicle.md)
- [Logistic Unit Types](logisticunittype.md)
- [Reports and Documents](reports.md)
- [Assign Permission Sets](assignpermissionsets.md)
