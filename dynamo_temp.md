Dynamo is a highly available and scalable distributed key-value storage system developed and used by Amazon.com for its e-commerce platform. It's designed for services requiring an "always-on" experience, achieving high availability by, in some cases, sacrificing strong consistency.

**System Assumptions and Requirements (Section 2.1):**
* **Query Model:** Simple read/write operations for binary objects (blobs, usually <1MB) identified by unique keys; no relational schema or multi-item operations. This because Dynamo target application that need to store objects that are relatively small and require simple read/write op.
* **ACID Properties:** Prioritizes availability over strong consistency (the "C" in ACID). Provides no isolation guarantees and only single-key updates.
* **Efficiency:** Must run on commodity hardware (standard, less expensive HW), meeting stringent SLAs (often 99.9th percentile latency and throughput), because critical for service operations. Allows services to tunes their specific performance/cost/availability/durability needs, with consequent tradeoffs.
    * *Example*: Higher availabilty and durability (through data replication) translate in higher cost or slower write performance.

**Service Level Agreements (SLA) (Section 2.2):**
SLAs are negotiated contracts between client and service on expected request rates for a particular API and service latencies under those conditions. Amazon's SOA relies on each service involved meeting its SLA. Amazon measures SLAs at the 99.9th percentile to ensure a good experience for most customers, rather than using averages or medians. Dynamo's design allows services to tune system properties and let service make their own tradeoffs to meet these demanding SLAs.

**Design Considerations (Section 2.3):**
* **Consistency vs. Availability:** Strong consistency in traditional approaches, often reduces availability during failures. Dynamo uses ***optimistic replication*** (more below) for higher availability, where changes propagate in the background, tolerating temporary inconsistency that have to be solved later. This can lead to need of conflict resoultion.
* **Eventual Consistency:** Dynamo is designed as an eventually consistent datastore, that is all updates reach all replicas eventually.
* **Conflict Resolution:**
    * **When:** To achieve an "always writeable" store (critical for services like shopping carts that should never reject updates), Dynamo pushes conflict resolution complexity to reads, rather than rejecting writes.
    * **Who:** Resolution can be by the datastore (simpler approached like "last write wins") or by the application (which understands the data schema and can merge versions). Dynamo allows the application to choose, using better approaches, otherwise, data store do the job by applying simple approaches.
* **Other Principles:** Incremental scalability (add nodes one at a time), symmetry (all nodes have same responsibilities), decentralization (peer-to-peer over centralized control), and heterogeneity (distribute work based on server capabilities).

* **Optimistic vs Pessimistic Replication**: Optimistic replication prioritizes availability and "always writeable" characteristics, **accepting that inconsistencies might temporarily arise and will need to be resolved**. Pessimistic replication prioritizes strong consistency, potentially at the cost of availability or write latency if replicas are not reachable.

**Related Work (Section 3)**
Dynamo is compared to other decentralized storage systems, P2P systems (like Freenet, Gnutella, Pastry, Chord, OceanStore, PAST), distributed file systems (like Ficus, Coda, GFS, Farsite), and databases (like Bayou, Bigtable). 

Key differentiators for Dynamo are its primary goals: 1. Being "always writeable" for high availability where no updates are rejected.
2. Its operation within a single administrative domain, where each node is assumed trusted.
3. Its focus on simple key-value access without hierarchical namespaces or complex relational schemas, 4. Its design for low-latency (99.9th percentile) operations, achieved partly by being a **zero-hop DHT**, each node mantains enough info. locally to route a request to the appropriate node directly to avoid multi-hop routing.

**System Architecture (Section 4)**

![alt text](./images/dynamo_core_architecture.png)

Dynamo employs several core distributed systems techniques:
* **System Interface (Section 4.1):** Exposes `get(key)` and `put(key, context, object)` operations.
    * `get(key)`: Locates replicas, returns a single object or a list of conflicting versions with a `context`.
    * `put(key, context, object)`: Writes replicas; the `context` is opaque metadata (including version info from a previous read). Keys are hashed (MD5) to 128-bit identifiers to **determine responsible storage nodes**.
