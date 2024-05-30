# REAR MESSAGES

**REAR** defines a set of messages that facilitate the client/provider interaction for the purchase of available computing resources or services. At its core, REAR has been designed with a focus on generality (i.e., able to be general enough to describe a huge variety of computing and/or service instances). Figure 1 depicts a possible interaction between a customer and a provider using the REAR protocol.
This section describes the main interaction enabled by the REAR protocol, whereas the details of the different APIs will be provided [here](./docs/api/README.md), and the data models for the Flavor definition is available [here](https://github.com/fluidos-project/REAR-data-models).

![](/images/REAR_workflow.png)

_Figure 1. Example of interaction between client and provider using the required messages_

The protocol comprises various messages, which can be classified as either _required_ or _optional_:

- **Required**:
  - **LIST_FLAVORS**, sent by the customer to probe the available flavors offered by a given provider.
  - **RESERVE_FLAVOR**, sent by the customer to inform the provider about its willingness to reserve a specific flavor.
  - **PURCHASE_FLAVOR**, sent by the customer to complete the purchase of an offered flavor.
- **Optional**:
  - **REFRESH_FLAVOR**, sent by the provider to refresh a particular flavor. By sending a refresh message, the provider helps maintain the availability of flavors and allows the consumer to effectively manage and allocate resources based on the updated expiration time.
  - **WITHDRAW_FLAVOR**, sent by the provider to the consumer to notify that a specific flavor is no longer available. This message serves as a notification mechanism to inform the consumer that the requested flavor is no longer available.

## GET THE LIST OF AVAILABLE FLAVORS

The **LIST_FLAVOR** message is sent by the consumer to probe the available flavors offered by a given provider.
Since different FlavorTypes can be offered by a single provider, the consumer can filter out possible resources by specifying the desired characteristics in the **LIST_FLAVOR** message, using the FlavorType data model described [here](https://github.com/fluidos-project/REAR-data-models). 
For example, if the consumer wants to purchase VMs, it can retrieve the list of possible VMs offered by a provider by specifying characteristics of the VM FlavorType, such as 2 CPUs and 4GB of RAM. 
The provider will then reply with a list of VMs that match the requirements, if any.

The **LIST_FLAVOR** message thus contains the requested FlavorType with the desired characteristics and some form of identification for the consumer (TODO: detail here the vPresentation to authenticate the consumer). 

## RESERVE A FLAVOR

Once the consumer knows the Flavors and their IDs, the Flavor reservation process is performed through the **RESERVE_FLAVOR** message, sent by the consumer to inform the provider about its willingness to reserve a specific flavor.
Specifically, the consumer/provider interaction can be summarized as follows.
After the client has collected the list of available Flavors offered by the provider, it notifies the intention of reserving a specific flavor by sending the **RESERVE_FLAVOR** message, specifying the ID of the Flavor to be reserved.

To verify the consumer identity, the **RESERVE_FLAVOR** message must also include an authentication token that will be then validated by a Trusted third-party authentication and authorization service (TODO: detail here the vPresentation to authenticate the consumer).
Once received, the provider checks if the flavor is still available (there might be some delay between the **LIST_FLAVOR** message and the subsequent reservation). and if so it replies with a summary of the reservation process including the _TransactionID_ and the _Time To Purchase (TTP)_, i.e., the time by which the Flavor must be purchased. 
This allows reserved Flavors to be released in case either the consumer becomes unreachable, or the subsequent purchase process exceeds a predefined threshold.
If the Flavor is not available a 404 error message is sent to the consumer.

## PURCHASE A FLAVOR

The **PURCHASE_FLAVOR** message is sent by the consumer upon receipt of the provider's response during the reservation phase to complete the purchase of an offered flavor.
Specifically, the **PURCHASE_FLAVOR** message notifies the intention of the consumer to finalize the purchase of the resource.
To do so, the consumer sends the **PURCHASE_FLAVOR** message including the _TransactionID_ (obtained with the **RESERVE_FLAVOR**) and the identification token to the provider (TODO: detail here the vPresentation to authenticate the consumer).
If authorized, the consumer will then be prompted to a payment service (either external or managed by the provider) and, if successful, a copy of the _Contract_ is returned to the consumer, detailing the purchase and the information required to access the purchased resource (e.g., IP address, API endpoint).

## SUBSCRIBE TO CHANGES

REAR defines a set of optional messages that extend the expressiveness of the protocol, and we summarize as subscribe to changes. Specifically, we include two optional interactions:

- Refresh, sent by the provider to refresh a particular flavor. By sending a refresh message, the provider helps maintain the availability of flavors and allows the consumer to effectively manage and allocate resources based on the updated expiration time.
- Withdraw, sent by the provider to the consumer to notify that a specific flavor is no longer available. This message serves as a notification mechanism to inform the consumer that the requested flavor is no longer available.

Figure 3 details the REAR interaction using the combination of both optional and required messages. Specifically, the interaction can be summarized as follows:

- The client sends a request to get the list of available flavors matching a predefined selector
- The client notifies the provider the intention to receive continuous updates on a specific flavor, using the SUBSCRIBE_FLAVOR message, that is mapped onto an appropriate request message which may vary depending on the implementation technology used (e.g. Websockets, publish/subscribe technologies).
  - In case the client is interested in multiple flavors, this results in multiple SUBSCRIBE_FLAVOR messages, one for each flavor.
- This internally triggers the creation of a stateful communication channel between the client and the provider.
- At this point the provided sends asynchronous updates over the created channel to the client for the specified flavor. We define two different types of updates:
  - The refresh expiration time message notifies the client that a previous offer for a specific flavor is still valid.
  - The withdraw message notifies the client that a previous flavor offer is no longer available for the purchase.

![](../../images/updating_messages.svg)

_Figure 3. REAR interaction using optional messages._
