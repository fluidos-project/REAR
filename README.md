![Alt text](./images/logo.svg)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
============
The **REsource Advertisement and Reservation (REAR) protocol** aims at providing secure data exchange of resources and capabilities between different cloud providers. It can be used to advertise resources (e.g., virtual machines and their characteristics in terms of CPU, RAM), capabilities (e.g., Kubernetes clusters) and (in future) services (e.g., a database as a server) to any third party, enabling potential customers to know what is available in other clusters, and possibly (automatically) establish the technical steps that enables the customer to connect and consume the resources/services agreed in the negotiation phase.

There are two main types of entity involved, which are **providers** and **customers**:
  * **Providers** avertise their resources and services in a standardized format.
  * **Customers**  explore and find resources according to their specific criteria.

Overall, REAR seamlessly integrates with established resource management systems and platforms.
This protocol accommodates diverse **resource types** and allows for **future expansions**.

### REAR key characteristics

**REAR** has been designed with a focus on generality. This is because it allows to perform resource exchange for (possibly) any type of resources and services, ranging from traditional VMs, Kubernetes clusters, services (e.g., DBs), and sensors and actuators (e.g., humidity and temperature sensors).

## Documentation structure

The **REAR** documentation has been organized as follows:

  * [Here](./docs/review/README.md) we detail the state of the art for other resource allocation protocols both from an industry and research perspective.
  * [Here](./docs/messages/README.md) we detail the different type of messages we envision in the REAR protocol 
  * [Here](./docs/api/README.md) we delve into the details of the REAR api.