* **Partitioning Algorithm (Section 4.2):** Uses **consistent hashing** to distribute keys across nodes on a logical "ring". Each node is assigned random positions (tokens) on the ring. To address non-uniform load distribution and heterogeneity, Dynamo uses **virtual nodes**: each physical node is responsible for multiple virtual nodes/tokens on the ring. This evens load distribution during node additions/removals and allows capacity-based token assignment.
* **Replication (Section 4.3):** Data is replicated on N hosts (N is configurable) for high availability and durability[cite: 217, 218]. A key's coordinator node stores the data locally and replicates it to N-1 clockwise successor nodes on the ring[cite: 220, 221]. The list of N nodes responsible for a key is its **preference list**, constructed to ensure distinct physical nodes, even with virtual nodes[cite: 224, 225].
* **Data Versioning (Section 4.4):** To achieve eventual consistency and handle concurrent updates, Dynamo treats each modification as a new, immutable version of data, allowing multiple versions to coexist[cite: 234, 235].
    * **Vector Clocks:** `(node, counter)` pairs are associated with each object version to capture causality[cite: 244, 245]. They help determine if versions are ancestors, descendants, or in conflict (parallel branches)[cite: 247, 248, 249]. When updating, clients pass the `context` (containing the vector clock) from a prior read[cite: 250, 251].
    * **Conflict Resolution:** If a read encounters causally unrelated versions, all are returned to the client for **semantic reconciliation** (e.g., merging shopping carts)[cite: 238, 254, 255]. The reconciled version then supersedes the divergent ones[cite: 255]. Vector clock sizes are managed by truncating the oldest (node, counter) pair if a threshold is met, storing a timestamp with each pair to identify the oldest[cite: 277, 278].
* **Execution of `get()` and `put()` (Section 4.5):**
    * Any node can act as a coordinator for a request[cite: 281]. Requests can be routed via a load balancer or a partition-aware client library (for lower latency)[cite: 284, 285]. If a non-coordinator node receives a request, it forwards it to a node in the key's preference list[cite: 289, 290].
    * Operations involve the first N healthy nodes in the preference list[cite: 291].
    * A **sloppy quorum** system is used with configurable R (minimum read replicas) and W (minimum write replicas)[cite: 294, 295, 296]. For consistency, often R+W > N[cite: 297].
    * `put()`: Coordinator generates a new vector clock, writes locally, sends to N highest-ranked reachable nodes. Success if at least W-1 nodes respond[cite: 300, 302, 303].
    * `get()`: Coordinator requests versions from N highest-ranked reachable nodes, waits for R responses, returns result (possibly with multiple versions for client reconciliation)[cite: 304, 305].
* **Handling Failures: Hinted Handoff (Section 4.6):**
    * If a target node in the preference list is down/unreachable, the replica is sent to a different healthy node (not necessarily in the top N) with a "hint" indicating the intended recipient[cite: 309, 311, 312].
    * The hinted node stores this replica temporarily and attempts to deliver it to the original target node once it recovers[cite: 313, 314]. This ensures R/W operations don't fail due to temporary issues[cite: 315]. Setting W=1 maximizes write availability[cite: 316].
    * Replicas are spread across multiple data centers to handle DC failures[cite: 321, 322].
* **Handling Permanent Failures: Replica Synchronization (Anti-entropy) (Section 4.7):**
    * An anti-entropy protocol using **Merkle trees** synchronizes replicas and handles permanent failures or lost hinted replicas[cite: 326, 327].
    * A Merkle tree is a hash tree where leaves are hashes of key values[cite: 328]. Nodes exchange Merkle tree roots for common key ranges; if roots differ, they traverse down the trees to find and synchronize inconsistent keys, minimizing data transfer[cite: 330, 331, 334, 335, 338, 339]. Each node maintains a tree per key range it hosts[cite: 336].
* **Membership and Failure Detection (Section 4.8):**
    * **Ring Membership:** Node addition/removal is an explicit administrative operation issued to a Dynamo node[cite: 345, 346]. Changes are persisted and propagated via a **gossip-based protocol** for eventual consistency[cite: 347, 348, 349, 350]. Node-to-token mappings are also gossiped[cite: 353, 354].
    * **External Discovery (Seeds):** Some nodes are designated as "seeds" (discoverable externally) to prevent logical ring partitions during membership changes[cite: 359, 360, 361, 362].
    * **Failure Detection:** A local notion is used. If node A fails to communicate with node B, A considers B failed and uses alternate nodes for requests mapped to B's partitions, retrying B periodically[cite: 365, 366, 367, 368]. Explicit join/leave methods reduce the need for a globally consistent failure view[cite: 373, 374].
