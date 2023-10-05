# REAR API

This section details the required messages in the REAR protocol for resource advertisement and reservation.

When it comes to optional messages, Websockets serve as the underlying technology. These messages can be defined either in XML or JSON format, providing flexibility in their specification.

## LIST_FLAVORS

#### Request body

None

#### Response body

      Items: [
        Flavor: {
                  # FlavorID is the ID of the Flavor
                  FlavorID string

                  # ProviderID is the ID of the provider of this Flavor.
                  ProviderID string

                  # Type is the type of the Flavor
                  FlavorType string

                  #Characteristics contains the characteristics of the Flavor.
                  Characteristics Characteristics

                  # Policy contains the policy of the Flavor.
                  Policy Policy

                  #Owner contains the identity info of the owner of the Flavor.
                  Owner NodeIdentity

                  # Price contains the price model of the Flavor.
                  Price Price

                  # Optional fields that can be retrieved from the Flavor.
                  OptionalFields OptionalFields

        }
      ]


        Characteristics: {
          # Architecture is the architecture of the Flavor.
          Architecture string

          # Cpu is the number of CPU cores of the Flavor.
          Cpu int

          # Memory is the amount of RAM of the Flavor.
          Memory int

          # Gpu is the number of GPU cores of the Flavor.
          Gpu int

          # EphemeralStorage is the amount of ephemeral storage of the Flavor.
          EphemeralStorage int

          # PersistentStorage is the amount of persistent storage of the Flavor.
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
            # CpuMin is the minimum requirable number of CPU cores of the Flavor.
            CpuMin int `json:"cpuMin"`

            # MemoryMin is the minimum requirable amount of RAM of the Flavor.
            MemoryMin int `json:"memoryMin"`

            # CpuStep is the incremental value of CPU cores of the Flavor.
            CpuStep int `json:"cpuStep"`

            # MemoryStep is the incremental value of RAM of the Flavor.
            MemoryStep int `json:"memoryStep"`
          },
          Aggregatable: {
            # MinCount is the minimum requirable number of instances of the Flavor.
            MinCount int

            # MaxCount is the maximum requirable number of instances of the Flavor.
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
            # Availability is the availability flag of the Flavor
            Availability bool

            # WorkerID is the ID of the worker that provides the Flavor.
            WorkerID string
        }

## LIST_FLAVORS + Selector

#### Request body

        Selector: {
            # FlavorType specifies the type of Flavor.
            FlavorType string

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

#### Response body

        Flavor: {
          # FlavorID is the ID of the Flavor
          FlavorID string

          # ProviderID is the ID of the provider of this Flavor.
          ProviderID string

          # Type is the type of the Flavor
          FlavorType string

          # Characteristics contains the characteristics of the Flavor.
          Characteristics Characteristics

          # Policy contains the policy of the Flavor.
          Policy Policy

          # Owner contains the identity info of the owner of the Flavor.
          Owner NodeIdentity

          # Price contains the price model of the Flavor.
          Price Price

          # Optional fields that can be retrieved from the Flavor.
          OptionalFields OptionalFields
        }

## RESERVE_FLAVOR

#### Request body

        Reservation: {
            # FlavorID specifies the ID of the Flavor to be reserved
            FlavorID string

            # Buyer is the buyer Identity of the Fluidos Node that is reserving the Flavor
            Buyer NodeIdentity
        }

#### Response body

        Transaction: {
            # TransactionID is the ID of the Transaction linked to the Reservation
            TransactionID string

            # FlavorID is the ID of the Flavor that is being reserved
            FlavorID string

            # Buyer is the buyer Identity of the Fluidos Node that is reserving the Flavor
            Buyer NodeIdentity

            # StartTime is the time at which the reservation should start
            StartTime string
        }

## PURCHASE_FLAVOR

#### Request body

        Purchase: {
          # TransactionID is a unique identifier for the transaction.
          TransactionID string
        }

#### Response body

      <empty> Status: 200 OK

The implementation of what to return as a response is left to the user (by default, it returns 200 OK). However, one possible solution could be to return a “Contract” that confirms the successful acquisition of the flavor between the two parties. An example could be:

        Contract: {
          ContractID string

          Flavor Flavor

          Buyer string

          Seller string

          Credentials LiqoCredentials

          …
        }

## REFRESH_FLAVOR

```xml
<RefreshMessage>
    <Flavor>
        <!-- Details of the Flavor object that has been refreshed -->
        <FlavorID>string</FlavorID>
        <ProviderID>string</ProviderID>
        <FlavorType>string</FlavorType>
        <!-- Other Flavor fields like Characteristics, Policy, Owner, Price, etc. -->
    </Flavor>
    <ModificationDetails>
        <!-- Details of the changes made to the Flavor -->
        <FieldModified>string</FieldModified>
        <OldValue>string</OldValue>
        <NewValue>string</NewValue>
        <!-- It is possibile to add other fields if needed -->
    </ModificationDetails>
</RefreshMessage>
```

**XML Structure:**

- `<RefreshMessage>` is the root element of the "refresh" message.
  - `<Flavor>` contains the details of the "Flavor" object that has been refreshed, with fields like FlavorID, ProviderID, FlavorType, and others.
  - `<ModificationDetails>` contains the details of the changes made to the Flavor, including the modified fields, the old values, and the new values. It is possibile to add additional fields to this section if necessary.

## WITHDRAW_FLAVOR

```xml
<WithdrawMessage>
    <Flavor>
        <!-- Details of the Flavor object that is no longer available -->
        <FlavorID>string</FlavorID>
        <ProviderID>string</ProviderID>
        <FlavorType>string</FlavorType>
        <!-- Other Flavor fields like Characteristics, Policy, Owner, Price, etc. -->
    </Flavor>
    <Reason>
        <!-- Reason for the withdrawal of the Flavor offer -->
        <Message>string</Message>
        <!-- It is possibile to add other fields for more detailed reasons if needed -->
    </Reason>
</WithdrawMessage>
```

**XML Structure:**

- `<WithdrawMessage>` is the root element of the "Withdraw" message.
  - `<Flavor>` contains the details of the "Flavor" object that is no longer available for purchase, with fields like FlavorID, ProviderID, FlavorType, and others.
  - `<Reason>` provides the reason for the withdrawal of the Flavor offer, typically in the form of a message. It is possibile to add additional fields for more detailed reasons if needed.
