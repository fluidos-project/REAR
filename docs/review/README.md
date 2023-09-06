# State of the art

This section introduces the state of the art for the REAR protocol, focusing both on commercial solutions, and research proposals.

## Commercial solutions

In this section, we will explore real use cases that demonstrate the resource acquisition workflows in action. By examining examples from various industries, such as Booking.com, we can gain insights into how these organizations manage the process of acquiring resources effectively and efficiently. Through these real-world scenarios, we will uncover the underlying workflows and understand how different platforms and frameworks facilitate the resource acquisition process.

### Booking.com Connectivity APIs

The Booking.com Connectivity APIs [1] enable to send and retrieve data for properties listed on Booking.com. It is possible to manage room availability, reservations, prices, and many other things.
The Booking.com Connectivity APIs offer a number of specialized functions, divided into these categories:
 - Content: Create properties, rooms, rates, and policies, and link this information together for the Booking.com website.
 - Rates and Availability: Load inventory counts, rates, and price availability re- strictions (for specific room-rate combinations), per date and/or date range com- bination.
 - Reservations: Retrieve reservations, modifications, and cancellations made on Booking.com.
 - Promotions: Create special promotions for certain date ranges and booker types.
 - Reporting: Report credit card problems, changes to reservations after check-in, and no-shows.

In addition to the specialized APIs, we also have a set of supporting APIs for retrieving general Booking.com system information, such as accepted currency codes and room names.

#### Reservations APIs

A reservation represents the booking of one or more room nights at a property. Each reservation is a unique booking created by a guest using the Booking.com channels. Reservations API keeps you updated on your bookings by sending a sequence of messages, also known as reservation messages.
 
The messages are classified as new booking confirmation, modification to an existing booking, or cancellation. Regardless of the category, the reservations API provides the data in a common format. A reservation may include several units of rooms, apartments or villas. Each reservation or booking is specific to exactly one property.
To process reservations, Booking.com provides two sets of endpoints using the fol- lowing two specifications:
 - OTA XML specifications (OTA_HotelResNotif e OTA_HotelResModifyNotif): A complete and fault-tolerant reservations processing solution following the specification from the OpenTravel Alliance (OTA). Use this solution to retrieve and acknowledge processing the reservations.
 - B.XML specifications (/reservations): A simple and light-weight solution to retrieve reservations following Booking.com’s XML specifications. Use this solution to retrieve the property reservations. Acknowledging that you successfully processed the reservation is currently not supported with this solution.

 ### Ticketmaster

Ticketmaster [2] is a globally recognized ticketing platform that revolutionized the way people purchase tickets for various events, including concerts, sports games, and theatrical performances. With its user-friendly interface and extensive event catalog, Ticketmaster has become a go-to destination for millions of customers worldwide.

The ticket acquisition workflow on Ticketmaster follows several key steps to ensure a seamless and efficient ticket purchasing process for customers:
 - Event Discovery: Customers begin by browsing Ticketmaster’s website or mobile app to explore upcoming events in their area. They can search by event type, artist, venue, or date to find the desired event.
 - Ticket Selection: Once customers find the event they are interested in, they can select the specific tickets they want to purchase. Ticketmaster offers various ticket options, including different seating sections, price ranges, and quantities.
 - Seat Allocation: After selecting tickets, the system allocates seats based on the customer’s preferences and availability. Ticketmaster’s seat selection algorithm ensures that seats are assigned in the most optimal way to accommodate the customer’s group and provide an enjoyable experience.
 - Checkout Process: Customers proceed to the checkout page, where they review their ticket selection, enter their payment and billing information, and complete the transaction. Ticketmaster supports multiple payment methods, including credit cards, digital wallets, and other secure payment options.
 - Order Confirmation: Once the purchase is completed, customers receive an order confirmation that includes details such as the event name, date, time, seating information, and a unique order ID. This confirmation serves as proof of purchase and is often sent via email or can be accessed through the customer’s Ticketmaster account.


#### Partner APIs

The Ticketmaster Partner API lets clients reserve, purchase, and retrieve ticket and event information.

If a user abandons a page/tab after a ticket reserve has been made, client applications should do their best to detect this and issue a DELETE /cart request to free up allocated resources on the ticketing server. This should also be done if client apps no longer want to wait through a long, continuing polling process. This is necessary since ticket reserve requests that result in polling will eventually complete asynchronously and take up resources even if clients do not consume the next polling url.

It is possible to use the different APIs to define the workflow for searching and purchasing a ticket:
 - GET /discovery/v2/events: find events and filter your search by location, date, availability, and much more.
 - POST /partners/v1/events/{event_id}/cart?apikey={apikey}: reserves the specified tickets. For integrations requiring captcha, send the captcha solution token in the json body. A hold time will be returned in the cart response that will indicate, in seconds, how long the cart is available for. This value may increase if the user moves through the cart process.
 - GET /partners/v1/events/{event_id}/...: get shipping options available for this event. Note: some API users will be pre-configured for certain shipping options and may not need to perform this. Specifying the “region” query parameter will return options available for users in the selected country. Using the value ‘ALL’ will return all options.
 - PUT /partners/v1/events/{event_id}/...: add a shipping option to the event. Note: some API users will be pre-configured for certain shipping options and may not need to perform this.
 - PUT /partners/v1/events/{event_id}/cart/payment: add customer and billing information to the order.
 - PUT /partners/v1/events/{event_id}/cart?apikey={apikey}: finalize the purchase and commit the transaction.