* **Adding/Removing Storage Nodes (Section 4.9):** When a new node is added, it gets assigned random tokens. Existing nodes responsible for the key ranges now covered by the new node transfer the relevant data to it[cite: 375, 376, 377]. This distributes bootstrapping load.

**Implementation (Section 5)**
Each Dynamo node has three Java components: request coordination, membership/failure detection, and a pluggable local persistence engine (e.g., Berkeley DB Transactional Data Store, MySQL) chosen based on application needs (object size, access patterns)[cite: 379, 380, 381, 383]. Most instances use BDB[cite: 384]. Request coordination uses an event-driven (SEDA-like) model with Java NIO, where each client request creates a state machine on the coordinator node[cite: 385, 386, 387, 388, 389]. **Read repair** is performed opportunistically: if a read coordinator receives stale versions from some replicas, it updates them with the latest version after responding to the client[cite: 394, 395]. Any of the top N nodes in a preference list can coordinate a write to distribute load and improve read-your-writes consistency by choosing the node that replied fastest to a previous read[cite: 399, 400, 401].

**Experiences & Lessons Learned (Section 6)**
Dynamo instances are configured with specific N (number of replicas, typically 3), R (read quorum), and W (write quorum) values, often (3,2,2), to meet application SLAs for performance, availability, and durability[cite: 403, 404, 420, 421, 422, 430].
* **Usage Patterns:** Business logic specific reconciliation (client merges versions, e.g., shopping cart [cite: 406, 407, 408]), timestamp-based reconciliation ("last write wins," e.g., session data [cite: 411, 412]), and as a high-performance read engine (R=1, W=N, e.g., product catalog cache [cite: 415, 416, 417, 418]).
* **Performance vs. Durability:** Dynamo targets 99.9% R/W within 300ms[cite: 437]. Write latencies are higher than reads[cite: 442]. An optimization using an in-memory write buffer can significantly lower 99.9th percentile latencies but trades some durability (server crash can lose buffered writes)[cite: 447, 448, 449, 451, 453]. This risk is mitigated by having one replica perform a "durable write" while the coordinator only waits for W responses[cite: 454, 455].
* **Load Distribution:** Consistent hashing aims for uniform load. The paper discusses three partitioning strategies. Strategy 3 (Q/S tokens per node, equal-sized partitions) achieved the best load balancing efficiency and reduced metadata size compared to the initial strategy (T random tokens per node, partition by token value) because it decouples partitioning from placement, simplifies bootstrapping/recovery, and eases archival[cite: 456, 472, 473, 492, 498, 499, 512, 513, 515, 516, 517, 518].
* **Divergent Versions:** Occur rarely, mostly due to many concurrent writers (e.g., busy robots) rather than system failures[cite: 523, 525, 530, 532, 533].
* **Coordination:** Client-driven coordination (client library manages R/W state machine) significantly reduces latency compared to server-driven (via load balancer) by eliminating an extra network hop[cite: 541, 542, 545, 549, 556, 557, 558].
* **Background vs. Foreground Tasks:** An admission controller manages resource slices for background tasks (replica sync, data handoff) based on monitored foreground operation performance to prevent contention[cite: 563, 564, 565, 566, 567, 568, 569, 570, 571, 572, 573].
* **Overall:** Dynamo provided high availability (99.9995% successful responses)[cite: 576, 577]. Exposing consistency and reconciliation logic to developers was manageable as many Amazon apps were already designed for inconsistencies[cite: 578, 580, 581]. The full membership model (gossiping full routing tables) scales to hundreds of nodes; larger scales might need hierarchical extensions[cite: 583, 585, 586, 587].

**Conclusions (Section 7)**
Dynamo successfully combines decentralized techniques to provide a highly available and scalable key-value store, demonstrating that an eventually-consistent system can be a robust building block for demanding applications[cite: 589, 590, 591, 592, 593]. It allows service owners to tune parameters (N,R,W) to meet specific SLAs[cite: 592].