# Autonomic Computing: Self-Managing Systems

Autonomic computing refers to systems capable of managing themselves based on high-level objectives set by administrators, thereby reducing the need for constant human intervention. This concept, introduced by IBM, draws an analogy to the human body's autonomic nervous system, which manages vital functions unconsciously. The primary motivation is to address the escalating complexity of IT systems.

## 1. The Vision and Need for Autonomic Computing

* **Complexity Crisis:** Modern IT systems (applications, infrastructure) are becoming exceedingly complex, involving millions of lines of code, heterogeneous components, and extensive interconnections. Managing this complexity manually is approaching the limits of human capability. (Source: Kephart and Chess [source: 2831-2832])
* **Goal:** To create computing systems that can:
    * Install, configure, tune, and maintain themselves.
    * Adapt to changing conditions, workloads, and demands.
    * Handle hardware/software failures and security threats with minimal human oversight.
* **In Cloud Computing:** Autonomic principles are crucial for managing the dynamic, heterogeneous, and distributed nature of cloud resources. They help address challenges like resource allocation, Quality of Service (QoS) assurance, and Service Level Agreement (SLA) violations. (Source: Singh and Chana, Sec 1 [source: 2861-2863])
    * Cloud environments face issues of uncertainty, resource dispersion, heterogeneity, dynamism, and failures, which traditional resource management struggles with.
    * Autonomic systems aim to optimize resource utilization, minimize costs, and ensure performance based on QoS parameters.

## 2. The Autonomic Manager and MAPE-K Cycle

At the core of an autonomic system is the **Autonomic Manager**, an intelligent component that oversees a **Managed Element** (a hardware or software resource). The Autonomic Manager operates through a continuous control loop, often referred to as the **MAPE-K cycle**.

(Source: Kephart and Chess, Fig 2 [source: 2836]; L2.1 [source: 2818]; Singh and Chana, Sec 3.1, Fig 2 [source: 2868-2869])

