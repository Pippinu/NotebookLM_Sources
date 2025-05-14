These slides introduce the concepts of IT automation and orchestration, contrasting them with autonomic computing, and then provide practical examples using Ansible for automation and Kubernetes for orchestration.

### IT Operation Automation (ITOA) 

Focuses on reducing human interaction and manual processes in IT operations by the use of software tools to automate repetitive tasks, orchestrate workflows, and integrate automation into existing systems.

* **Benefits:** **Improved efficiency/productivity** (freeing up IT staff), **reduced human error** (leading to more accurate/consistent results), and **saved time/costs.**

### Challenges Addressed by Automation & Autonomic Computing

* **Datacenter Challenges:** Server consolidation (VM migration), guaranteeing SLAs, managing failures (discover, remediate, fix), software updates, and vulnerability/bug fixes.
* **Large-Scale Infrastructure/Cloud Application Challenges:** Infrastructure node configuration, application component configuration, updates/patching (in the right order), backup, and CI/CD (Continuous Integration/Continuous Delivery).

Most of these challenges benefit from a **combination of both**:
* Automation provides the tools to do things repeatably and reliably. 
* Autonomic computing provides the "brain" to decide what to do, when to do it, and to learn and adapt, often using automation tools to carry out its decisions. 

The line can be blurry, as sophisticated automation often incorporates some level of decision-making logic, and autonomic systems rely on automation for execution.

### Automation Tools and General Automation**

A wide array of automation tools are exist (Ansible, Terraform, Kubernetes, ...), highlighting the rich ecosystem available.

**In data centers**, specialized platforms automate tasks like provisioning, configuration, patching, and monitoring. \
CI/CD is a key area of automation. The diagram shows a VIM (Virtual Infrastructure Manager) managing hypervisors on physical servers.

#### Ansible as an Automation Example
* **Ansible Overview (Slide 10):** An open-source IT configuration management, deployment, and orchestration tool.
    * **Agentless:** No software needs to be installed on managed hosts.
    * **Push Model (Default):** The control server prepares instructions (in "Playbooks" using "Modules") and pushes them to managed hosts, typically over SSH (Linux/UNIX) or WinRM (Windows). It can be reverted to a pull model.
    * **Components:** Users interact with Ansible, which uses a Host Inventory, Playbooks, Core Modules, Custom Modules, and Connection Plugins to manage hosts (physical servers, VMs, or cloud instances).
* **Agentless Architecture (Slide 11):** Emphasizes no overhead on managed hosts when management isn't active, making it suitable for high-security or high-performance environments.
* **Push Model Details (Slide 12):** Ansible modules (code for tasks) are passed to remote machines. Remote machines don't see or affect other machines' configurations.
* **Network Automation (Slide 13):** For devices that can't run code (like network devices), module code is executed locally on the Ansible control node, interacting with the device via its API.
* **Playbooks (Slides 14-15):**
    * YAML-based files for configuration management and multi-machine deployment.
    * Repeatable and reusable.
    * Can declare configurations, orchestrate ordered steps across multiple hosts, and launch tasks synchronously or asynchronously.
    * Define the desired state and can be idempotent (running them multiple times has the same effect as running them once).
    * Consist of 'plays' (targeting a set of hosts from the 'inventory'), and each play has 'tasks'.
    * Each task calls an Ansible module (e.g., simple like placing a file, complex like spinning up AWS CloudFormation infrastructure).
