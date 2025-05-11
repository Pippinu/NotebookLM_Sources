# Chapter 2: Enabling Technologies for Cloud Computing

This chapter explores the fundamental technologies and architectural concepts that underpin cloud computing. These include distributed computing principles, various architectural styles, service-oriented approaches like SOA and microservices, and the concept of autonomic computing for self-managing systems.

## 1. Distributed Computing

Distributed computing is a foundational model for cloud computing. It involves breaking down computation into units that can be executed concurrently on different computing elements, which may be in different locations and have heterogeneous hardware/software.

**Key Definitions:**

* **DEFINITION**: A distributed system is a collection of independent computers, connected by a network, which communicates only by passing messages, that appears to its users as a **single coherent system**.

**Tanenbaum:** "A distributed system is a collection of independent computers that appears to its users as a **single coherent system**." (Focuses on unified usage and resource aggregation).

**Coulouris:** "A distributed system is one in which components located at networked computers communicate and coordinate their actions only by passing messages." (Focuses on the networked nature and message-based communication).

**Characteristics often implied by "Distributed":**
* **Computation spread** across different computing elements (processors on different nodes, same computer, or cores).
* Computing elements may be in **different physical locations**.
* **Heterogeneity** in hardware and software features is common (Different hardware and software (OS, Programming Languages)).

**Components of a Distributed System (Layered View):**
1.  **Applications:** End-user applications and services.
2.  **Middleware:** Provides a uniform environment for developing and deploying distributed applications, hiding the heterogeneity of underlying layers.
3.  **Operating System:** Manages hardware resources, provides basic IPC services, process scheduling, and management. \
Allows easy harnessing of heterogeneous components and their organization into
a coherent and uniform system.
4.  **Hardware & Networking:** Physical infrastructure including computers, servers, and network components (routers, switches, etc.). \
Taken together these two layers become the platform on top of which software is deployed to turn a set of networked computers into a distributed system.

**Cloud Computing as a Specialized Distributed System:**
Cloud computing is a form of distributed computing that introduces specific utilization models for remotely provisioning scalable and measured resources. 

The layered view maps to cloud service models:
* Applications (SaaS)
* Middleware/Frameworks for Cloud Application Development (PaaS)
* Hardware, OS, Virtual Hardware, Networking, OS Images, Storage (IaaS)

## 2. Architectural Styles for Distributed Systems

Using Client-Server, P2P and Message Message Queuing as architectural styles examples, Architectural styles define:
* **Components** (unit of software that encapsulates a function of the system) and **Connectors** (communication mechanism between components) **Vocabulary**
    * *Example*: Client and Servers, Peers, Producer and Consumer; Network Protocols to handling requests and response, Message Queue and Message Broker.
* Components and Connectors **roles**
    * *Example*: Initiate requests, Presenting user interface, Handling user interaction, Create and send or Receive and process messages.
* **Components distribution** among multiple machines
    * *Example*: Centralized or Distributed Servers, Producers and Consumers on separate machines, Message Queue/Broker on dedicated or distributed infrastructure.
* **Pattern for structural organization**.
    * *Example*: 
        * Hierarchical, centralized server(s) in Client-Server; 
        * Decentralized, symmetric roles, direct peer-to-peer interaction in P2P; 
        * Decoupled, asynchronous communication managed by the message queue/broker in Message Queuing.

### A. Software Architectural Styles (Logical Organization) (CONTINUE FROM HERE)

Software architectural styles are based on the **logical arrangement** of components. They are helpful because they provide an **intuitive view** of the whole system, **despite its physical deployment**. They also identify the **expected interaction patterns** between them.

The following are common software architectural styles that focus on the logical arrangement of software elements:

1.  **Data-Centered Architectures:** Data is fundamental element of the system, Access to shared data is core characteristic.
    * **Repository Style:** Features a central data structure (repository) and independent components that operate on this data.
        * **Database Systems**: Components triggers operations on central data structure (Database).
        * **Blackboard Systems**: Central data structure (blackboard) triggers components operation to execute. 
            * Used in AI, speech recognition.
