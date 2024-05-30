# REAR API

This section details the required messages in the REAR protocol for resource advertisement and reservation.

> **_API Data Models Disclaimer_**  
> The data models used for the following API are referred to the [REAR Data Models](https://github.com/fluidos-project/REAR-data-models) specifications.

## How to use

Redocly OpenAPI viewer [here](https://redocly.github.io/redoc/?url=https://raw.githubusercontent.com/topix-hackademy/Rear/main/docs/api/openapi.yaml).

## Optional Messages (WIP)

### REFRESH_FLAVOR

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

### WITHDRAW_FLAVOR

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
