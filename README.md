![Alt text](./images/logo.svg)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
============

The **REAR (REsource Advertisement and Reservation) protocol** enables different actors such as cloud providers and customers to **advertise**, **reserve** (and then **consume**) **resources** (e.g., virtual machines and their characteristics in terms of CPU, RAM; a Kubernetes cluster, etc.), and **services** (e.g., a database as a service).
A dedicated onthology supports also **capabilities** (e.g., the availability of GPU hardware in a Kubernetes clusters), in order to better characterize offered resource/services, while being easily extensible and future-proof.

In REAR two main actors are involved, namely **providers** and **customers**:
  * **Providers** avertise their resources and services in a standardized format.
  * **Customers**  explore and find resources according to their specific criteria.

REAR enables potential customers to know what is available in the provider clusters, hence enabling a true, dynamic market.
In addition, it aims at automating (and hiding) the technical steps that enable the customer to connect and consume the resources/services agreed in the negotiation phase, such as setting up a peering with [Liqo](https://liqo.io).

Overall, REAR seamlessly integrates with established resource management systems and platforms.

### REAR key characteristics

**REAR** has been designed with a focus on generality. This is because it allows to perform resource exchange for (possibly) any type of resources and services, ranging from traditional VMs, Kubernetes clusters, services (e.g., DBs), and sensors and actuators (e.g., humidity and temperature sensors).

## Documentation structure

The **REAR** documentation has been organized as follows:

  * [Here](./docs/review/README.md) we detail the state of the art for other resource allocation protocols both from an industry and research perspective.
  * [Here](./docs/messages/README.md) we detail the different type of messages we envision in the REAR protocol 
  * [Here](./docs/api/README.md) we delve into the details of the REAR api.


