### Summary: Back of the Envelope Estimates in System Design

This video focuses on the concept of **Back of the Envelope (BoE) Estimates**, a crucial but often overlooked topic in system design, especially at senior engineering and architecture levels. BoE calculations help estimate the required resources such as CPU, memory, storage, and servers before designing or deploying a system.

---

### Core Concepts and Overview

- **Back of the Envelope Estimates** refer to rough, approximate calculations done quickly to get a ballpark figure for system requirements.
- These estimates help avoid **random guesses** about resources (e.g., CPU cores, storage size) which can lead to either over-provisioning (wasting cost) or under-provisioning (system crashes).
- The method is inspired by research by Google scientist **Jeff Dean**, who emphasized this approach in system design and capacity planning.
- BoE estimates are not precise but rather **rounded approximations** that aid initial planning and resource allocation.

---

### Key Steps in Back of the Envelope Calculations

- **Rounding and Approximations:** Always round numbers to nearest 10s, 100s, or convenient units to simplify calculations. For example, instead of 86,400 seconds per day, approximate it as 100,000 seconds.
- **Know Basic Units and Conversions:**
  - $1$ byte = $8$ bits
  - $1$ KB = $10^3$ bytes
  - $1$ MB = $10^6$ bytes
  - $1$ GB = $10^9$ bytes
  - $1$ TB = $10^{12}$ bytes
  - $1$ PB = $10^{15}$ bytes
- **Latency Awareness:** Understand latency differences between memory levels and storage:
  - L1 Cache: $0.5$ nanoseconds
  - L2 Cache: $5$ nanoseconds
  - Main Memory (RAM): $100$ nanoseconds
  - Disk Access: $10$ milliseconds
- Memory is **faster but volatile**, while disk is **slower but persistent**.
- Compression algorithms should be **simple and fast** to reduce transmission time without excessive CPU cost.
- Data centers located in different regions cause **network latency**; e.g., communication between India and US servers has higher latency.

---

### Practical Example: Twitter System Estimation

| Parameter                          | Value / Assumption                | Explanation                                    |
|----------------------------------|---------------------------------|------------------------------------------------|
| Monthly Active Users (MAU)        | $300$ million                   | Total users interacting monthly                |
| Daily Active Users (DAU)          | $300 \times 0.5 = 150$ million | Estimated 50% of MAUs active daily              |
| Average tweets per user per day   | $2$ tweets                      | Average number of tweets posted daily           |
| Tweets per second (QPS)           | $\approx 3500$                  | Calculated from DAU and tweets per day          |
| Peak QPS                         | $3500 \times 2 = 7000$          | Peak load estimate (double normal QPS)          |
| Tweet ID size                    | $64$ bytes                      | Unique identifier size per tweet                 |
| Average tweet text size          | $140$ bytes                    | Average text content per tweet                    |
| Media percentage in tweets       | $10\%$                         | Portion of tweets containing media               |
| Average media size per tweet     | $1$ MB                         | Average media file size in tweets                 |
| Media storage per day            | $150M \times 2 \times 0.1 \times 1MB = 30$ TB | Media storage generated daily                   |
| Long-term storage requirement    | $30 \text{ TB} \times 365 \times 5 \approx 55$ PB | Storage needed for 5 years                        |

**Insights:**

- The system needs to support **3500 insert operations per second** normally, scaling up to **7000 QPS** at peak.
- Storage needs are huge due to media content, requiring around **55 petabytes** over 5 years.
- Assumptions must be **clearly documented**, including user activity percentages, tweet counts, and media proportions.
- Units should always be labeled to avoid confusion.

---

### Practical Tips for BoE Calculations

- **Start with bare minimum resources** during development to reduce cost and test system logic.
- Monitor key metrics such as **CPU utilization, memory usage, query rates**, and **network traffic** under small user loads (e.g., 8-10 users).
- Use these observations to **scale estimates** for larger user bases.
- BoE calculations improve with **experience** and real-world monitoring feedback.
- Always **write down assumptions** and **units** explicitly.
- BoE is a flexible tool, not a fixed formula; it depends heavily on the context and learned experience.

---

### Summary Table: BoE Calculation Principles

| Principle                      | Description                                                                                  |
|-------------------------------|----------------------------------------------------------------------------------------------|
| Approximate and Round Numbers  | Use rounded numbers for simplicity, e.g., seconds per day ~ 100,000                          |
| Understand Units               | Know conversions between bits, bytes, KB, MB, GB, TB, PB                                    |
| Latency Awareness             | Memory is faster than disk; prioritize in-memory operations where possible                   |
| Compression                   | Use simple, fast compression to reduce data transfer time                                   |
| Document Assumptions           | Clearly record all assumptions with units                                                   |
| Start Small and Scale         | Begin with minimal resources, monitor, and adjust estimates based on usage patterns         |
| Experience Improves Estimates | Real-world data helps refine future BoE calculations                                        |

---

### Conclusion

**Back of the Envelope Estimates** serve as a vital starting point for system design, helping engineers make **informed, though approximate, decisions** about resource requirements. This approach reduces guesswork, cut costs, and provides a practical framework to scale systems efficiently. Mastery of these estimates improves with experience and real-world validation, making it an indispensable skill for senior engineers and architects involved in system design and deployment.

---

### Keywords

- Back of the Envelope Estimates (BoE)
- System Design
- Resource Estimation
- CPU, Memory, Storage
- Latency
- Approximation
- Twitter QPS Example
- Data Storage Calculation
- Compression
- Scaling
- Assumptions Documentation