## Literature review

Reservation protocols are essential communication mechanisms that ensure fair and efficient resource allocation in shared environments. These protocols are commonly used in distributed systems, networking, and multi-user applications to prevent conflicts and coordinate access to critical resources.

One of the most adopted network communication protocol in computer networks is RSVP, which stands for Resource Reservation Protocol [3]. RSVP primary goal is to establish and manage resource reservations for data transmission, and it is primarily utilized in Quality of Service (QoS) enabled networks to ensure the efficient and reliable delivery of data traffic. Still, one of the main limitations of RSVP is related to the limited scalability, because, as the number of participants and the complexity of the network increase, managing and maintaining reservations can become challenging. This is because RSVP operates in a soft-state manner, which requires the continuous refreshing of reservations, limiting its adoption for mobile hosts. To overcome such limitation MRSVP [4] have been proposed, allowing mobile devices to perform reservations non only for the current location, but also for future locations. In addition, [8] extends the problem formulation, including the price of networking resources, so that the network service provider can communicate availability of services and delivers price quotations and charging information to the user, and the user requests or re-negotiates services with desired specifications for one or more flows.

As an extension of RSVP, RSVP-TE (Resource Reservation Protocol - Traffic Engineering) is designed to support traffic engineering capabilities in computer networks. It enables the establishment of explicit paths for data traffic, allowing network administrators to control the flow of traffic and optimize network resources. RSVP-TE has thus been proposed in combination with MPLS to preform path signaling in wide area network [5][6]. However, with RSVP and similar solutions, network devices are only included in the path if they have enough bandwidth resources to accommodate flows, failing to include the multi-dimensionality of the computing resources.

Authors in [7] present the Service Negotiation and Acquisition Protocol (SNAP) as a means to enable communication and negotiation between different entities in a distributed system, such as clients and servers. The protocol aims to establish agreements on the expected quality of service (QoS) that clients require and that servers can provide. In the attempt to extend the flexibility of the SLA negotiation mechanism [9] proposes a bilateral protocol for SLA negotiation using the alternate offers mechanism wherein a party is able to respond to an offer by modifying some of its terms to generate a counter offer. Finally, authors in [12] also describe a brokering architecture that is able to make advance resource reservations and create SLAs using the WS-Agreement standard [10], based on the Contract Net protocol for negotiating SLAs [11]. 

Recently, also telco Operators in the 5G era have a significant opportunity to monetize the capabilities of their networks. This paradigm change led to additional requirements for the Edge infrastructure [13], and the to the definition of a suitable protocol to allow seamless application deployment across different Telco providers [14]. 


## References 

[1] Booking.com Connectivity APIs, https://connect.booking.com/user_guide/site/en-US/user_guide.html

[2]Ticketmaster developer docs, https://developer.ticketmaster.com/

[3]	Zhang, Lixia, et al. "RSVP: A new resource reservation protocol." IEEE network 7.5 (1993): 8-18.

[4] Talukdar, Anup Kumar, B. R. Badrinath, and Arup Acharya. "MRSVP: A resource reservation protocol for an integrated services network with mobile hosts." Wireless Networks 7 (2001): 5-19.

[5] Awduche, Daniel, et al. RSVP-TE: extensions to RSVP for LSP tunnels. No. rfc3209. 2001.

[6] Berger, Lou. Generalized multi-protocol label switching (GMPLS) signaling resource reservation protocol-traffic engineering (RSVP-TE) extensions. No. rfc3473. 2003.

[7] Czajkowski, Karl, et al. "SNAP: A protocol for negotiating service level agreements and coordinating resource management in distributed systems." Job Scheduling Strategies for Parallel Processing: 8th International Workshop, JSSPP 2002 Edinburgh, Scotland, UK, July 24, 2002 Revised Papers 8. Springer Berlin Heidelberg, 2002.

[8] Wang, Xin, and Henning Schulzrinne. "RNAP: A resource negotiation and pricing protocol." Transit 6.B7 (1999): B8.

[9] Venugopal, Srikumar, Xingchen Chu, and Rajkumar Buyya. "A negotiation mechanism for advance resource reservations using the alternate offers protocol." 2008 16th Interntional Workshop on Quality of Service. IEEE, 2008.

[10] Andrieux, Alain, et al. "Web services agreement specification (WS-Agreement)." Open grid forum. Vol. 128. No. 1. 2007.

[11] Smith, Reid G. "The contract net protocol: High-level communication and control in a distributed problem solver." IEEE Transactions on computers 29.12 (1980): 1104-1113.

[12] Elmroth, Erik, and Johan Tordsson. "A grid resource broker supporting advance reservations and benchmark-based resource selection." International Workshop on Applied Parallel Computing. Berlin, Heidelberg: Springer Berlin Heidelberg, 2004.

[13] GSMA Operator Platform Telco Edge Requirements 2022 https://www.gsma.com/futurenetworks/resources/gsma-operator-platform-telco-edge-requirements-2022/

[14] GSMA Operator Platform Group – East-Westbound Interface APIs https://www.gsma.com/futurenetworks/resources/east-westbound-interface-apis/

