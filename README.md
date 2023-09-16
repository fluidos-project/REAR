![Alt text](./images/logo.svg)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
============

The **REAR (REsource Advertisement and Reservation) protocol** enables different actors such as cloud providers and customers to **advertise**, **reserve** (and then **consume**) **resources** (e.g., virtual machines and their characteristics in terms of CPU, RAM; a Kubernetes cluster, etc.), and **services** (e.g., a database as a service).
A dedicated onthology supports also **capabilities** (e.g., the availability of GPU hardware in a Kubernetes clusters), in order to better characterize offered resource/services, while being easily extensible and future-proof.

Two main actors are involved in REAR, namely **providers** and **customers**:
  * **Providers** avertise their resources and services in a standardized format.
  * **Customers**  explore and find resources according to their specific criteria.

REAR enables potential customers to know what is available in the provider clusters, hence enabling a true, dynamic market.
In addition, it aims at automating (and hiding) the technical steps that enable the customer to connect and consume the resources/services agreed in the negotiation phase, such as setting up a peering with [Liqo](https://liqo.io).

Overall, REAR seamlessly integrates with established resource management systems and platforms.


### REAR key characteristics

**REAR** has been designed to be easily extended, in particular with respect to the support of arbitrary resources. In fact, it enables exchanging information about any type of resources and services, ranging from traditional VMs, Kubernetes clusters, services (e.g., DBs), and sensors and actuators (e.g., humidity and temperature sensors).

## Documentation structure

The **REAR** documentation has been organized as follows:

  * [State of the art](./docs/review/README.md)
  * [Overall worklow and main messages](./docs/messages/README.md) 
  * [REAR API and detailed message syntax](./docs/api/README.md)