2.  **Data-Flow Architectures:** Orderly motion of data from component to component, output of one is input to the next.
    * **Batch Sequential Style:** Ordered sequence of separate programs; Whole output is required to be processed as input. 
        * *Common in scientific computing for pre-filter, analyze, post-process workflows.*
    * **Pipe-and-Filter Style:** Data processed a sequence of "filters" (components of the processing chain) connected by "pipes" (data streams, often in-memory buffers); As long as a consumable amount of output data is ready, it can be used as input.
        * *Examples:*  MapReduce paradigm (Input -> Splitting -> Mapping -> Shuffling -> Reducing -> Final Result).
3.  **Virtual Machine Architectures:**
    * An abstract execution environment (virtual machine) simulates features not available in the hardware/software, making applications portable.
    * **Rule-Based Style:** An inference engine uses rules/predicates and input facts/assertions to transform data or infer properties. Used in process control, network intrusion detection.
    * **Interpreter Style:** An engine interprets a pseudo-program. Used for high-level programming languages (Java, C#) and scripting languages.
4.  **Call and Return Architectures:**
    * Systems organized by components connected mainly by method calls.
    * **Top-Down Style (Main Program and Subroutine):** A main program calls subprograms/procedures. Tree-like execution structure.
    * **Object-Oriented Style:** Systems are classes and objects; data and operations are coupled. Encapsulation and integrity are key.
    * **Layered Style:** System designed in layers, each providing a different level of abstraction and interacting primarily with adjacent layers via defined protocols/interfaces. *Examples:* OS kernels, TCP/IP stack.
5.  **Independent Components Architectures:**
    * Systems modeled as independent components with their own lifecycles, interacting to perform activities.
    * **Communicating Processes:** Independent processes use IPC facilities (RPC, Web Services, REST, Distributed Objects) for coordination. Can be physically organized as Client/Server, P2P, SOA.
    * **Event-Based Systems (Publish-Subscribe):** Loosely coupled components. Components publish events; other components subscribe and provide callbacks that execute when events are activated. Promotes open systems. *Example:* A component `P1` emits event `e1`, an event/message broker delivers it to subscribed services `S1`, `Sn`.

### B. System Architectural Styles (Physical Organization)

These describe the physical organization and deployment of components.

1.  **Client/Server Architecture:**
    * **Components**: Clients and a Server, interacting over a network.
    * **Communication** is typically client-initiated (request-response).
    * **Conceptual Tiers:** 
        * Presentation: Interaction with the user.
        * Application Logic: Core functionality of the application.
        * Data Storage: Management and persistance of data.
    * **Thin-Client Model:** Application logic and Data Storage (Server); Presentation (Client).
    * **Fat-Client Model:** Data Storage (Server); Application and Presentation (Client).
    * **Multi-Tiered Models:**
        * **Two-Tier (Classic Model):** Client (presentation) and Server (application logic + data storage). Suffers scalability issues.
        * **Three-Tier:** Separates presentation, application logic, and data storage into distinct tiers. More scalable as tiers can be distributed. 
            *Example:* Web browser (client), application server (business logic), database server (data storage).
        * **N-Tier**: Tree-tier with load balancers, multiple app servers, and sharded/replicated databases.

2.  **Peer-to-Peer (P2P) Architecture:**
    * Symmetric architecture where all components (peers) play the same role, acting as both clients and servers.
    * Suitable for highly decentralized systems, offering better scalability in terms of the number of peers.
        * *Examples:* File-sharing applications (BitTorrent).

### C. Models for Interprocess Communication (IPC)

**IPC** (Interprocess Communication) are mechanisms that allow different processes running on a computer system to **communicate** and **share data** with each other, it is what test together different components of a distributed system making them to act as a single system.

1.  **Message-Based Communication:**
    * A foundational concept where discrete amounts of information (messages) are passed between entities.
    * **Message Passing:** Explicit encoding of data into messages (e.g., MPI).
    * **Remote Procedure Call (RPC):** Extends procedure calls across processes. Messages convey procedure info, parameters, and return values (marshaling/unmarshaling).
    * **Distributed Objects:** RPC for object-oriented paradigms (e.g., CORBA, Java RMI, .NET Remoting). Messages handle remote method invocation.
    * **Web Services:** RPC over HTTP (e.g., SOAP, REST). Messages (often XML/JSON) for requests and responses.

2.  **Interaction Patterns for Message-Based Communication:**
    * **Point-to-Point:** Direct addressing between a sender and a receiver. Can be direct communication or queue-based.
    * **Publish-Subscribe** (Called "Event-Based" in slides): Publishers emit messages on specific topics/events; subscribers register interest and receive relevant messages. Decouples publishers and subscribers. Uses push (publisher notifies) or pull (subscriber checks) strategies.
    * **Request-Reply:** For each message sent, a reply is expected. Common in point-to-point models.

## 3. Service-Oriented Architecture (SOA)

SOA is an architectural style that structures an application as a collection of interacting services. It emphasizes **service-orientation**, where services are the primary building blocks.

**What is a Service (in SOA)?**
* A logical representation of a repeatable activity with a specified outcome (e.g., check customer credit).
* **Self-contained** (does not rely on other services) and often a "**black box**" to consumers (which interact using exposed interface, like API).
* May be **composed** of other services.
* **Characteristics:**
    * **Interfaces** to interact with the service are kept minimal to foster **reuse** and **simplify interaction**.
    * **Autonomous services**, they can be integrated in different systems at the same time and indipendently can handle failures.
    * Services share **schema** and **contracts**, not interfaces and classes (like OOP), so heterogeneity of distributed systems is promotod.
        * Sceham: Defines structure of data exchanged between services.
        * Contract: Defines structure of messages the service can send and receive.
    * **Compatibility** is determined by policy (semantic compatibility beyond structural).

**SOA Principles & Guiding Concepts:**
* **Standardized Communication:** Services adhere to a communication agreement.
* **Loose Coupling:** Services are self-contained, minimizing dependencies.
* **Abstraction:** Services hide their logic; defined by contracts.
* **Reusability:** Designed as components, services can be reused.
* **Autonomy:** Services control their encapsulated logic.
* **Lack of State (Statelessness):** Services ideally maintain no state between requests from consumers, enhancing reusability and scalability.
* **Discoverability:** Services defined by discoverable metadata (e.g., via UDDI registry).
* **Composability:** Services can be *orchestrated* or *choreographed* to create complex operations.

**Roles in SOA:**
1.  **Service Provider:** Maintains and makes services available. Defines and publishes service **contracts** (communication system) in a registry along with service **schemas** (data formats).
2.  **Service Consumer:** Locates service descriptors in registry, develops client components to bind and use the service.
3.  **Service Registry:** Stores **service descriptors** which defines everything a consumer need to know to use the service, it also provide **service discovery** mechanism to allow consumer to search for specific service.

Service Providers and Consumer can belogn to different organizations or business domains.

**Service Coordination:**

![alt text](./images/service_orchestration_choreography.png)

* **Service Orchestration:** A central entity (*orchestrator*) coordinates the interaction of services to execute a business process. 
    * *Example*: Travel planner service invoking flight, hotel, and car rental services.
* **Service Choreography:** Coordinated interaction (message exchange) of services **without a single point of control**. Each service knows its role in the interaction. 
    * *Example*: Online payment involving a seller service and a credit card company service.

## 4. Web Services (SKIPPABLE)

Web services are a prominent technology for implementing SOA, enabling interoperability across platforms using internet standards.
* **SOAP (Simple Object Access Protocol):** XML-based protocol for exchanging structured information in Web service communication (method invocation, results).
* **WSDL (Web Service Description Language):** XML-based language to describe the interface of a Web service (operations, parameters, data types).
* **UDDI (Universal Description, Discovery, and Integration):** A registry for businesses worldwide to list themselves and their Web services. (Less common now).
* **REST (Representational State Transfer):** An architectural style often used as a simpler alternative to SOAP for web services. Uses standard HTTP methods (GET, POST, PUT, DELETE) and typically JSON or XML for data exchange. Focuses on resources identified by URIs.

Web 2.0 technologies (AJAX, JSON) further enhance web applications that consume these services, providing richer user experiences.

## 5. Microservice Architecture

An architectural style that structures an application as a collection of small, autonomous, and independently deployable services. Services are typically organized around business capabilities and often owned by small, dedicated teams.

**Core Ideas (from "Microservices Patterns" by Chris Richardson & "The Art of Scalability"):**
* **X-axis Scaling (Duplication):** Cloning the application across multiple identical instances. Each instance runs the entire application code and handles a portion of the overall load, typically behind a load balancer.
    * *Example:* If one instance of your microservice can handle 100 requests per second, adding three more identical instances behind a load balancer theoretically allows you to handle 400 requests per second.
* **Y-axis Scaling (Functional Decomposition):** Splitting application (*Monolith*) into services (*Microservice*) based on function.
    * *Example:* Instead of a monolithic "*Order Management*" service, you might break it down into "*Order Processing*," "*Inventory Management*", "*Payment Processing*," and "*Shipping Management*" microservices.
* **Z-axis Scaling (Data Partitioning):** Splitting data across multiple istances of the application.
    * *Example:* Customer database can be splitted based on the first letter of the customer's last name, each letter is handled by an instance.

* **Services as Units of Modularity:** Each service has a well-defined API, an impermeable boundary.
* **Each Service Has Its Own Database:** Promotes loose coupling, allowing independent schema evolution and preventing one service from blocking another via database locks. This doesn't necessarily mean a separate database *server* for each, but logically distinct datastores.
* **Enables Continuous Delivery/Deployment:**
    * Small service size improves testability and maintainability.
    * Independent deployability allows for rapid, frequent updates.
    * Autonomous teams can develop, deploy, and scale their services independently.

**Benefits of Microservices:**
* Enables continuous delivery/deployment of large, complex applications.
* Services are small and easily maintained.
* Services are independently deployable and scalable.
* Enables autonomous, loosely coupled teams.
* Allows easier experimentation with and adoption of new technologies for different services.
* Better fault isolation (failure in one service is less likely to bring down the entire system).

**Drawbacks of Microservices:**
* **Complexity of Distributed Systems:** Developers must handle inter-service communication, partial failures, and latency.
* **Finding Service Boundaries:** Decomposing a system into the "right" set of services is challenging. Incorrect decomposition can lead to a "distributed monolith."
* **Data Management:** Implementing transactions and queries that span multiple services is complex (requires patterns like Sagas, API Composition, CQRS).
* **Deployment Complexity:** Managing many moving parts in production requires a high level of automation (e.g., using PaaS, Docker orchestration like Kubernetes).
* **Coordinating Multi-Service Deployments:** Deploying features that span multiple services requires careful coordination.
* **When to Adopt:** May not be suitable for initial versions of applications, especially for startups where rapid iteration on a monolith is often faster.

**Microservices vs. Monolithic Architecture:**
* **Monolith:** Single deployable unit.
    * *Pros (initially):* Simple to develop, test, deploy, and scale (horizontally).
    * *Cons (as it grows):* Becomes complex, slow to develop/build/start, difficult to scale components with conflicting needs, poor fault isolation, technology stack lock-in ("monolithic hell").
* **Microservices:** Collection of small, independent services.
    * Addresses many cons of large monoliths but introduces distributed system complexities.

**Comparing Microservices and SOA (from "Microservices vs. SOA" by Mark Richards):**

| Feature                        | SOA (Service-Oriented Architecture)                                     | Microservices Architecture                                            |
| :----------------------------- | :---------------------------------------------------------------------- | :-------------------------------------------------------------------- |
| **Service Granularity** | Can range from small application services to large enterprise services. Often coarse-grained. | Generally small, fine-grained, single-purpose services.             |
| **Component Sharing** | Share-as-much-as-possible (e.g., enterprise services, global data model). | Share-as-little-as-possible (favors bounded contexts, data per service). |
| **Inter-service Communication**| Often uses "smart pipes" like an Enterprise Service Bus (ESB) with message processing logic. Heavyweight protocols (SOAP, WS*). | Typically "dumb pipes" (message brokers) or direct service-to-service communication. Lightweight protocols (REST, gRPC). |
| **Data Management** | Often involves global data models and shared databases.                  | Each service typically owns its own database/datastore.               |
| **Service Taxonomy** | More formal and layered (e.g., Business, Enterprise, Application, Infrastructure services). | Simpler (e.g., Functional services, Infrastructure services).         |
| **Service Ownership** | Often different owners for different service types (business, shared services teams, app dev teams), requiring more coordination. | Typically application development teams own both functional and infrastructure services, enabling more autonomy. |
| **Middleware** | Relies on messaging middleware (integration hub/ESB) for coordination, transformation, routing. | Generally avoids heavy middleware; may use an API Gateway (simpler facade). |
| **Service Orchestration vs. Choreography** | Uses both. ESB often acts as an orchestrator.                          | Favors service choreography (services call each other directly or via events) over central orchestration. |
| **Heterogeneous Interoperability** | Strong support via ESB for protocol transformation and integrating diverse systems. | Simpler integration, often relying on common protocols like REST. Less emphasis on protocol transformation. |
| **Contract Decoupling** | Supported via ESB (message transformation, enhancement).              | Generally not supported; services and consumers expect matching contracts. |
| **Application Scope** | Well-suited for large, complex, enterprise-wide systems needing integration with many heterogeneous applications. | Better suited for smaller, well-partitioned web-based systems or where independent scaling/deployment is key. |

**Key Distinguishing Concepts:**
* **Bounded Context (Microservices):** Coupling of a service and its associated data as a self-contained unit with minimal dependencies.
* **API Gateway (Microservices):** An optional layer that acts as a single entry point for client requests, routing them to appropriate backend services. Can handle concerns like authentication, rate limiting.
* **Enterprise Service Bus (ESB) (SOA):** A middleware component that facilitates communication and integration between services, often handling message routing, transformation, and orchestration.

## 6. Autonomic Computing

Autonomic computing systems are systems that can manage themselves based on high-level objectives set by administrators, reducing the need for manual intervention.

**MAPE-K Cycle:** The core control loop for an autonomic manager:
1.  **Monitor (M):** Collects data (metrics, system states) from the managed element (resource/system) using sensors.
2.  **Analyze (A):** Processes and interprets monitored data to detect patterns, anomalies, predict future states, and identify issues or opportunities for optimization. Compares data with thresholds or desired states.
3.  **Plan (P):** Develops a strategy or a sequence of actions to achieve the desired objectives or to correct deviations, based on the analysis.
4.  **Execute (E):** Implements the planned actions, interacting with the managed element through effectors to apply necessary changes (e.g., allocate/deallocate VMs, reroute traffic).
5.  **Knowledge (K):** A shared knowledge base containing policies, system models, historical data, and other information used by the M, A, P, and E components to make decisions.

**Autonomic Element:** Consists of one or more managed elements controlled by an autonomic manager. It uses sensors to monitor and effectors to act upon the managed element.

**Self-* Properties of Autonomic Systems:**
* **Self-Configuration:** Ability to adjust system configurations seamlessly to adapt to varying and unpredictable conditions.
* **Self-Healing:** Ability to detect, diagnose, and repair problems to remain functional when faults arise.
* **Self-Protection:** Ability to detect threats and take appropriate actions to protect the system from attacks or unauthorized access.
* **Self-Optimization:** Ability to continuously monitor and adjust resources or parameters for optimal operation (e.g., performance, cost-efficiency).

**Autonomic Manager Configuration Policies:** Define high-level objectives and guide the manager's behavior.
1.  **Event-Condition-Action (ECA) Policies:**
    * Format: `WHEN event OCCURS AND condition HOLDS THEN execute action`.
    * *Example:* "When 95% of Web servers' response time exceeds 2s AND there are available resources, THEN increase the number of active Web servers."
    * Can lead to conflicts as the number of policies increases.
2.  **Goal Policies:**
    * Specify criteria that characterize desirable states, leaving the system to determine how to achieve them.
    * *Example:* "The response time of the web server should be under 2s."
    * Require planning to determine how to achieve the goal. May only distinguish between desirable/undesirable states.
3.  **Utility Function Policies:**
    * Define a quantitative level of desirability (utility) for each possible system state. The system aims to maximize this utility.
    * Takes various parameters as input and outputs a utility value.
    * *Example:* A function `U = F(ResponseTime_Web, ResponseTime_App)` returns the utility for given response times.
    * Can be very complex to define accurately, as all influencing factors must be quantified.

**Planning in Autonomic Systems:**
* Involves using monitoring data (from sensors) to produce a series of changes (actions) to be effected on the managed element.
* **Model-Driven Approach:**
    * A model of the managed system (reflecting its behavior, requirements, goals) is created and maintained.
    * The model is updated with sensor data.
    * The model is used to reason about the system and plan adaptations.
    * If the model is invalidated, ECA-like rules can serve as repair strategies.
* **Planning Techniques:** Optimization (linear/non-linear programming), heuristics, metaheuristics (e.g., genetic algorithms), Markov Decision Processes (MDP), Machine Learning/Deep Learning techniques.

