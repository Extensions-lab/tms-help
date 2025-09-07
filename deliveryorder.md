# Delivery Order

is a document that details HOW the transportation will be carried out. It specifies the carrier, driver, and vehicle. The Delivery Order represents the actual journey of the truck, outlining the stops where loading or unloading will take place.

## Capabilities

The Delivery Order document covers the following functional areas of the TMS:

- Load Management
- Route Management
- Weight, Volume, Footage Control
- Load Sequencing & Optimization

A Delivery Order is:

- a real vehicle run, the actual driving route, detailing the order of stops the vehicle must make
- a set of tasks to be completed, such as loading, unloading, or pallet pickup,
- a loading sequence for the truck,
- a tool for control weight, volume, or footage limits at each stop along the route

![Elements of the Delivery Order](resources/deliveryorder/pics/deliveryorder1.png)

Delivery Order consolidates multiple transport requests into a single document, creating a vehicle route that accounts for weight and volume constraints, while optimizing the route based on distance or duration.

![Elements of the Delivery Order](resources/deliveryorder/pics/deliveryorder2.png)

Delivery Order allows for the automatic determination of the vehicle loading sequence at the warehouse, based on the order of the delivery stops along the route.

![Elements of the Delivery Order](resources/deliveryorder/pics/deliveryorder3.png)

The Delivery Order allows you to manually set the vehicle loading sequence or the order of stops along the route by adjusting the Sequence field.

![Elements of the Delivery Order](resources/deliveryorder/pics/deliveryorder4.png)

Delivery Order can control for additional route elements, such as the trip to the warehouse or the return journey (distance and duration), which are critical for managing driver work hours or fleet operations for companies with their own vehicle fleets

![Elements of the Delivery Order](resources/deliveryorder/pics/deliveryorder5.png)

Delivery Order enables the selection of Transport Requests based on route numbers. Each customer can be assigned a route number that corresponds to their delivery address.

![Elements of the Delivery Order](resources/deliveryorder/pics/deliveryorder6.png)

Integration with Google Maps for constructing the optimal delivery route between MAP locations with the capability for optimization based on distance or duration criteria. [Setup](googlemapintegration.md)

![Elements of the Delivery Order](resources/deliveryorder/pics/deliveryorder7.png)

Automatically checks the loading and unloading schedule for the entire route, accounting for transportation time, and indicates if it’s not possible to arrive at a specific location at the required time.

![Elements of the Delivery Order](resources/deliveryorder/pics/deliveryorder8.png)

