### Summary of System Design Patterns Video

This video provides an insightful overview of several **key system design patterns** crucial for building scalable, maintainable, and efficient software architectures. The focus is on understanding the **problem statements**, **definitions**, **trade-offs**, and practical considerations of each pattern, especially in the context of **microservices** and **high-throughput systems**.

---

### Key System Design Patterns Discussed

#### 1. Microservices Architecture vs. Monolith
- **Microservices**:  
  - Each feature is an independent service (e.g., Authentication, Notification, Post, Comment service).  
  - Benefits include independent development, deployment, and scaling of services.  
  - Challenges: managing multiple servers, inter-service communication, and complexity.  
  - Failure isolation: if one service (e.g., Notification) fails, others (Authentication, Comments) continue working.

- **Monolithic Architecture**:  
  - Single large server with all features in one codebase.  
  - Easier management but limited scalability (scale entire server, not individual services).  
  - Single point of failure: bug in one module affects the entire server.

**Trade-offs**:  
| Aspect                 | Microservices                      | Monolith                         |
|------------------------|----------------------------------|---------------------------------|
| Scalability            | Service-level independent scaling| Scale entire server             |
| Fault Isolation        | Isolated service failures         | Single failure affects whole app|
| Complexity             | High (multiple servers, comms)    | Lower (single codebase)          |
| Deployment Flexibility | High (different tech stacks)      | Low                             |

---

#### 2. Database per Service Architecture
- Each microservice maintains its own **database** rather than sharing a common one.
- Example:  
  - Authentication service uses MongoDB.  
  - Notification service uses ClickHouse (to track analytics like open rate).  
  - Post service uses PostgreSQL.

- **Advantages**:  
  - Service autonomy in choosing optimal database technology.  
  - Independent scaling and indexing.  
  - Avoids database-level contention.

- **Challenges**:  
  - No direct **database joins** across services; joins must be done at the application level.  
  - Increased operational cost due to multiple databases.  
  - Risk of inconsistent data if services update data independently.

---

#### 3. Circuit Breaker Pattern
- Addresses **cascading failures** in microservices where one service's downtime can cause dependent services to fail.
- Acts as a **proxy layer** between services managing communication.
- **States of Circuit Breaker**:  
  - **Closed**: Normal operation, requests pass through.  
  - **Open**: Requests blocked, no data flow to failing service.  
  - **Half-Open**: Trial state where limited requests are sent to test if the service recovered.

- **Functionality**:  
  - Detects failures and prevents repeated failed requests.  
  - Improves system resilience by stopping cascading failures and allowing graceful recovery.  
  - Retries after intervals to check if the service is back online.

- Supported by industry best practices (e.g., Microsoft’s implementation).

---

#### 4. Event Sourcing Pattern
- Used in **high throughput systems** like banking or e-commerce.
- Instead of storing mutable state, maintain an **immutable event log** capturing every state change, e.g., order status changes or transaction entries.
- Benefits include:  
  - Simplified consistency and audit trail.  
  - Avoids locking and contention in databases.  
  - Enables reconstructing current state from event replay.

- Example:  
  - Order status progresses through events: Placed → Ready to Ship → Shipping → Shipped → Delivered.  
  - Bank account balance tracked by summing transaction events rather than storing current balance directly.

---

#### 5. CQRS (Command Query Responsibility Segregation)
- Splits system into two parts:  
  - **Command side** for writes/mutations (create, update, delete).  
  - **Query side** for reads.

- Commands generate **events** that update the read database asynchronously, enabling optimized read performance.
- Works well with event sourcing but introduces **eventual consistency** (read data may lag behind writes).
- Useful in large-scale systems (e.g., Amazon) to avoid bottlenecks.

---

### Overview Table of Patterns and Core Concepts

| Pattern                      | Purpose / Problem Solved                                      | Key Benefits                         | Trade-offs / Considerations                     |
|------------------------------|--------------------------------------------------------------|------------------------------------|------------------------------------------------|
| Microservices vs Monolith     | Modular vs single codebase architecture                       | Scalability, fault isolation       | Complexity, inter-service communication         |
| Database per Service          | Independent DB per microservice for autonomy                  | Flexibility, optimized storage     | No cross-service joins, higher cost             |
| Circuit Breaker              | Prevent cascading failures due to dependent service downtime | System resilience, fault tolerance | Requires proxy layer, state management          |
| Event Sourcing               | Immutable event log for state changes in high throughput     | Auditability, consistency          | Requires event replay, complexity in queries    |
| CQRS                        | Separate read/write models to optimize performance            | Scalability, optimized queries     | Eventual consistency, complexity in sync logic  |

---

### Additional Highlights
- **Microservice communication** and management are complex but essential for scalable modern systems.
- Database per service architecture offers flexibility but complicates data joins and consistency.
- Circuit breaker pattern is critical to avoid cascading failures in distributed systems.
- Event sourcing transforms mutable state management by appending events, enhancing consistency and traceability.
- CQRS complements event sourcing to separate concerns for reads and writes, improving system scalability.

---

### Conclusion
This video provides a **comprehensive foundation** on essential system design patterns for scalable software architecture, especially emphasizing **microservices**, their database strategies, fault tolerance via **circuit breakers**, and data consistency using **event sourcing** and **CQRS**. Understanding these patterns and their trade-offs is critical for designing robust distributed systems.

---

### Keywords
- Microservices  
- Monolithic Architecture  
- Database per Service  
- Circuit Breaker Pattern  
- Event Sourcing  
- CQRS (Command Query Responsibility Segregation)  
- High Throughput Systems  
- Fault Tolerance  
- Scalability  
- Eventual Consistency