* **Core Modules (Slide 16):** Designed for easy desired state configuration. They check if a task needs to be done before executing (e.g., only starts a webserver if it's not already running). This idempotency ensures efficiency.
* **Playbook Example (Slide 17):** A YAML playbook with two plays:
    * "Update web servers": Targets `webservers`, ensures `httpd` (Apache) is at the latest version using the `ansible.builtin.yum` module, and writes an Apache config file using `ansible.builtin.template`.
    * "Update db servers": Targets `databases`, ensures `postgresql` is at the latest version and is started using `ansible.builtin.yum` and `ansible.builtin.service`.
* **Complex Automation Example (Slides 18-20):** Illustrates a multi-tier web application (load balancers, application servers, database servers, content servers, monitoring system). Ansible can automate a complex rolling update process across this cluster, involving steps like:
    * Consulting configuration repositories.
    * Configuring base OS.
    * Identifying servers to update.
    * Signaling monitoring/load balancers for outage/removal from pool.
    * Stopping/updating/starting application servers.
    * Running tests.
    * Signaling load balancers/monitoring to re-integrate servers.
    * Repeating for other servers/tiers.
    * Sending reports.

5.  **Automation and Autonomic Computing (Slide 21)**
    * This slide connects automation (specifically Ansible playbooks) back to the self-\* properties of autonomic computing:
        * **Self-configuration:** Once a reconfiguration is decided (by an autonomic manager), a playbook can deploy the new system configuration.
        * **Self-optimization:** If the decision is to add new servers, a playbook can execute all required actions.
        * **Self-healing:** If server/application performance degrades, a playbook can detach and restart it.
        * **Self-protection:** If a system is under attack due to a vulnerability, a playbook can deploy a rolling update to patch it.
    * The key idea is that the "Execute" phase of the MAPE-K loop can be implemented using automation tools like Ansible.

6.  **Orchestration (Slides 22-26)**
    * **Orchestration vs. Automation (Slide 23):**
        * **Automation:** Setting up *one task* to run on its own.
        * **Orchestration:** Automating a *series of individual tasks* to work together as a process or workflow.
    * **Contexts of Orchestration (Slide 24):** Can refer to service orchestration, IT infrastructure orchestration, or container orchestration.
    * **Container Orchestration (Slides 25-26):**
        * Allows defining how to select, deploy, monitor, and dynamically control multi-container applications.
        * Manages runtime aspects: Deploy, Run, and Maintain phases.
        * **Key Features:** Resource limit control, scheduling, load balancing, health checks, fault tolerance, and auto-scaling.
        * A diagram shows the container lifecycle (Acquire, Build, Deliver, Deploy, Run, Maintain (runtime & offline)) with the orchestrator focusing on Deploy, Run, and Maintain (runtime).
        * A table (from 2019/20) compares features of orchestrators like Kubernetes, Cloudify, Swarm, and Marathon.

7.  **Kubernetes as an Orchestration Example (Slides 27-54)**
    * **Kubernetes (K8s) Overview (Slide 28):** A platform for managing containerized workloads and services.
        * **Manages:** Service discovery/load balancing, storage orchestration, automated rollouts/rollbacks (desired state management), automatic bin packing (resource optimization), self-healing (restarting/replacing faulty containers), and secret/configuration management.
    * **K8s Cluster Architecture (Slide 29 & 39 & 44):**
        * **Control Plane:** Runs on a dedicated server or VM. Manages the worker nodes and Pods in the cluster. Components include `kube-apiserver`, `etcd`, `kube-scheduler`, `kube-controller-manager`, and optionally `cloud-controller-manager`.
        * **Nodes:** VMs or physical servers that run containerized workloads (Pods). Each node contains `kubelet`, `kube-proxy`, and a container runtime.
    * **K8s Cluster Components (Slide 30):**
        * **Control Plane Components:**
            * `kube-apiserver`: Exposes the Kubernetes API; front-end for the control plane.
            * `etcd`: Consistent, highly-available key-value store for all cluster data.
            * `kube-scheduler`: Assigns newly created Pods to nodes.
            * `kube-controller-manager`: Runs various controller processes.
            * `cloud-controller-manager`: Links K8s to a specific cloud provider's API.
        * **Node Components:**
            * `kubelet`: Primary node agent; ensures containers in a Pod are running and healthy.
            * `kube-proxy`: Network proxy on each node; implements K8s Service concepts, maintains network rules.
            * `Container runtime`: Software for running containers (e.g., Docker, CRI-O).
    * **Key K8s Concepts (Slide 31):** Objects, Pods, Nodes.
    * **K8s Objects (Slides 32-34):**
        * Persistent entities representing the state of the cluster (e.g., running applications, available resources, policies).
        * A "record of intent": users define the desired state (`spec`), and K8s constantly works to make the current `status` match the `spec`.
        * Example: A `Deployment` object defines an application with a desired number of replicas. K8s ensures that many instances run, replacing failed ones.
    * **Pods (Slide 35):**
        * Smallest deployable units in K8s.
        * Model an application-specific "logical host," containing one or more tightly coupled application containers.
        * Containers in a Pod share storage resources (Volumes), network resources (IP address, port space), and a specification for how to run.
        * Always co-located and co-scheduled, running in a shared context (Linux namespaces, cgroups).
    * **K8s Nodes (Slide 36):**
        * Worker machines (VMs or physical) managed by the control plane.
        * Run Pods.
        * Become cluster members via self-registration by `kubelet` or manual addition.
        * The control plane validates new Node objects before scheduling Pods on them.
    * **K8s Node Status (Slides 37-38):** Described by:
        * **Address:** Hostname, external/internal IP.
        * **Condition:** `Ready` (healthy, accepting Pods), `DiskPressure`, `MemoryPressure`, `PIDPressure`, `NetworkUnavailable`. If a node is not ready, Pods assigned to it may be evicted.
        * **Capacity:** Total resources available on the node (CPU, memory, max Pods).
        * **Allocable:** Resources available for normal Pods.
        * **Info:** Kernel version, K8s versions, container runtime details, OS.
    * **K8s Control Plane Components - Detailed (Slides 40, 43):**
        * `kube-apiserver`: Horizontally scalable front-end.
        * `etcd`: Backing store for all cluster data.
        * `kube-scheduler`: Watches for unassigned Pods and selects a node based on resource requirements, constraints, affinity/anti-affinity, data locality, interference, and deadlines.
        * `kube-controller-manager`: Runs controller processes like:
            * Node controller (handles node failures).
            * Job controller (manages one-off tasks via Pods).
            * EndpointSlice controller (links Services to Pods).
            * ServiceAccount controller.
        * `cloud-controller-manager`: Integrates with specific cloud provider APIs.
    * **Node Affinity/Anti-Affinity (Slides 41-42):**
        * **Node Affinity:** Rules to constrain which nodes a Pod can be scheduled on, based on node labels. Useful for data sovereignty, network latency optimization, HPC resource allocation, specific storage needs, multi-tenancy.
        * **Node Anti-Affinity:** Rules to prevent co-locating Pods on the same node(s). Useful for high availability (spreading risk) and separating workloads for performance/security.
    * **K8s Node Components - Detailed (Slide 45):**
        * `kubelet`: Agent on each node, manages Pods based on `PodSpecs` received from the API server.
        * `kube-proxy`: Manages network rules on nodes for communication to/from Pods.
        * `container runtime`: Runs the containers.
    * **K8s Controllers (Slide 47):**
        * Control loops that watch the cluster state and make changes to align current state with desired state (defined in object `spec` fields).
        * Track at least one K8s resource type.
        * Send messages to the API server to effect changes.
        * This is directly analogous to the Autonomic Manager's MAPE loop.
    * **Job Controller Example (Slide 48):** When a `Job` object (for a one-off task) is created, the Job controller ensures the right number of Pods are run to complete the task. It tells the API server to create/remove Pods, and updates the `Job` object's status to `Finished` upon completion.
    * **Control of External Resources Example (Slide 49):** The Cluster Autoscaler is a controller that horizontally scales the *nodes* in a cluster by interacting with the cloud provider's API.
    * **Scheduler (Slide 50):**
        * Ensures Pods are matched to Nodes for `kubelet` to run them.
        * `kube-scheduler` is the default, responsible for finding the best Node for each unassigned Pod.
    * **Kube-scheduler Process (Slides 51-53):**
        * For each new/unscheduled Pod, it selects an optimal node.
        * **Filtering:** Finds feasible Nodes that meet the Pod's scheduling requirements (e.g., `PodFitsResources` filter checks for sufficient CPU/memory). If no suitable node, the Pod remains unscheduled.
        * **Scoring:** Assigns a score to each feasible Node based on active scoring rules.
        * **Binding:** Assigns the Pod to the Node with the highest score (randomly selects among ties) and notifies the API server.
        * Considers factors like resource needs, constraints, affinity, data locality, interference.
    * **Configuration of Filtering and Scoring (Slide 54):**
        * **Scheduling Policies (deprecated since v1.23):** Configured Predicates (for filtering) and Priorities (for scoring).
        * **Scheduling Profiles:** Current method; configures Plugins for different scheduling stages (QueueSort, Filter, Score, Bind, etc.) to customize `kube-scheduler`.

8.  **Outcome (Slide 55)**
    * The presentation aimed to cover:
        * General concepts of Automation and Orchestration.
        * The difference between Autonomic Computing and Automation.
        * The difference between Automation and Orchestration.
        * An example of an automation tool (Ansible).
        * An example of an orchestrator (Kubernetes).