# 🧠 Zyven: Technical Explainer & Logic Flows

This document provides detailed visualizations of the core engineering logic within Zyven. It covers retry mechanisms, idempotency, security, and the internal state machine.

---

## 1. Retry & Backoff Logic
Zyven uses an **Exponential Backoff with Jitter** strategy to prevent overwhelming downstream services.

```mermaid
flowchart TD
    Start([Worker Picks Job]) --> Attempt[Attempt Delivery]
    Attempt --> Success{Success 2xx?}
    
    Success -- Yes --> Delivered([Mark DELIVERED])
    Success -- No --> Permanent{Permanent 4xx?}
    
    Permanent -- Yes --> DLQ([Move to DLQ])
    Permanent -- No --> MaxRetries{Max Retries Exceeded?}
    
    MaxRetries -- Yes --> DLQ
    MaxRetries -- No --> Calculate[Calculate Next Delay]
    Calculate --> Wait[Schedule Delayed Job]
    Wait --> Attempt
    
    subgraph Backoff_Formula [Strategy: Exponential Backoff + Jitter]
    direction LR
    F[delay = base * 2^retry + jitter]
    end
```

---

## 2. Idempotency Enforcement
Zyven ensures that a single event is never processed multiple times, even if the client retries the ingestion request.

```mermaid
sequenceDiagram
    participant C as Client
    participant A as Zyven API
    participant D as PostgreSQL
    participant Q as Redis Queue

    C->>A: POST /v1/events (Idempotency-Key: X)
    A->>D: BEGIN TRANSACTION
    A->>D: SELECT * FROM events WHERE key = 'X'
    
    alt Key 'X' Found
        D-->>A: Existing Event Object
        A->>D: ROLLBACK
        A-->>C: 200 OK (Return Existing ID)
    else Key 'X' Not Found
        A->>D: INSERT INTO events (key, payload, status='RECEIVED')
        A->>D: COMMIT
        D-->>A: Success
        A->>Q: Push(event_id)
        A-->>C: 202 Accepted (New Event ID)
    end
```

---

## 3. Security: The Outgoing Proxy (SSRF Protection)
Zyven protects your infrastructure by ensuring workers never make direct network calls to untrusted external URLs.

![Zyven Security Boundary](assets/security_boundary.png)

---

## 4. Event Lifecycle State Machine
Every event in Zyven transitions through a well-defined set of states.

```mermaid
stateDiagram-v2
    [*] --> RECEIVED: API Accepts Event
    RECEIVED --> DISPATCHING: Enqueued to BullMQ
    DISPATCHING --> DELIVERED: 2xx Success
    DISPATCHING --> RETRY_SCHEDULED: 5xx/Timeout
    RETRY_SCHEDULED --> DISPATCHING: Backoff Delay Expired
    DISPATCHING --> DEAD_LETTERED: Max Retries / 4xx Failure
    RETRY_SCHEDULED --> DEAD_LETTERED: Retries Exhausted
    DEAD_LETTERED --> DISPATCHING: Manual Replay Triggered
    DELIVERED --> [*]
```

---

[Back to Architecture](architecture.md) | [Back to Main README](../README.md)
