# REAR Workflow and Messages
**REAR** defines a set of messages that facilitate the client/provider interaction for the purchase of available computing resources or services. At its core, REAR has been designed with a focus on generality (i.e., able to be general enough to describe a huge variety of computing and/or service instances). Figure 1 depicts a possible interaction between a customer and a provider using the REAR protocol.
This section describes the main interaction enabled by the REAR protocol, whereas the details of the different APIs will be provided [here](./docs/api/README.md).

![](/images/main_workflow.png)

*Figure 1. Example of interaction between client and provider using the required messages*
 
## REAR FLAVORS
TODO Francesco: add the definition of a FLAVOR.

## LIST_FLAVORS: get the list of available flavors

The **LIST_FLAVORS** message provides the client with the list of available flavors offered by a given producer. Using a standardized selector, a client can request the list of available flavors matching specific needs, like a given amount of computing resources (e.g., CPU, RAM, storage), the flavor type (e.g., VM, Kubernetes cluster, DB service), and additional policies (e.g., maximum price). 

If properly formatted, the list flavor message returns the list of available flavors offered by a given producer (if any). Specifically, each item in the list will have the following key information:
 * **Flavor ID**: Each offer should be identified by a unique Flavor ID instead of just the name. 
 * **Provider ID**: Associate the Flavor with the corresponding Provider ID. 
 * **Type**: Specify the type of the Flavor (e.g., VM/K8s Cluster/etc.).
 * **Characteristics**: Specify the capacities and resources provided by the Flavor (CPU, RAM, etc.). 
 * **Policy**: Specify if the Flavor is aggregatable /partitionable
 * **Owner**: represents the entity that owns the Flavor (FQDN/unknown). It can correspond to the Provider ID of the Flavor.
 * **Price or Fee**: If applicable, specify the price or fee associated with the Flavor. 
 * **Expiration Time**: It represents the duration after which the Flavor needs to be refreshed. If the Flavor is not refreshed within the Expiration Time, it becomes invalid or expires. The Expiration Time can be calculated by adding a specific timestamp to the current time, indicating the number of hours or days until expiration.
 * **Optional Fields**: Other details such as limitations, promotions, availability etc., can be included as optional information.

Note that if the producer does not have available Flavors, or does not have Flavors matching the provided selector, it may return an empty list.

The interaction is always initiated by the client and can be summarized as follows:
 * The client wants to retrieve the list of available favors offered by a provider.
   * The client creates the selector using one of the standardized ones based on the requirements.
 * After the message is ready, an HTTP GET  is sent to the provider to get the list of filtered flavors.
 * The provider returns the list of matching flavors.
   * If the provider does not have available flavors, or does not have flavors matching the specified selector, an empty list will be returned.

## RESERVE_FLAVOR: reserve a flavor
The **RESERVE_FLAVOR** message is sent by the client to the provider to notify the intention of reserving an offered flavor. It is the first step that requires to handle the concurrency in client requests, as different clients may be interested in the same flavor. Note that this message only notifies the provider the intention of purchasing a flavor, the request must then be finalized using the confirm purchase message (see following subsection).

Specifically, the client/provider interaction can be summarized in the following:
 * After the client has collected the list of available flavors offered by the provider, it notifies the intention of reserving a specific flavor by sending an HTTP POST and including the ID of the flavor to be reserved.
 * Once received by the provider, two separate actions are performed:
   * The provider checks if the flavor is still available (there might be some delay between the list flavor message and the subsequent reserve flavor request, thus the flavor may no longer be available). In case the Flavor is still available the provider replies with a summary of the reservation process , otherwise a 404 error message is sent to the client.
   * The provider instantiates a timer to limit the reservation time for that specific flavor. This allows reserved flavors to be released in case either the client becomes completely irresponsive, or the subsequent purchase process exceeds a predefined threshold.

Figure 2 extends the non-concurrent interaction, including concurrent access to shared resources (i.e., flavors), from multiple clients. Specifically, the interaction can be summarized with the following steps:
 * Customer 1 and 2 both request the list of available flavor based on predefined selectors, and they both notify the intention to reserve a specific flavor (i.e., flavor 1234 in this case).
 * The first customer to send the reserve flavor message, triggers on the provider side the acquisition of the lock associated with the shared flavor.
 * The first customer can thus continue with the purchase of the selected flavor, whereas the second will not receive any further messages until the first customer releases the shared lock, either finalizing the purchase, or exceeding the predefined timeout.
 * In case the first customer finalized the purchase, the second customer will acquire the shared lock, and receive a 404-error message, notifying that the favor is no longer available. In case the first customer didn’t finalize the purchase, the second customer can proceed with the normal interaction described in the previous use case.

![](/images/concurrent_access.png)

*Figure 2. Concurrent flavor access from two different client.*
 
## CONFIRM_RESERVATION 
The confirm reservation message concludes the resource advertisement and reservation process. It is sent by the provider to the client, to notify the finalization of the purchase .
The interaction can be summarized in the following steps:

**TBD**

## REFRESH and WITHDRAW: handle possible changes in the flavor list 
REAR defines a set of optional messages that extend the expressiveness of the protocol, and we summarize as subscribe to changes. Specifically, we include two optional interactions:
 * REFRESH, sent by the provider to refresh a particular flavor. By sending a refresh message, the provider helps maintain the availability of flavors and allows the consumer to effectively manage and allocate resources based on the updated expiration time.
 * WITHDRAW, sent by the provider to the consumer to notify that a specific flavor is no longer available. This message serves as a notification mechanism to inform the consumer that the requested flavor is no longer available.

Figure 3 details the REAR interaction using the combination of both optional and required messages. Specifically, the interaction can be summarized as follows:
 * The client sends a request to get the list of available flavors matching a predefined selector
 * The client notifies the provider the intention to receive continuous updates on a specific flavor, using the subscribe flavor message, that is mapped onto an appropriate request message which may vary depending on the implementation technology used (e.g. Websockets, publish/subscribe technologies). 
   * In case the client is interested in multiple flavors, this results in multiple subscribe flavor message, one for each flavor.
 * This internally triggers the creation of a stateful communication channel  between the client and the provider. 
 * At this point the provided sends asynchronous updates over the created channel to the client for the specified flavor. We define two different types of updates:
   * The refresh expiration time message notifies the client that a previous offer for a specific flavor is still valid. 
   * The withdraw message notifies the client that a previous flavor offer is no longer available for the purchase.


![](/images/optional_msg.jpg)

*Figure 3. REAR interaction using optional messages.*
