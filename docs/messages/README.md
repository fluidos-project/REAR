# REAR

**REAR** defines a set of messages that facilitate the client/provider interaction for the purchase of available computing resources or services. At its core, REAR has been designed with a focus on generality (i.e., able to be general enough to describe a huge variety of computing and/or service instances). Figure 1 depicts a possible interaction between a customer and a provider using the REAR protocol.
This section describes the main interaction enabled by the REAR protocol, whereas the details of the different APIs will be provided [here](./docs/api/README.md), and the data models for the Flavor (i.e., resources in the continuum) definition is available [here](https://github.com/fluidos-project/REAR-data-models).

![](/images/REAR_workflow.png)

_Figure 1. Example of interaction between consumer and provider using the required messages_

The protocol comprises various messages, which can be classified as either _required_ or _optional_:

- **Required**:
  - **LIST_FLAVORS**, sent by the customer to probe the available flavors offered by a given provider.
  - **RESERVE_FLAVOR**, sent by the customer to inform the provider about its willingness to reserve a specific flavor.
  - **PURCHASE_FLAVOR**, sent by the customer to complete the purchase of an offered flavor.
- **Optional**:
  - **REFRESH_FLAVOR**, sent by the provider to refresh a particular flavor. By sending a refresh message, the provider helps maintain the availability of flavors and allows the consumer to effectively manage and allocate resources based on the updated expiration time.
  - **WITHDRAW_FLAVOR**, sent by the provider to the consumer to notify that a specific flavor is no longer available. This message serves as a notification mechanism to inform the consumer that the requested flavor is no longer available.

# AUTHENTICATION

The REAR protocol foresees a peer-to-peer interaction between the **consumer** (i.e., the node requesting a given resource) and the **provider** (i.e., the node offering the reaource). 
Both parties must be authenticated to prevent unauthorized access to the resources available in the continuum.

At it's core, REAR relies on a **Decentralized identifiers (DIDs)** system as a way to provide globally unique identifier to verify the various actors involved in the REAR message exchange and removing the need for a centralized registry.
Each device in the continuum is represented by a set of attributes, constituting what's called a **Verifiable Credential (VC)**.
Upon connecting to the continuum, a node must performe the enrolment towards a Trusted Issuer to be included in the DID, receiving as the VC with the attributes disclodesed during the enrolment (see Figure 2).  
The VC will then be stored in the private wallet of the owner for future use.

![](/images/REAR_auth.png)

_Figure 2. Example of interaction between consumer and trusted issuer_

In the following, we outline the fields that can be included in the VC (note that no all attributes must be present):

 - "holderName": "John Doe"
 - "name": "John Doe"
 - "phone": "+1-555-1234"  
 - "holderRole": "Researcher"
 - "fluidosRole": "Admin"
 - "association": "IEEE"
 - "deviceType": "Laptop"
 - "fluidosID": "12345ABC"
 - "holderAddress": "1234 Elm Street, Apt 5"
 - "holderEmail": "john.doe@example.com"  
 - "ou": "Research and Development"
 - "physicalAddress": "5678 Maple Avenue, Building B"
 - "DID": "did:example:123456789abcdefghi"
 - "macAddress": "00:1A:2B:3C:4D:5E"
 - "registrationDate": "2023-01-15T10:00:00Z"
 - "orgIdentifier": "ORG-123456"
 - "department": "Computer Science"
 - "university": "Tech University"
 - "organization": "Tech Innovations Inc."
 - "city": "San Francisco"
 - "zipcode": "94107" 
 - "state": "California"
 - "province": "California"
 - "country": "USA"
 - "dn": "CN=John Doe, OU=Research and Development, O=Tech Innovations Inc., L=San Francisco, ST=California, C=US"

The VC contains all the attributes describing a given node, however, not all of them must be provided to the producer when purchasing a given resource.
In fact, a given provider might require only a subset of them (e.g., only university and department are required).
To this end, a **Verifiable Presentation (VP)** can be requested through the local privacy and security manager, including only the subset of requested attributes and preventing information disclosure (see Figure 3).

![](/images/VPresentation.png)

_Figure 3. Verifiable Credential (VC) and Verifiable Presentations (VP)_


# REAR MESSAGES

In the following, we outline the main interactions in the REAR protocol.
Figure 4 depicts the REAR workflow introduced in Figure 1, including also the authentication presented in the previous section.

![](/images/REAR_workflow_complete.png)

_Figure 4. Complete REAR workflow (TODO: update the messages and replace with a better quality image)_

## ENROLLMENT AND AUTHENTICATION/AUTHORIZATION
This preliminary phase ensures the authentication and authorization of a FLUIDOS node for any subsequent communication. Upon connecting to a FLUIDOS infrastructure a customer is requested to provide a set of attributes to retrieve the Verifiable Credentials (i.e., the digital proof of identity recognized in FLUIDOS). The Verifiable Credential can then be used to request the related Verifiable Presentation (VP) containing only a subset of the attributes to use for future resource negotiation.

Before initiating any resource negotiation, the customer and the provider mutually exchange the respective VP to authenticate, following a two-step approach: the customer fist send the VP to the provider, which validates it using its own Privacy and Security Manager and a Distributed Ledger Technology (DLT). Upon validating the VP, the provider requests a VP to the Privacy and Security Manager (if not already available in its digital wallet) and sends it to the customer, which, in turn, validates it.

If this preliminary authentication phase is successful, all the subsequent messages will be authenticated/authorized.

## GET THE LIST OF AVAILABLE FLAVORS

The **LIST_FLAVOR** message is sent by the consumer to probe the available flavors offered by a given provider.
Since different FlavorTypes can be offered by a single provider, the consumer can filter out possible resources by specifying the desired characteristics in the **LIST_FLAVOR** message, using the FlavorType data model described [here](https://github.com/fluidos-project/REAR-data-models). 
For example, if the consumer wants to purchase VMs, it can retrieve the list of possible VMs offered by a provider by specifying characteristics of the VM FlavorType, such as 2 CPUs and 4GB of RAM. 

The provider will then reply with a list of VMs that match the requirements, if any.

## RESERVE A FLAVOR

Once the consumer knows the Flavors and their IDs, the Flavor reservation process is performed through the **RESERVE_FLAVOR** message, sent by the consumer to inform the provider about its willingness to reserve a specific flavor.
Specifically, the consumer/provider interaction can be summarized as follows.
After the client has collected the list of available Flavors offered by the provider, it notifies the intention of reserving a specific flavor by sending the **RESERVE_FLAVOR** message, specifying the ID of the Flavor to be reserved.

Once received, the provider checks if the flavor is still available (there might be some delay between the **LIST_FLAVOR** message and the subsequent reservation).
If the requirement is satisfied it replies with a summary of the reservation process including the _TransactionID_ and the _Time To Purchase (TTP)_, i.e., the time by which the Flavor must be purchased, and, again witht its VP (the Figure needs to be updated). 
This allows reserved Flavors to be released in case either the consumer becomes unreachable, or the subsequent purchase process exceeds a predefined threshold.
If the Flavor is not available a 404 error message is sent to the consumer.

## PURCHASE A FLAVOR

The **PURCHASE_FLAVOR** message is sent by the consumer upon receipt of the provider's response during the reservation phase to complete the purchase of an offered flavor.
Specifically, the **PURCHASE_FLAVOR** message notifies the intention of the consumer to finalize the purchase of the resource.
To do so, the consumer sends the **PURCHASE_FLAVOR** message including the _TransactionID_ (obtained with the **RESERVE_FLAVOR**).

The consumer will then be prompted to a payment service (either external or managed by the provider) and, if successful, a copy of the _Contract_ is returned to the consumer, detailing the purchase and the information required to access the purchased resource (e.g., IP address, API endpoint).

To ensure the contract hasn't been changed and to verify the provider's identity, the contract is encapsulated within a **JSON Web Token (JWT)** and signed using the DID private key. 
Specifically, the contract is first signed by the provider using the JWT, then returned to the consumer, who can add their own signature to the JWT. 
More nested signatures can be added as needed.

Hashes of the contracts can be stored in the **Distributed Ledger Technology (DLT)**, ensuring immutability and traceability. 
By retrieving the contract's hash from the DLT, it is possible to verify the contract's existence and integrity, providing notary-like effects. The signatures on the JWT can be verified using the public key associated with the DID of the signer, which can be publicly accessed in the DLT. This process bonds both the consumer and the producer to the agreed-upon transaction and ensures the integrity and authenticity of the contract.

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
