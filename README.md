# Transportation Management System for Microsoft Dynamics 365 Business Central

Help and Documentation of the [TMS on AppSource](https://appsource.microsoft.com/en-us/product/dynamics-365-business-central/PUBID.extensionsforcelimited1647259189111%7CAID.tms%7CPAPPID.7bfc8c44-7cc8-4ba3-98d0-4f9964697a01?tab=Overview)

## Introduction

TMS is a comprehensive solution intended to streamline the transportation processes, including order management, scheduling, and tracking of shipments.

The TMS solution is ideal for two usage scenarios:

- if you are a Shipper company that manages the transportation process of its sales, purchase and trasnfer orders and uses either its own transport with its drivers or hires, as well as uses third-party carriers.
- if you are a Logistic Service Provider (LSP) whose business involves the delivery of third-party goods, managing the transportation process and may also have its own fleet of vehicles and drivers or utilize services of third-party carriers. In this scenario, the mechanism for cost control and transparency in the invoicing process for transportation is crucial.

## Prerequisites

- Microsoft Dynamics 365 Business Central [product page](https://www.microsoft.com/en-us/dynamics-365/products/business-central). TMS is an add-on extension for Business Central that builds on its core modules—such as Customers, Sales, Vendors, Purchasing, and Warehousing—using them as a solid foundation while adding only the features necessary for transportation management.

## Installation and Setup

The TMS configuration process involves the creation and editing of TMS directories and entities to ensure optimal alignment of the TMS module with the company's business processes.

The configuration process is contingent on the company type, whether it is a Shipper, that is, it manages the shipment of its own goods, or an LSP (Logistic Service Provider), whose business revolves around managing the transportation of third-party cargo.

Many settings are utilized in both scenarios but with varying levels of detail.

TMS comprises two main components: the core TMS and the Logistic Units extension. The core TMS utilizes functions provided by Logistic Units, which offers functionality for handling pallets and containers and features its own configuration system.

- Installation for Business Central On-line [See detailed](installation.md)

## After Installation

After installing the TMS extension, several steps must be completed to make TMS available to users.

- Purchase and assign licenses. [See detailed instructions](buylicenses.md)

- Assign permission set to users [See detailed instructions](assignpermissionsets.md)

## Settings

- **Stages profiles**. Defines the structure of the transportation process, including the number of stages, such as whether it will be a single-stage delivery or a three-stage multimodal transportation [details](stages.md).
- **Status profiles**. Statuses are used to organize the workflow of document processing in accordance with the business process. Different types of documents may have different status structures [details](statuses.md).
- **Services**. The Services directory is a reference within the TMS system designed for accounting for transportation services provided by the company. These services are used to invoice clients for transportation services. Required for invoicing [details](services.md).
- **Charges**. The Charges directory is a reference within the TMS system intended for the recording and allocation of costs to orders and the calculation of profitability for transportation orders. Required for profit calculation [details](charges.md).
- **Service Set**. This setting allows for the definition of sets of services and costs that can be added automatically when creating orders, or for the use of restrictions on the sets of services and costs that can be utilized in an order.
- **Content Set**. Predefined set of content automatically added to an order upon its creation.
- **Forwarding Order Types**. Templates for forwarding orders that establish the structure of the order and the business process for handling said order, as well as setting default values. Mandatory for LSP scenario [details](forwardingordertype.md).
- **Freight Order Types**. Templates for freight orders define order parameters, including the status structure and the model for order numbering.
- **Logistic Unit Type**. Templates for logistics units, such as pallets, containers, and boxes, define dimensional parameters: length, width, height, as well as fill control parameters: by weight or volume.
- **Map Locations**. Map Locations is a directory of map-based locations utilized for precise pinpointing using geolocation services. Map Locations are employed for route mapping, distance calculations, and estimating transportation durations. Map Locations can be linked to the addresses of clients and suppliers. Useful if we have our own fleet.
- **Map Location Types**. A directory of location types on the map, such as Client, Vendor, Port, Gateway, Hub, etc.
- **Map Provider services** integration. Configuration of Google Maps and TMS integration. It' needed for distance and duration estimation  [details](googlemapintegration.md)
- **Carriers**. A directory of third-party carriers that provide transportation services to our company.
- **Vehicles**. A directory of transportation vehicles, either owned by our company or by third-party carriers.
- **Drivers**. Configuration of the directory for drivers, whether from our company or external.
- **Routes**. Routes are used for the logical grouping of customer addresses by geographical attribute to facilitate the assignment of a set of customer orders to a specific truck or carrier.
- **Units of measure**. The TMS module's measurement units directory defines the primary linear, volumetric, and weight measurement units, along with their interrelationships and conversion coefficients.
- **Freight Classes**. The Freight Class Directory is a standardized classification system for less-than-container load (LCL) freight shipments, categorizing them based on specific attributes. The classification of cargo freight is established using various criteria, such as value, weight, length, density, and additional factors.
- **Setup** General settings of the TMS  [details](setup.md).

## Shipper Scenario

if you are a Shipper company that manages the transportation process of its sales (and purchase) orders and uses either its own transport with its drivers or hires, as well as uses third-party carriers.

- **Transport Request** is a transportation request based on internal company documents, such as purchase orders, sales orders, or transfer orders. It defines WHAT needs to be transported, where the goods are to be picked up, and where they need to be delivered, while also specifying the shipper and the consignee.
- **Logistic Units** is an item of any composition intended for transportation. Logistic units take many forms: a single box containing a limited number of products, a pallet with multiple products, or an intermodal container containing multiple pallets.
- **Delivery Order** is a document that details HOW the transportation will be carried out. It specifies the carrier, driver, and vehicle. The Delivery Order represents the actual journey of the truck, outlining the stops where loading or unloading will take place.

## Logistics Service Provider scenario (LSP)

## Use cases
