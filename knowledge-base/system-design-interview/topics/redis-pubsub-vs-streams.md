# Redis Pub/Sub vs. Redis Streams (vs. Lists)

In System Design, choosing the right messaging mechanism in Redis is crucial for defining the **durability**, **consistency**, and **scalability** of your system.

This guide details the differences between **Pub/Sub**, **Streams**, and **Lists**, focusing on when to use each one.

---

## 1. Redis Pub/Sub (Fire-and-Forget)

The **Publish/Subscribe** pattern is a message broadcasting mechanism where publishers don't send messages to specific recipients. Instead, messages are published to channels.

### Key Characteristics
*   **Fire-and-Forget:** Redis does not store the message. If no one is listening on the channel at the time of sending, the message is **lost forever**.
*   **Fan-out:** A message sent to a channel is immediately delivered to **all** subscribers of that channel.
*   **Low Latency:** Extremely fast, as there is no persistence or acknowledgment overhead.
*   **No History:** A new subscriber does not receive past messages.

### Ideal Use Cases
*   **Real-Time Notifications:** "User X is typing...", "New like on your photo".
*   **Ephemeral Chat:** Where history is not critical or is loaded from another source.
*   **Service Discovery:** Notifying services that a new instance has come up.
*   **Live Updates (e.g., Collaborative Editor):** Showing the mouse cursor position of other users in real time. If a "mouse position" packet is lost, it's not critical, as the next packet will correct the position.

---

## 2. Redis Streams (Persistent Event Log)

Introduced in Redis 5.0, **Streams** is an append-only log data structure, inspired by Apache Kafka. It solves the lack of persistence in Pub/Sub.

### Key Characteristics
*   **Persistence:** Messages are stored on disk (if Redis is configured for it) and in memory. They remain there until explicitly deleted or expired (capping).
*   **Consumer Groups:** Allows multiple consumers to read from the same stream in a coordinated way. Redis manages which consumer read which message (offset).
*   **Acknowledge (ACK):** The consumer must confirm (`XACK`) that it processed the message. If the consumer fails before ACK, the message can be reprocessed by another (at-least-once delivery guarantee).
*   **History Access:** New consumers can read old messages from the beginning of the stream (`0`) or only new ones (`$`).

### Ideal Use Cases
*   **Event Sourcing:** Storing all state changes of an object (e.g., all edits in a document).
*   **Reliable Job Queues:** Where losing a task is unacceptable (e.g., payment processing, transactional email sending).
*   **Snapshot Workers:** Consolidating edit deltas into a SQL/NoSQL database. The worker can crash and come back, resuming from where it left off without losing data.

---

## 3. Redis Lists (Simple Queues)

Before Streams, Lists (`LPUSH` / `RPOP`) were used for queues. Still valid for simple cases.

### Characteristics
*   **Simple:** Easy to understand and implement.
*   **One Consumer per Message:** When doing `POP`, the message leaves the queue. Not ideal for fan-out (multiple services needing the same data).
*   **No Native Consumer Groups:** Requires manual implementation to handle failures and retries (e.g., `RPOPLPUSH` to move to a processing/dead-letter queue).

---

## Comparative Summary: The "Collaborative Editor" Scenario

Imagine you are designing a Google Docs. You need two things:
1.  Users to see letters appearing on each other's screens **now**.
2.  The document to be saved to the database safely.

### The Common Mistake (Using Pub/Sub for everything)
If you use Pub/Sub for the Worker to save to the database:
*   The user types "A". The server publishes to channel `doc-123`.
*   The Worker is restarting (deploy) and misses the message.
*   The user sees "A", but in the database "A" was never saved.
*   **Result:** Data loss.

### The Correct Architecture (Hybrid Approach)

| Component | Technology | Why? |
| :--- | :--- | :--- |
| **User-to-User Sync** | **Redis Pub/Sub** | We need maximum speed (Real-time). If a user loses a "cursor position" packet, it doesn't break the document. |
| **Persistence (Worker)** | **Redis Streams** | We need reliability. The Worker reads the Stream as a "Write-Ahead Log". If the Worker crashes, Redis keeps the offset and the Worker resumes later. Zero data loss. |

### Quick Decision Table

| Feature | Pub/Sub | Streams | Lists |
| :--- | :---: | :---: | :---: |
| **Persistence** | No | Yes | Yes |
| **History** | No | Yes | Yes |
| **Fan-out (1:N)** | Yes (Native) | Yes (Consumer Groups) | Difficult (Requires N lists) |
| **Delivery Guarantee** | At-most-once (May lose) | At-least-once (ACK) | At-most-once (Default) |
| **Complexity** | Low | Medium/High | Low |
