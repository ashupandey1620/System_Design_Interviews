### Summary of Multi-Conference Application System Design

This video provides an in-depth explanation of the **system design of multi-conference video applications**, focusing on how these applications work, the architectural patterns involved, and the challenges of scaling multi-party video calls. Key terms explained include **WebRTC**, **Peer-to-Peer (P2P)** connections, **Mesh networks**, **MCU (Multipoint Control Unit)**, and **SFU (Selective Forwarding Unit)**.

---

### Core Concepts and Architectural Patterns

#### 1. Multi-Conference Application Overview
- Multi-conference apps allow **multiple users to engage in video calls simultaneously**.
- Examples include popular platforms such as **Zoom** and **Google Meet**.
- Multi-conference implies more than two participants communicating together.

#### 2. Peer-to-Peer (P2P) Communication
- **Peer**: Any participant in the video call.
- When only **two peers are involved**, **WebRTC** facilitates direct peer-to-peer communication.
- P2P connections use the **UDP protocol** for faster, direct data transmission without a server.
- Each peer sends and receives **audio and video streams directly** to/from the other peer.
- **Advantages**:
  - No server costs.
  - Low latency due to direct connection.
- **Disadvantages**:
  - Limited to two participants.
  - Not scalable for more peers.
  
#### 3. Mesh Peer-to-Peer Network
- When more than two peers join, a **mesh network** is formed where each peer connects to every other peer.
- For $n$ peers, each peer creates $(n-1)$ P2P connections.
- This approach quickly becomes **complex and unscalable**:
  - High CPU and bandwidth usage on each peer.
  - Frequent reconnections required as peers join/leave.
  - Not fault tolerant; prone to crashes with many peers.

---

### Server-Based Architectures

#### 4. Forwarding Unit / Server Introduction
- To overcome mesh network limitations, a **central server (forwarding unit)** is introduced.
- Each peer establishes only **one P2P connection with the server**.
- Peers send their individual streams to the server.
- The server processes these streams and redistributes them.

#### 5. Multipoint Control Unit (MCU)
- **MCU** combines all incoming streams into a **single mixed stream**.
- The server mixes audio and video into one stream and sends it back to all peers.
- **Advantages**:
  - Simplifies client processing: only one stream to decode.
- **Disadvantages**:
  - Very **CPU-intensive** to mix streams in real-time.
  - Introduces **latency (lag)**.
  - Loss of stream control for individual clients (e.g., cannot mute a specific user’s video).
  - Server bandwidth can become a bottleneck.

| Feature            | Peer-to-Peer (Mesh) | MCU                         |
|--------------------|---------------------|-----------------------------|
| Number of Connections per Peer | $(n-1)$           | 1                           |
| Server Involvement | None                | High (mixing streams)       |
| CPU Consumption    | Low (on peers)      | High (on server)            |
| Scalability        | Poor                | Moderate                    |
| Stream Control     | Yes (on each peer)  | No (combined stream)        |
| Latency            | Low                 | High                        |

#### 6. Selective Forwarding Unit (SFU)
- **SFU** is a more efficient architecture widely used in production (e.g., Google Meet, Zoom).
- Each peer sends its raw stream to the server.
- The SFU **does not mix streams** but **forwards individual streams selectively** to other peers.
- Peers receive multiple separate streams and decide locally how to render or mute them.
- **Advantages**:
  - Lower CPU load on the server compared to MCU.
  - Greater client-side control (pinning, muting individual streams).
  - Better scalability for large conferences.
- **SFU** essentially acts as a **“pass-through tunnel”** forwarding streams without processing.

| Feature            | MCU                         | SFU                         |
|--------------------|-----------------------------|-----------------------------|
| Stream Combination | Combines into one stream    | Forwards separate streams   |
| CPU Usage          | High                        | Low                         |
| Client Control     | Limited (combined stream)   | Full (individual streams)   |
| Scalability        | Moderate                    | High                        |
| Latency            | Higher                      | Lower                       |

---

### Key Technical Details

- **WebRTC** supports **only peer-to-peer connections** natively.
- For multi-party calls, either mesh P2P or server-based forwarding is required.
- **Mesh P2P** involves many connections, leading to poor scalability.
- **MCU** mixes streams into a single stream but is CPU-intensive and adds lag.
- **SFU** forwards streams selectively without mixing, improving scalability and user experience.
- The SFU approach allows clients to **choose which streams to consume or ignore**, enabling advanced UI features like selective muting or spotlighting a participant.
- **Mediap** is an open-source SFU library mentioned for implementing SFU servers.
- Implementing these architectures requires understanding routing, transport, and worker processes within SFU.

---

### Summary Table of Architectures and Their Characteristics

| Architecture Type      | Description                                        | Scalability        | Server Load    | Client Complexity | Latency       | Stream Control        |
|-----------------------|--------------------------------------------------|--------------------|----------------|-------------------|---------------|-----------------------|
| Peer-to-Peer (P2P)    | Direct connection between two participants       | Very low (2 peers) | None           | Low               | Low           | Full                  |
| Mesh P2P              | Each peer connects to every other peer           | Low                | None           | High              | Low           | Full                  |
| MCU                   | Server mixes all streams into one combined stream| Moderate           | High           | Low               | High          | Limited (combined)    |
| SFU                   | Server forwards individual streams selectively   | High               | Moderate       | Moderate          | Low to medium | Full (individual)     |

---

### Key Insights

- **WebRTC is optimized for peer-to-peer connections** but not scalable for multi-party calls alone.
- **Mesh networks** for multi-party calls suffer from exponential connection growth, making them impractical beyond a few peers.
- **MCU architecture simplifies client requirements but is expensive and inefficient on server resources**, resulting in latency and limited user control.
- **SFU architecture is the industry standard for scalable multi-party video conferencing**, balancing server load and client flexibility.
- The **SFU allows clients to decide how to render and consume streams**, enabling features like selective muting and spotlighting.
- Open-source libraries like **Mediap** provide frameworks to build SFU servers but require advanced knowledge to implement.

---

### Conclusion

The video systematically explains the evolution of multi-conference video application designs from **peer-to-peer** to **mesh**, then to **server-based architectures like MCU and SFU**. It highlights **why SFU is the most balanced and scalable approach** in modern real-time communication applications. Understanding these architectures is crucial for designing efficient, scalable multi-party video conferencing systems.

