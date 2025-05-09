# Summary of NIST SP 500-292: Cloud Computing Reference Architecture

NIST Special Publication 500-292 provides a vendor-neutral conceptual model and taxonomy for cloud computing. Its goal is to facilitate understanding of cloud operational intricacies and provide a basis for discussing requirements, structures, uses, and standards.

## 2. Conceptual Reference Model & Major Actors
The reference architecture is actor/role-based and identifies **five** major actors which roles and responsibilities vary based on the service mode (IaaS, PaaS, SaaS):


* **Cloud Consumer:**
    * **Person/Organization** that maintains a business relationship and uses services from Cloud Providers.

* **Cloud Provider (CSP):**
    * **Person/Organization/Entity** responsible for making a service available to Cloud Consumers.
    * Major activity areas: Service Deployment, Service Orchestration, Cloud Service Management, Security, and Privacy.

* **Cloud Auditor:**
    * Independent **third-party entity** that assesses cloud services for performance, security, and conformance to standards and policies (e.g., security controls, privacy impact).

* **Cloud Broker:** 
    * **Entity** that manages the use, performance, and delivery of cloud services, and negotiates relationships between Cloud Providers and Cloud Consumers.
    * Can offer services in **three categories**:
        * **Service Intermediation:** Enhances a given service or provides value-added services (e.g., identity management, security enhancements).
        * **Service Aggregation:** Combines and integrates multiple services into new services, ensuring data integration and security.
        * **Service Arbitrage:** Similar to aggregation but services are not fixed; the broker has flexibility to choose services from multiple providers (e.g., for best pricing or performance).

* **Cloud Carrier:** Intermediary that provides connectivity of cloud services from Cloud Providers to Cloud Consumers (e.g., network providers).

* **Interactions between roles:**
    * Consumers can request services directly from Providers or via Brokers.
    * Auditors interact with Consumers and Providers for assessments.
    * Carriers provide the network link between Consumers and Providers.

## 3. Scope of Control (Provider vs. Consumer)
The level of control a consumer has over resources depends on the service model:
* **Application Layer:** Used by SaaS consumers; installed/managed by PaaS/IaaS consumers and SaaS providers.
* **Middleware Layer:** Used by PaaS consumers; installed/managed by IaaS consumers or PaaS providers; hidden from SaaS consumers.
* **Operating System Layer:** Hidden from SaaS/PaaS consumers. IaaS consumers manage guest OS(s); IaaS providers control the host OS.
    * **SaaS:** Consumer has minimal control (user-specific application configurations).
    * **PaaS:** Consumer controls deployed applications and some hosting environment configurations.
    * **IaaS:** Consumer controls OS, storage, deployed applications, and some networking components.

## 4. Architectural Components (Cloud Provider Activities)

### 4.1. Service Deployment
**Refers to how the cloud infrastructure is operated and made available**. 

The **four** deployment models are:
* **Public Cloud:** Infrastructure is available to the general public, owned by a cloud services seller.
* **Private Cloud:** Infrastructure is for exclusive use by a single organization. Can be on-site or outsourced (on-premises and off-premises, respectively).
* **Community Cloud:** Infrastructure is shared by several organizations with common concerns (e.g., mission, security). Can be on-site or outsourced (on-premises and off-premises, respectively).
* **Hybrid Cloud:** Composition of two or more distinct cloud infrastructures (private, community, or public) bound by technology enabling data/application portability.

### 4.2. Service Orchestration

![alt text](./images/service_orchestration.png)

**The arrangement, coordination, and management of computing resources to provide cloud services.** 

It involves a **three-layered** model:
* **Service Layer: Defines interfaces for consumers to access SaaS, PaaS, and IaaS.**

    Services can be:
    * **Interdependent**:  Operating in a connected way, where one cloud service layer relies on or utilizes the capabilities and functionalities provided by other cloud service layers within the same orchestration model. SaaS application can be built on top of PaaS components, PaaS components can be built on top of IaaS components.
    * **Standalone**: Operating independently and without requiring or relying on other cloud service layers within the same orchestration model. A consumer can directly access and utilize this service without needing to interact with underlying PaaS or IaaS components.
* **Resource Abstraction and Control Layer:** Contains software components (e.g., hypervisors, VMs, virtual storage) that abstract physical resources and manage their access, allocation, and monitoring. This layer enables resource pooling, dynamic allocation, and measured service.
* **Physical Resource Layer:** Includes all physical computing resources (hardware: CPUs, memory, networks, storage) and facility resources (HVAC, power, communications).

### 4.3. Cloud Service Management (SISTEMARE FROM NOW ON)
Cloud Service Management includes all of the service-related functions that are necessary for the 
management and operation of those services required by or proposed to cloud consumers. 

It includes:
* **Business Support:** Business Support entails the set of business-related services dealing with clients and supporting processes. It includes the components used to run business operations that are client-facing:
    * Customer Management: Manage customer accounts, open/close/terminate accounts. 
    * Contract Management: Manage service contracts
    * Pricing & Rating: Evaluate cloud services and determine pricing rules based on a user's profile, etc.
* **Provisioning and Configuration:**
    * Rapid Provisioning (automated deployment)
    * Resource Changing (upgrades, repairs, adding nodes)
    * Monitoring & Reporting (virtual resources, cloud operations, performance)
    * Metering (tracking usage for billing)
    * SLA Management (definition, monitoring, enforcement)
* **Portability and Interoperability:**
    * **Data Portability:** Ability to copy data into/out of a cloud or use bulk transfer.
    * **Service Interoperability:** Ability to use data and services across multiple clouds with a unified interface.
    * **System Portability:** Ability to migrate VM instances, images, applications, and content between providers. Requirements vary by service model.

### 4.4. Security
A cross-cutting concern spanning all layers and actors. Key considerations:
* **Service Model Perspectives:** Different service models (SaaS, PaaS, IaaS) present different attack surfaces and security needs (e.g., browser security for SaaS, hypervisor security for IaaS).
* **Deployment Model Implications:** Security concerns vary by deployment model (e.g., workload isolation is more critical in public clouds than private clouds; access boundaries differ for on-site vs. outsourced clouds).
* **Shared Security Responsibilities:** Both providers and consumers share security responsibilities. The split depends on the service model and who has control over specific resources.

### 4.5. Privacy
Cloud providers must protect the proper and consistent collection, processing, communication, use, and disposition of Personal Information (PI) and Personally Identifiable Information (PII). Cloud computing introduces additional privacy challenges.

## 5. Cloud Taxonomy
The document provides a four-level taxonomy to classify cloud computing concepts:
* **Level 1: Role** (e.g., Cloud Consumer, Cloud Provider)
* **Level 2: Activity** (e.g., Service Deployment, Service Orchestration)
* **Level 3: Component** (e.g., Private Cloud, Service Layer, Data Portability)
* **Level 4: Sub-component** (e.g., Electronic Transfer under Cloud Distribution)

This taxonomy provides a controlled vocabulary and hierarchical structure for describing cloud computing elements unambiguously.
