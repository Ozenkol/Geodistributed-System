# Report: Geo-Distributed Systems

## 1. Problem Background
When building critical IT systems — for banking, healthcare, aviation — reliability becomes extremely important. A single failure can cause enormous losses. Local infrastructure, even with good redundancy, cannot fully protect against rare but catastrophic events like accidental cable damage by an excavator.

To reduce risks, engineers create geo-distributed systems: the same services operate simultaneously across multiple data centers, often in different cities and countries.

## 2. Requirements for Solving the Problem
The key requirements for such a system are:

- **High reliability** — the system must continue functioning even if one data center completely fails.
- **Operation under network failures** — inter-data-center networks are inherently unreliable.
- **Data consistency** — to avoid user errors, like getting two horses instead of one (example from a game store).
- **Acceptable response time** — users shouldn't have to wait 10 seconds for a reply.

These requirements lead to a necessary choice between:

- **Strict synchronization** (slow but accurate),
- **Asynchronous processing** (fast but potential inconsistencies).

## 3. Modeling the Situation
The situation can be described mathematically using the **CAP theorem**:

- **Consistency** — strict data accuracy,
- **Availability** — system responsiveness,
- **Partition tolerance** — resilience to network splits.

In geo-distributed systems, **Partition Tolerance** is mandatory, meaning we must choose between **Consistency** and **Availability**.

The **PACELC model** further clarifies that under normal conditions we also trade between **latency** and **consistency**.

Real-world business scenarios can be modeled:

- **Balance top-up** — strict consistency is crucial, speed is secondary.
- **Store purchase** — fast response is crucial, minor consistency delays are acceptable.

Thus, different data handling strategies are chosen based on the business context.

## 4. How the Model Influences System Architecture
From the model, the architectural structure follows:

- **Nodes** (servers) in different data centers must have identical functionality.
- **Nodes** should process local requests autonomously where risk allows.
- **Data replication** is essential, but replication mode (sync/async) depends on the data type.

Key functional and structural blocks:

- **Transaction processing services**,
- **Databases** with sharding or master-master replication,
- **Components for balance synchronization** between data centers.

The main unit of design becomes a service that manages its data portion, while databases synchronize according to agreed rules.

## 5. Engineering Approaches to Solution Preparation
Key engineering methods include:

- **Replication and quorum mechanisms** — for example, a request is successful after confirmation from two out of three data centers.
- **Local balancing** — small operations (like purchases) are handled instantly at the nearest location.
- **Negotiation between services** — for major operations (e.g., large purchases), cross-center coordination is performed.
- **Optimistic locks and asynchronous replication** — with later data reconciliation.
- **Load management techniques** — like sharding, for handling clients generating high traffic.

## 6. How Optimal Is Our Solution? Advantages
The solution is optimal under real-world conditions because:

- It **accounts for real user scenarios**.
- It **balances availability and consistency** depending on request type.
- It **remains functional despite network losses**.
- **Risk/performance trade-offs** are consciously managed.
- **Scalability** is achievable, allowing new data centers to be added without massive restructuring.

**Main advantage**: **flexibility** — the system adapts to various working scenarios instead of being rigidly fixed.

## 7. Brief Conclusion
As a student, my main takeaway is:

> **Architecture is not about technology — it's about understanding how business works.**

Systems cannot be built blindly or just by following online articles. An engineer must understand:

- What users truly need,
- Where speed can be sacrificed,
- Where data loss is unacceptable,
- What real risks and trade-offs are tolerable.

Only then can a geo-distributed system be built that truly works, not just looks good on a presentation.

**Personal conclusion**:
> **Only deep subject matter understanding and engineers taking full responsibility for architectural choices lead to truly successful systems. No one else will make the right choices for us.**
