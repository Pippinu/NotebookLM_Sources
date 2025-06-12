## DCN Addressing and Routing Basics

Once the physical components are in place, the core networking challenge is deciding how to address the thousands of servers and route traffic between them efficiently. This choice has major implications for scalability, flexibility, and support for critical data center features like live Virtual Machine (VM) migration.

### The Core Dilemma: IP vs. Ethernet

Data center networks can be designed using two primary packet-switching technologies, each with its own set of trade-offs.

#### IP Networking (Layer 3)
* **Addressing:** Uses **hierarchical IP addressing**. An IP address is tied to its topological location in the network.
* **Pros:** Highly scalable. The hierarchical structure allows routers to aggregate routes, keeping forwarding tables small and manageable even in very large networks. Standard routing protocols like OSPF can efficiently find the best paths.
* **Cons:** Poor support for VM migration. If a VM moves from a server in one rack to another, its IP address must change to reflect its new location. This breaks active TCP connections and adds significant management overhead.

#### Ethernet Networking (Layer 2)
* **Addressing:** Uses **flat MAC addressing**. A device's MAC address is permanent and not tied to its location.
* **Pros:** Excellent support for VM migration. A VM can move anywhere within the same Layer 2 domain and keep its IP address, as the address is not tied to the physical location. It is also "plug-and-play," requiring no address configuration on servers.
* **Cons:** Not scalable or efficient for large networks.
    * **Flooding:** Unknown destinations require flooding frames to all ports, creating unnecessary traffic.
    * **Spanning Tree Protocol (STP):** To prevent routing loops in topologies with redundant paths, STP disables links. This is extremely wasteful, as it prevents the network from using all its available capacity.

### Hybrid Solutions and Tunneling

Since neither pure IP nor pure Ethernet is ideal, modern data centers use a hybrid approach that combines the strengths of both. The key technology that enables this is **tunneling** (or encapsulation).

The basic idea is to create a virtual Layer 2 network that spans across a physical Layer 3 network. A packet from a VM is wrapped inside another packet for transport across the core network.

* **Inner Packet:** Contains the original source and destination MAC/IP addresses used by the VMs.
* **Outer Packet:** Contains source and destination addresses that are routable across the physical data center network.

This technique abstracts the VM's location from the physical network, allowing a VM to move freely while still benefiting from the scalable and efficient routing of an IP-based core network. Standards like **VXLAN** (Virtual Extensible LAN) and **TRILL** (Transparent Interconnection of Lots of Links) are built on this principle.

### Case Study: VL2 Addressing

VL2 is a well-known architecture that demonstrates this hybrid approach. It uses two types of IP addresses to separate a service's identity from its physical location:

* **Application-specific Addresses (AAs):** These are permanent, portable addresses assigned to VMs. They function as the stable endpoint identifiers and do not change when a VM migrates.
* **Location-specific Addresses (LAs):** These are standard IP addresses assigned to the physical network hardware, such as the ToR switches. They are used for the actual routing of packets through the data center.

The system works as follows:
1.  A **Directory System**, a centralized mapping service, keeps track of which LA corresponds to each AA.
2.  When a source VM sends a packet, a **VL2 agent** (a shim layer on the server) intercepts it.
3.  The agent queries the directory system to find the LA of the ToR switch connected to the destination VM.
4.  It then **encapsulates** the original packet (using AAs) inside a new IP packet with the source and destination LAs.
5.  The physical network routes the packet using the LAs.
6.  When the packet arrives at the destination ToR switch, it is decapsulated, and the original packet is forwarded to the correct VM using its AA.

### Leveraging Multiple Paths with ECMP

Data center topologies are intentionally designed with rich connectivity, meaning there are often many different paths of the same cost (length) between any two servers. To take advantage of this, data centers use **Equal Cost Multi-Path (ECMP)** routing.

ECMP prevents the network from sending all traffic down a single path while others sit idle. Instead, it distributes traffic across all available equal-cost paths.

* **Mechanism:** To prevent reordering packets within a single TCP connection, ECMP does not distribute packets on a round-robin basis. Instead, it uses a **hash function**. It calculates a hash based on fields in the packet header (like source/destination IPs and ports) to create a flow identifier. All packets belonging to the same flow will produce the same hash result and will therefore be sent down the same path, preserving in-order delivery.

### Latency in Data Centers

In the context of a data center, latency is composed of three main factors:
* **Propagation Delay:** The time it takes a signal to travel across the physical medium (copper or fiber). Given the relatively short distances inside a data center (a few hundred meters at most), this is essentially zero (less than a few microseconds).
* **Switching Latency:** The time a switch takes to process a packet and forward it. This is a small but significant component, typically a few microseconds per switch.
* **Queuing Latency:** The time a packet spends waiting in a buffer inside a switch because of congestion. This is the **most significant and variable** component of latency in a data center.

Understanding and managing queuing latency is the primary goal of congestion control protocols, which we will cover later.