* **Managed Element:** The system component (e.g., server, database, application, network device) that is being controlled.
* **Autonomic Manager:**
    * **Sensors (Monitor - M):** Collect data (metrics like performance, availability, health state, QoS parameters) from the Managed Element and its environment.
    * **Analyze (A):** Processes and interprets the monitored data. This involves:
        * Data aggregation.
        * Identifying patterns, anomalies, and trends.
        * Predicting future states.
        * Comparing current state against desired states, thresholds, or SLAs.
        * Diagnosing problems.
    * **Plan (P):** Develops a strategy or a sequence of actions to achieve high-level objectives or to correct deviations identified during analysis. This might involve deciding to scale resources, replicate components, migrate workloads, re-route traffic, etc.
    * **Execute (E):** Implements the planned actions, interacting with the Managed Element through **Effectors** to apply the necessary changes.
    * **Knowledge (K):** A shared knowledge base that informs all stages of the MAPE loop. It contains:
        * Policies (defining goals and constraints).
        * System models (representing the managed element's behavior and dependencies).
        * Historical data, logs.
        * Rules for decision-making.

An **Autonomic Element** consists of one or more managed elements controlled by an autonomic manager. These elements interact with each other and with human administrators via their autonomic managers.

## 3. The Four "Self-*" Properties (Aspects of Self-Management)

Autonomic systems exhibit several "self-managing" properties, often referred to as the "self-*" properties. These are the goals of autonomic behavior:

(Source: Kephart and Chess, Table 1 [source: 2834]; L2.1 [source: 2819]; Singh and Chana, Sec 3.2, Table I [source: 2870-2871])

1.  **Self-Configuration:**
    * **Definition:** The ability of components and systems to automatically configure themselves according to high-level policies that specify what is desired, not necessarily how to achieve it. New components should integrate seamlessly, and the rest of the system should adjust automatically.
    * **In Cloud:** Adapting to changes in the environment, such as installing missing or outdated components based on alerts, without human intervention.
    * *Example (Kephart): When a new component is introduced, it learns about the system, registers itself, and other components adapt.*
    * *Example (FC2): An Autonomic Manager (AM) installing missed or outdated components based on system alerts.*

2.  **Self-Optimization:**
    * **Definition:** The ability of components and systems to continually seek opportunities to improve their own performance and efficiency (in terms of performance or cost). They monitor, experiment with, and tune their own parameters.
    * **In Cloud:** Improving performance, such as by dynamically scheduling tasks to optimize resource utilization and complete workloads efficiently, reducing overloading or underloading.
    * *Example (Kephart): Systems proactively seeking and applying the latest updates to improve function.*
    * *Example (FC2, Scenario 1): An AM monitors web app metrics (CPU, response time), analyzes SLO violations, plans the number of VMs to add/remove to meet SLOs while minimizing costs, and executes the VM allocation/deallocation.*

3.  **Self-Healing:**
    * **Definition:** The ability of a system to automatically detect, diagnose, and repair localized software and hardware problems.
    * **In Cloud:** Identifying, analyzing, and recovering from faults automatically to improve performance through fault tolerance and reduce the impact of failures.
    * *Example (Kephart): A system detecting a faulty software module, reverting to an older version, diagnosing the issue, and alerting a developer.*
    * *Example (FC2, Scenario 2): An AM monitors the health of image recognition app instances, checks if the number of healthy instances is above a threshold (e.g., seven), and starts new instances if needed, adding them to a load balancer.*
    * **Common Cloud Failures:** Unexpected configuration changes, resource unavailability, overloading, memory shortages, network failures. (Source: FC2 Answers, Q2.11 [source: 3014-3015])
    * **Handling Techniques:** Check-pointing (restarting failed tasks on other resources), failure forecasting, replication. (Source: FC2 Answers, Q2.11 [source: 3014-3015])

4.  **Self-Protection:**
    * **Definition:** The ability of a system to automatically defend against malicious attacks or cascading failures that remain uncorrected by self-healing measures. It involves anticipating problems based on early warnings and taking steps to avoid or mitigate them.
    * **In Cloud:** Protecting against intrusions and threats by detecting malicious attacks and maintaining system security and integrity.
    * *Example (Kephart): System using early warnings from sensors to anticipate and prevent system-wide failures.*
    

## 4. Autonomic Manager Configuration Policies

High-level objectives and constraints that guide the Autonomic Manager's behavior are defined through policies.
(Source: L2.1 [source: 2820-2827]; Kephart and Chess [source: 2833, 2839])

1.  **Event-Condition-Action (ECA) Policies:**
    * **Format:** `WHEN event OCCURS AND condition HOLDS THEN execute action`.
    * Define specific actions to be taken when certain events occur and specified conditions are met.
    * *Example:* "When 95% of Web servers' response time exceeds 2s AND there are available resources, THEN increase the number of active Web servers."
    * **Challenge:** Can lead to conflicts as the number of policies increases.

2.  **Goal Policies:**
    * Specify criteria that characterize desirable system states, but leave it to the system to determine *how* to achieve that state.
    * *Example:* "The response time of the web server should be under 2s, while that of the application server under 1s."
    * **Challenge:** Require sophisticated planning capabilities within the autonomic manager. May only distinguish between desirable and undesirable states, not offering guidance on the "best" undesirable state if the goal is unachievable.

3.  **Utility Function Policies:**
    * Define a quantitative measure of desirability (utility) for each possible system state. The system aims to choose actions that maximize this utility value.
    * The utility function takes various system parameters as input and outputs a numerical desirability score.
    * *Example:* A function `U = F(ResponseTime_Web, ResponseTime_App)` calculates the utility based on the response times of web and application servers.
    * **Challenge:** Can be extremely difficult to define accurately, as every aspect influencing the decision must be quantified and weighted appropriately.

## 5. Planning in Autonomic Systems

Planning involves using monitoring data to generate a sequence of actions to be performed on the managed element to achieve desired goals or maintain stability.
(Source: L2.1 [source: 2828-2829]; Kephart and Chess [source: 2839])

* **Model-Driven Approach:**
    * A model of the managed system (reflecting its behavior, requirements, goals, architecture) is created and maintained within the Knowledge base.
    * This model is continuously updated with real-time sensor data.
    * The Autonomic Manager uses this model to reason about the system's current and future states and to plan adaptations or corrective actions.
    * If the model becomes invalidated (e.g., due to unforeseen changes), ECA-like rules can be used as repair strategies or to trigger model relearning.
* **Planning Techniques:**
    * Optimization (linear/non-linear programming).
    * Heuristics and Metaheuristics (e.g., genetic algorithms).
    * Control Theory.
    * Markov Decision Processes (MDP).
    * Machine Learning / Deep Learning techniques (for prediction, decision making).
    * Queuing network models, Petri nets, graph-based models for system architecture.

## 6. Autonomic Computing in the Cloud Context

Applying autonomic principles to cloud computing aims to manage the inherent complexity, dynamism, and scale of cloud environments effectively.

### 6.1. Need for Autonomic Cloud Computing
(Source: Singh and Chana, Sec 1.1 [source: 2863-2864])
* Cloud services depend on QoS, but heterogeneity, dynamism, and resource dispersion make fulfilling QoS requirements challenging with traditional methods.
* Autonomic cloud computing can:
    * Effectively allocate resources based on user QoS requirements.
    * Heal unexpected failures automatically.
    * Optimize QoS parameters (e.g., execution time, cost, reliability, resource utilization).
    * Identify and schedule suitable resources for workloads to maximize resource utilization efficiency.
    * Manage scheduling for both homogeneous and heterogeneous workloads.

### 6.2. QoS Parameters in Autonomic Cloud Computing
(Source: Singh and Chana, Sec 6.6, Fig 16 [source: 2951-2953]; FC2 Answers, Q2.1, Q2.3 [source: 2997, 2999])
Research in autonomic cloud computing considers various QoS parameters. The most frequently considered (according to Singh and Chana's review) are:
1.  **Cost** (e.g., monetary cost of resources, execution cost) - *Top considered (20%)*
2.  **Resource Utilization** (e.g., CPU utilization, memory utilization) - *(17%)*
3.  **Execution Time** (e.g., time to complete a workload) - *(17%)*
4.  **Response Time** (e.g., time taken to respond to a user request) - *(16%)*
5.  **Energy** (e.g., energy consumed by resources) - *(15%)*
Others include: Reliability, Accuracy, Deadline, Computation Overhead, CPU speed, Scalability, Availability, Security, SLA Violation rate.

### 6.3. Service Level Agreements (SLAs) and QoS
(Source: Singh and Chana, Sec 3.5 [source: 2880-2882])
* SLAs are crucial agreements between cloud consumers and providers, specifying QoS parameters (price, time, reliability, etc.).
* **Main reasons for SLA violation:** (Source: FC2 Answers, Q2.2 [source: 2998])
    * Unexpected events (Hardware/Software failures).
    * Unstable demand (workload fluctuations).
    * Service composition complexities (degradation in one component affects the whole).
    * Under-provisioning of resources.
    * Conflicting consumer needs or provider/consumer objectives.
* Autonomic systems aim to monitor QoS continuously to detect and prevent SLA violations.

### 6.4. Phases of Autonomic Resource Management in Cloud
(Source: Singh and Chana, Sec 6, Fig 10 [source: 2926-2927])
Autonomic resource management in the cloud can be categorized into several interconnected phases:

1.  **Design of Application:** (Source: Singh and Chana, Sec 6.1 [source: 2927-2935])
    * **Type of Application:** Workload (set of tasks) vs. Workflow (interrelated tasks, often represented by DAGs).
    * **Domain of Application:** Scientific, business, healthcare, HTC, HPC. Different domains have different QoS needs (e.g., healthcare: high throughput, availability; geoscience: security, processing speed, large storage).
    * **Application Presentation:** Language-based (e.g., XML) or Tool-based (e.g., GUI, Petri Nets).
    * **I/O Data Requirements:** Large vs. Small datasets; sequential vs. parallel data input.

2.  **Workload Scheduling:** (Source: Singh and Chana, Sec 6.2 [source: 2936-2937]; FC2 Answers, Q2.5, Q2.6, Q2.7 [source: 3001-3005])
    * Involves two main functions:
        * **Resource Allocation:** Assigning appropriate resources to suitable workloads on time for effective utilization.
        * **Resource Mapping:** Matching workloads with appropriate resources based on QoS requirements (from SLAs) to optimize objectives like cost, execution time, and profit.
    * **Scheduling Architecture:**
        * **Hierarchical:** Multi-level schedulers; higher-level controls lower-level. *Pros:* Can use different scheduling techniques. *Cons:* Central controller is a single point of failure.
        * **Centralized:** A single central controller makes all scheduling decisions. *Pros:* Single point of control. *Cons:* Not scalable, single point of failure.
        * **Decentralized:** Resources assigned based on individual workload requirements; decisions made locally. *Pros:* Scalable. *Cons:* Hard to coordinate decisions, works best for homogeneous workloads/resources.
    * **Scheduling Objective:** Typically optimizing for cost, time, resource utilization, or load balancing.
    * **Scheduling Decision (Timing):**
        * **Static:** Matchmaking of workload to resources based on requirements and availability, decided offline.
        * **Dynamic:** Mapping and execution based on resource provisioning at runtime; provides scalability and reduces resource wastage.
    * **Integration:** Combining results from different execution units or schedulers (especially in SOA-like environments).

3.  **Resource Allocation (Mechanisms):** (Source: Singh and Chana, Sec 6.3 [source: 2939-2940]; FC2 Answers, Q2.8, Q2.9 [source: 3006-3009])
    * Managed by a Cloud Resource Manager (CRM).
    * **Decision Criteria (in distributed scheduling):**
        * **Independent:** Each scheduler acts independently without considering overall resource utilization status.
        * **Mutual:** Schedulers coordinate their decisions (e.g., low-level and high-level schedulers), which can reduce resource contention.
    * **Coordination Mechanism:**
        * **Group-Based:** Resources shared among groups of users with similar requirements. SLAs often defined at the group level. Aims for better performance and fault tolerance.
        * **Market-Based:** Uses negotiation (based on SLAs) for resource provision. Economic entities buy/sell computing resources in a virtual market.
    * **Intercommunication Protocol:** One-to-one (consumer-provider) vs. One-to-many (provider to multiple users).

4.  **Monitoring:** (Source: Singh and Chana, Sec 6.4 [source: 2943-2944]; FC2 Answers, Q2.10 [source: 3010-3013])
    * Essential for performance optimization by tracking resource utilization, system status, and service execution.
    * **Levels of Monitoring:**
        * **Execution Monitoring (Process/Task Level):** Tracks status (started, queued, completed, failed). Can be *active* (agent continually checks and modifies) or *passive* (local agent monitors against QoS in SLAs).
        * **Status Monitoring (Infrastructure Level):** Autonomic Elements monitor system performance (resource/memory utilization, network usage, SLA deviations, resource uptime) to avoid SLA violations.
        * **Service Monitoring (Workload as Service Executions):** Collects info on free resources, utilization, and load. Checks if execution meets QoS in SLAs.
        * **Resource Usage Monitoring (Infrastructure Level):** Collects usage via metrics (resource/memory utilization) to avoid under/overloading and manage QoS (security, availability, performance). Balances consumer goals (min cost/time) and provider goals (min resources used).

5.  **Self-Management (applying the self-* properties):** (Source: Singh and Chana, Sec 6.5 [source: 2946-2947])
    * This phase applies the four self-* properties (Healing, Configuration, Optimization, Protection) using the insights from Monitoring and decisions from Planning.
    * The most investigated properties in autonomic cloud research are Self-Optimization and Self-Configuration, followed by Self-Healing and Self-Protection. (Source: FC2 Answers, Q2.4 [source: 3000])

## 7. Architectural Considerations for Autonomic Systems
(Source: Kephart and Chess [source: 2835-2840])
* Autonomic systems are envisioned as interactive collections of **autonomic elements**.
* System self-management arises from both the internal management of individual elements and the interactions *between* these elements.
* **Life Cycle of an Autonomic Element:** Includes design, implementation, test/verification, installation, configuration, optimization, upgrading, monitoring, problem determination, recovery, and uninstallation. Each stage presents unique challenges.
* **Relationships Among Autonomic Elements:** Also have a life cycle:
    * **Specification:** Describing capabilities and requirements in standard formats.
    * **Location:** Discovering needed services/partners (e.g., via directory services).
    * **Negotiation:** Reaching agreements (SLAs) for service provision (can range from simple posted-price to complex multi-attribute negotiations).
    * **Provision:** Allocating internal resources based on agreements.
    * **Operation:** Operating under the agreement, with ongoing monitoring.
    * **Termination:** Ending the agreement and releasing resources.
* **System-Wide Issues:** Security, privacy, trust, and the emergence of "middle agent" services (brokers, matchmakers, monitors).
* **Goal Specification:** A critical human role is providing high-level goals and policies. Ensuring these are correct and that systems behave reasonably even with imperfect goals is a major challenge.

## 8. Challenges and Future Directions
(Source: Kephart and Chess [source: 2840-2844]; Singh and Chana, Sec 9 [source: 2972-2976])
* **Engineering Challenges (Kephart):** Programming autonomic elements, testing/verification in complex environments, installation/configuration bootstrapping, monitoring without excessive overhead, problem determination, graceful recovery, managing upgrades, handling complex life cycles of elements and their relationships, system-wide security/privacy/trust, robust goal specification by humans.
* **Scientific Challenges (Kephart):** Developing behavioral abstractions and models for emergent behavior, theories of robustness, learning and optimization in multi-agent adaptive environments, negotiation theory, automated statistical modeling.
* **QoS-Aware Autonomic Cloud Computing Challenges (Singh & Chana):**
    * Effective SLA management and violation detection.
    * Dynamic and efficient autonomic resource provisioning.
    * Ensuring diverse QoS requirements are met.
    * Optimizing resource scheduling in heterogeneous and dynamic environments.
    * Addressing energy efficiency.
    * Improving decision-making with advanced data mining/machine learning.
    * Developing decentralized, scalable autonomic techniques.
    * Need for real-world testing and validation of proposed mechanisms.



