---
title: "Map Providers"
description: "Configure Google Maps or Azure Maps for geocoding, route calculation, distance, and visualization in TMS."
---

# Map Providers

TMS can use **Google Maps** or **Azure Maps** for geocoding, distance and duration calculation, and route visualization.

## Credential model

Each provider is configured with one credential in **TMS Setup**:

| Credential | Used by | Security restriction |
|---|---|---|
| **Google Api Key** | Business Central service tier and the Google Maps browser control | The value is visible to browser users. Apply API restrictions, quotas, rotation, and monitoring. |
| **Azure Maps Subscription Key** | Business Central service tier and the Azure Maps browser control | The value is visible to browser users. Configure CORS, quotas, rotation, and monitoring. |

The setup page shows a masked placeholder when a credential exists. TMS stores the value in two company-scoped encrypted isolated-storage slots and uses it internally for server requests and browser visualization. Entering, replacing, or clearing the field updates both slots and requires permission to modify **TMS Setup**. Operations users can continue to use maps without permission to change the shared credential.

During a supported upgrade from legacy Google-key storage, TMS saves and verifies both encrypted slots before clearing the plaintext table value. The obsolete table field remains in the schema for AppSource compatibility. Runtime map operations read isolated storage only and do not fall back to that field; if a key is missing, an administrator must configure it in **TMS Setup**.

Dependent extensions must use the supported business operations of **TMA Map Management**, such as geocoding, distance calculation, and map or route display. Existing provider-specific business functions remain available. Direct credential access is internal to TMS; the obsolete public Google and ABS credential methods remain declared for compatibility but reject calls with an error, including calls that only check whether a key exists.

Because the same value is used by both server and browser operations, encryption protects it only at rest. Users who can inspect browser traffic can extract it.

## Privacy and outbound data transfer

Google Maps and Azure Maps are optional external services. Selecting a provider and saving a credential does not by itself send business addresses. TMS sends data when a user or background process performs geocoding, calculates distance or duration, obtains a distance matrix, searches for an address in the map control, or displays a location or route.

Depending on the operation, the selected provider receives:

| Data sent outside Business Central | Purpose |
|---|---|
| Address, city, post code, country/region, and related location text | Geocoding or browser address search. |
| Latitude, longitude, origins, destinations, and waypoints | Map display, route calculation, and distance matrix calculation. |
| Travel mode | Route calculation for the selected mode of transport. |
| Provider credential and technical request metadata | Authentication, service operation, security, diagnostics, metering, and billing by the provider. |

The information can identify customer, vendor, ship-to, order, warehouse, terminal, or other business locations and may constitute personal data when it relates to an identifiable person. Before enabling a provider, the customer administrator is responsible for:

- confirming a lawful basis and any required notices or consent for the addresses and locations being processed,
- reviewing the provider terms, privacy policy, retention rules, geographic processing, and subprocessor arrangements,
- selecting the appropriate **Azure Maps Geo Scope** and verifying that it meets the organization’s data-residency requirements,
- limiting address content to what is necessary for routing and geocoding,
- configuring credential restrictions, CORS where supported, quotas, cost controls, rotation, and monitoring,
- enabling **Allow HttpClient Requests** for the TMS extension only after approving the outbound connection.

TMS stores map-location data and provider-aware cached distance/duration results in Business Central. Administrators must manage correction, deletion, and cache retention in accordance with their policies and the selected provider’s terms. Browser map scripts and tiles are loaded directly from the selected provider and are governed by that provider’s terms.

Review the applicable external documents before use:

