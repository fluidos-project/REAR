# REAR API

This chapter details all the REAR messages, the purpose of then and the message body. Specifically, we can distinguish the messages in required and optional:
 * Required (Figure 1 details an example of possible interaction between client and provider using the required messages)
   * **List flavors**, sent by the client to probe the available flavors offered by a given provider
   * **Reserve flavor**, sent by the client to perform a reservation on a specific flavor
   * **Confirm reservation**, sent by the client to complete the purchase of an offered flavor
 * Optional
   * **Refresh**, sent by the provider to refresh a particular flavor. By sending a refresh message, the provider helps maintain the availability of flavors and allows the consumer to effectively manage and allocate resources based on the updated expiration time.
   * **Withdrawal**, sent by the provider to the consumer to notify that a specific flavor is no longer available. This message serves as a notification mechanism to inform the consumer that the requested flavor is no longer available.

Note that the sequence of messages between the client and the provider is fixed, as well as the order. This is because each step requires a set of information returned from the previous step(s).

Moreover, there is a huge difference in the communication pattern between required and optional messages. Indeed, required messages follow a client/server approach, i.e., with the client always initiating the communication, whereas the optional messages are sent asynchronously by the server towards the clients. Such design choice greatly improves the expressiveness of the protocol, but it calls for different architectural style for the communication (e.g., REST, Websocket, …), as the different types of messages have different requirements. Appendix A details the communication patterns that are REAR-compatible. 

## Required messages
This section details the required messages in the REAR protocol for resource advertisement and reservation.

### List Flavours

    Flavour: {			
		Items: [		
			Flavour: {	
				# FlavourID is the ID of the Flavour.FlavourID 
                flavourID: string

                # ProviderID is the ID of the provider of this Flavour.
                ProviderID string

                # Type is the type of the Flavour
                FlavourType string

                #Characteristics contains the characteristics of the Flavour. They are based on the type of the Flavour and can change depending on it.
                Characteristics Characteristics

                # Policy contains the policy of the Flavour. The policy describes the partitioning and aggregation properties of the Flavour.
                Policy Policy

                #Owner contains the identity info of the owner of the Flavour. It can be unknown if the Flavour is provided by a reseller or a third party.
                Owner string

                #Price contains the price model of the Flavour.
                Price Price
                
                # This field is used to specify the optional fields that can be retrieved from the Flavour.
                OptionalFields OptionalFields
			}	
		]		
	}						

			
	Policy: {		
		Partitionable: {	
			# CpuMin is the minimum requirable number of CPU cores of the Flavour.
            CpuMin int

            # MemoryMin is the minimum requirable amount of RAM of the Flavour.
            MemoryMin int

            # CpuStep is the incremental value of CPU cores of the Flavour.
            CpuStep int
            
            # MemoryStep is the incremental value of RAM of the Flavour.
            MemoryStep int

        },	
        Aggregatable: {	
            # MinCount is the minimum requirable number of instances of the Flavour.
            MinCount int

            # MaxCount is the maximum requirable number of instances of the Flavour.
            MaxCount int
		}	
	}	

### Reserve flavor

		
	Reservation: {			
		Items: [		
			Reservation: {	
				# ReservationID is the ID of the reservation
                ReservationID string

                # TransactionID is the ID of the transaction that this reservation is part of
                TransactionID string

                #  BuyerID is the ID of the buyer that is reserving the node
                BuyerID string

                # Flavour is the flavour of the node that is being reserved
                Flavour Flavour

                # StartTime is the time at which the reservation started
                StartTime string
			}	
		]		
	}			
	  		
## Optional messages

### Refresh

**TBD**

### Withdraw

**TBD**
			

