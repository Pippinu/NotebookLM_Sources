### The "Big Picture": A Policy Dilemma

The policies discussed so far present a clear trade-off:

  * **Push Algorithms (JSQ, PoD):** These are **delay-optimal in the heavy-traffic regime**, providing excellent performance when the system is under stress. However, they suffer from non-zero dispatching delays and can have high messaging overhead.
  * **Pull Algorithms (JIQ):** These have **zero dispatching delay** and very low message overhead. However, their performance is poor in the heavy-traffic regime, as they devolve into a simple RANDOM policy.

The goal, therefore, is to design a policy that combines the best properties of both approaches: the low overhead of "pull" policies with the high-performance guarantees of "push" policies.

### A Hybrid Approach: The Join-Below-Threshold (JBT) Algorithm

The Join-Below-Threshold (JBT) algorithm is a hybrid "pull-based" scheme designed to achieve this goal. It operates with two key components: the dispatcher and the server.

#### JBT($\large r$) Algorithm: Dispatcher's Role

The dispatcher's logic is as follows:

1.  It maintains a list of server IDs that are known to have a queue length less than a predefined threshold, $\large r$.
2.  When a new task arrives, the dispatcher checks its list of available servers.
      * If the list is **not empty**, it picks a server ID from the list at random, sends the task to that server, and **immediately removes that ID from the list**. This is an optimistic measure, assuming that sending one task might push the server's queue over the threshold.
      * If the list is **empty**, it defaults to a simple policy like RANDOM.
3.  The dispatcher's list is only repopulated when it receives "I'm available" messages from the servers themselves.

#### JBT($\large r$) Algorithm: Server's Role

The server's role is very simple and communication-efficient:

1.  The server continuously monitors its own task queue.
2.  A server sends a message to the dispatcher **only** at the specific moment its queue length drops from being equal to the threshold $\large r$ to $\large r-1$. This single message announces that it is now "below threshold" and available to be added back to the dispatcher's list.

### Practical Implementation: The Adaptive JBT-d Algorithm

A fixed threshold $\large r$ is not optimal, as the ideal queue length threshold changes with the overall system load. The **JBT-d** algorithm is a practical, **adaptive** version that dynamically adjusts this threshold.

1.  **Adaptive Threshold Update:** The threshold is no longer a fixed value. Periodically (e.g., every $\large T$ time units), the dispatcher polls a small, random sample of $\large d$ servers. It then sets the **new threshold** to be the **minimum queue length** it observed among that sample. This new threshold is then communicated to all servers.
2.  **Server Behavior:** Each server operates as in the basic JBT policy, sending its ID to the dispatcher whenever its queue length falls below the *current, dynamically updated* threshold.
3.  **Dispatcher Behavior:** Upon a new task arrival, the dispatcher sends it to a server from its list of available (below-threshold) servers. If the list is empty, it dispatches the task randomly.