- [Google Maps Platform Terms of Service](https://cloud.google.com/maps-platform/terms)
- [Google Privacy Policy](https://policies.google.com/privacy)
- [Google Maps Platform security and compliance](https://developers.google.com/maps/security/compliance/security-compliance)
- [Microsoft Azure Product Terms — Azure Maps](https://www.microsoft.com/licensing/terms/productoffering/MicrosoftAzureMaps/)
- [Microsoft Privacy Statement](https://privacy.microsoft.com/privacystatement)
- [Azure Maps geographic scope and data locations](https://learn.microsoft.com/azure/azure-maps/geographic-scope)

The integration is also registered in the standard Business Central **Privacy Notices Status** page as **TMS Google Maps and Azure Maps**. An administrator can review the disclosure link and record the organization-level decision there.

## Configure Google Maps

1. Select **Google Map** in **TMS Setup**.
2. Enter the Google API key. The same value is used for geocoding, distance requests, and browser visualization.
3. Verify geocoding and route display from a [Map Location](maplocation.md).

Google Maps supports driving, walking, transit, and bicycling. A truck travel mode is rendered as regular driving because this integration does not expose a Google truck profile.

## Configure Azure Maps

1. Select **Azure Maps** in **TMS Setup**.
2. Enter the Azure Maps subscription key. The same value is used for server requests and browser visualization.
3. Select the required geographic scope: **Global**, **Europe**, or **United States**.
4. Verify geocoding, route calculation, and route display from a [Map Location](maplocation.md).

Azure Maps supports the configured travel mode, including truck routing when **Truck** is selected on the TMS mode of transport.

> **Security note:** Azure Maps Shared Key authentication is exposed in browser requests and Microsoft classifies it as the least secure client-side authentication option. Configure CORS, rotation, monitoring, quotas, and cost controls. A future SAS-token or Microsoft Entra flow is required where Shared Key exposure is not acceptable.

## Travel mode in the LSP application

The standard LSP document flow derives the map travel mode directly from **Mode of Transport**.

Azure Maps uses a real truck route when **Truck** is selected. Google Maps renders the same mode as regular driving because this integration does not expose Google truck-aware routing.

Provider-aware cache entries include the signature of the effective travel mode, so results calculated for a different mode cannot be reused silently.

A full forwarding or freight order can be displayed as one route only when its stages form one continuous, non-parallel chain and resolve to the same map travel mode. For a disconnected, parallel, or multimodal stage graph, TMS stops with an explanatory error instead of drawing a misleading route; display the individual stages separately.

## Distance matrix behavior

The operational **Route Distance Matrix** shows provider cache rows, their creation/validity metadata, and manual overrides. During upgrade, rows from the published legacy **Distance Matrix** are copied to **Route Distance Matrix**:

- legacy manual rows are migrated as provider-neutral manual overrides,
- legacy calculated rows are identified as Google Maps provenance but remain cold history because the old model did not preserve enough coordinate and travel-mode context for safe reuse,
- the legacy table and page are obsolete and remain in the extension only for AppSource schema and dependent-extension compatibility. The legacy page is hidden from search and is read-only when opened directly; TMS does not expose it as an operational setup or cache page.

After migration, create and maintain manual overrides in **Route Distance Matrix**. During the Pending lifecycle, manual writes made by a dependent extension through the published legacy table are immediately mirrored into Route Distance Matrix; core routing still reads only the new table. TMS otherwise touches the legacy table only through compatibility adapters for already-published provider contracts. Dependent extensions must move to **TMA Route Distance Matrix** before the legacy object is removed in a future major version.

Provider results are reused only when all of the following still match:

- map provider,
- effective travel-mode signature,
- origin and destination coordinates,
- provider API contract version.

Changing coordinates, provider, or travel mode therefore cannot silently reuse an incompatible cached result.

## Troubleshooting

| Problem | What to check |
|---|---|
| Geocoding fails | Provider API key, enabled provider APIs, address quality, and outbound HTTP permission. |
| The factbox is blank | Provider API key and the selected provider in TMS Setup. |
| Azure route is not shown | Azure Maps subscription key, endpoint scope, CORS, and coordinates for every route point. |
| A route uses an unexpected distance | Manual Route Distance Matrix override, coordinates, selected provider, and Mode of Transport. |

## Related

- [Google Maps Integration](googlemapintegration.md)
- [Map Locations](maplocation.md)
- [TMS Setup](setup.md)
