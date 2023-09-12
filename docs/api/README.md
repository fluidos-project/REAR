# REAR API

This chapter details all the REAR messages, the purpose of then and the message body. Specifically, we can distinguish the messages in required and optional:
 * Required (Figure 1 details an example of possible interaction between client and provider using the required messages)
   * **List flavors**, sent by the client to probe the available flavors offered by a given provider
   * **Reserve flavor**, sent by the client to perform a reservation on a specific flavor
   * **Purchase flavor**, sent by the client to complete the purchase of an offered flavor
 * Optional
   * **Refresh**, sent by the provider to refresh a particular flavor. By sending a refresh message, the provider helps maintain the availability of flavors and allows the consumer to effectively manage and allocate resources based on the updated expiration time.
   * **Withdrawal**, sent by the provider to the consumer to notify that a specific flavor is no longer available. This message serves as a notification mechanism to inform the consumer that the requested flavor is no longer available.

Note that the sequence of messages between the client and the provider is fixed, as well as the order. This is because each step requires a set of information returned from the previous step(s).

Moreover, there is a huge difference in the communication pattern between required and optional messages. Indeed, required messages follow a client/server approach, i.e., with the client always initiating the communication, whereas the optional messages are sent asynchronously by the server towards the clients. Such design choice greatly improves the expressiveness of the protocol, but it calls for different architectural style for the communication (e.g., REST, Websocket, …), as the different types of messages have different requirements. Appendix A details the communication patterns that are REAR-compatible. 

## Required messages
This section details the required messages in the REAR protocol for resource advertisement reservation and

### List Flavours

##### Request body
None

##### Response body
      Items: [		
        Flavour: {	
                  # FlavourID is the ID of the Flavour
                  FlavourID string
                  
                  # ProviderID is the ID of the provider of this Flavour.
                  ProviderID string
                  
                  # Type is the type of the Flavour
                  FlavourType string
                  
                  #Characteristics contains the characteristics of the Flavour. 
                  Characteristics Characteristics
                  
                  # Policy contains the policy of the Flavour. 
                  Policy Policy
                  
                  #Owner contains the identity info of the owner of the Flavour. 
                  Owner NodeIdentity
                  
                  # Price contains the price model of the Flavour.
                  Price Price
                  
                  # Optional fields that can be retrieved from the Flavour.
                  OptionalFields OptionalFields

        }	
      ]							


        Characteristics: {	
          # Architecture is the architecture of the Flavour.
          Architecture string 

          # Cpu is the number of CPU cores of the Flavour.
          Cpu int

          # Memory is the amount of RAM of the Flavour.
          Memory int

          # Gpu is the number of GPU cores of the Flavour.
          Gpu int

          # EphemeralStorage is the amount of ephemeral storage of the Flavour.
          EphemeralStorage int

          # PersistentStorage is the amount of persistent storage of the Flavour.
          PersistentStorage int
        }

        
        Price: {	
          # Amount is the amount of the price.
          Amount string 

          # Currency is the currency of the price.
          Currency string

          # Period is the period of the price.
          Period string 
        }
        

        Policy: {
          Partitionable: {
            # CpuMin is the minimum requirable number of CPU cores of the Flavour.
            CpuMin int `json:"cpuMin"`
            
            # MemoryMin is the minimum requirable amount of RAM of the Flavour.
            MemoryMin int `json:"memoryMin"`
            
            # CpuStep is the incremental value of CPU cores of the Flavour.
            CpuStep int `json:"cpuStep"`
            
            # MemoryStep is the incremental value of RAM of the Flavour.
            MemoryStep int `json:"memoryStep"`
          },
          Aggregatable: {
            # MinCount is the minimum requirable number of instances of the Flavour.
            MinCount int
            
            # MaxCount is the maximum requirable number of instances of the Flavour.
            MaxCount int
          }
        }


        NodeIdentity: {	  
          # Domain is the domain of the node.
          Domain string 

          # NodeID is the ID of the node.
          NodeID string 

          # IP is the IP of the node.
          IP     string 
        }	

        
        OptionalFields: {	
            # Availability is the availability flag of the Flavour
            Availability bool

            # WorkerID is the ID of the worker that provides the Flavour.
            WorkerID string 
        }	

### List Flavors + Selector

##### Request body
        Selector: {	
            # FlavourType specifies the type of Flavour.
            FlavourType string 

            # Architecture specifies the architecture of the resource.
            Architecture string 

            # Cpu is the exact desired CPU quantity.
            Cpu int 

            # Memory is the exact desired memory quantity.
            Memory int 

            # EphemeralStorage is the exact desired ephemeral storage quantity.
            EphemeralStorage int 

            # MoreThanCpu specifies the minimum CPU quantity desired.
            MoreThanCpu int 

            # MoreThanMemory specifies the minimum memory quantity desired.
            MoreThanMemory int 

            # MoreThanEph specifies the minimum ephemeral storage quantity desired.
            MoreThanEph int 

            # LessThanCpu specifies the maximum CPU quantity desired.
            LessThanCpu int 

            # LessThanMemory specifies the maximum memory quantity desired.
            LessThanMemory int 

            # LessThanEph specifies the maximum ephemeral storage quantity desired.
            LessThanEph int 
        }

##### Response body
        Flavour: {	
          # FlavourID is the ID of the Flavour
          FlavourID string
                  
          # ProviderID is the ID of the provider of this Flavour.
          ProviderID string
                  
          # Type is the type of the Flavour
          FlavourType string
                  
          # Characteristics contains the characteristics of the Flavour. 
          Characteristics Characteristics
                  
          # Policy contains the policy of the Flavour. 
          Policy Policy
                  
          # Owner contains the identity info of the owner of the Flavour. 
          Owner NodeIdentity
                  
          # Price contains the price model of the Flavour.
          Price Price
                  
          # Optional fields that can be retrieved from the Flavour.
          OptionalFields OptionalFields
        }

### Reserve flavor	

##### Request body
        Reservation: {	
            # FlavourID specifies the ID of the Flavour to be reserved
            FlavourID string

            # Buyer is the buyer Identity of the Fluidos Node that is reserving the Flavour
            Buyer NodeIdentity
        }

##### Response body
        Transaction: {	
            # TransactionID is the ID of the Transaction linked to the Reservation
            TransactionID string

            # FlavourID is the ID of the flavour that is being reserved
            FlavourID string

            # Buyer is the buyer Identity of the Fluidos Node that is reserving the Flavour
            Buyer NodeIdentity

            # StartTime is the time at which the reservation should start
            StartTime string 
        }

### Purchase flavor

##### Request body
        Purchase: {	
          # TransactionID is a unique identifier for the transaction.
          TransactionID string

          # FlavourID identifies the flavour being purchased.
          FlavourID string

          # BuyerID is the identifier of the buyer.
          BuyerID string
        }

##### Response body

      <empty> Status: 200 OK

The implementation of what to return as a response is left to the user (by default, it returns 200 OK). However, one possible solution could be to return a “Contract” that confirms the successful acquisition of the flavor between the two parties. An example could be:

        Contract: {	
          ContractID string
          
          Flavour Flavour
          
          Buyer string
          
          Seller string
          
          Credentials LiqoCredentials
          
          …
        }

## Optional messages

### Refresh

**TBD**

### Withdraw

**TBD**
			

