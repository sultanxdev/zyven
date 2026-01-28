# 🏗️ Zyven System Architecture & Theory

This document provides a deep dive into the engineering principles and architectural decisions behind Zyven.

## 1. Reliability Philosophy
Zyven is built on the principle of **Durable Acknowledgment**. Once an event enters our system and is persisted to the PostgreSQL storage layer, we take full ownership of its delivery.

### The Reliability Boundary
The "Reliability Boundary" occurs at the moment of database commit. 
- **Before Commit**: The client is responsible for retrying the request.
- **After Commit**: Zyven guarantees delivery attempts until success or explicit DLQ routing.

## 2. Component Breakdown

### API Ingest Layer
- **Stateless & Scalable**: Can be scaled horizontally behind a Load Balancer.
- **Synchronous Logic**: Authentication, schema validation, and idempotency checks are performed here.
- **Payload Sanitization**: Ensures no malicious payloads enter the system.

### Persistence Layer (PostgreSQL)
- **Source of Truth**: Not the queue, but the DB.
- **Indices**: Optimized for fast lookups by `idempotency_key` and `status`.

### Task Queue (Redis + BullMQ)
- **Message Passing**: Acts as the nervous system for the workers.
- **Delayed Jobs**: Handles exponential backoff logic without blocking worker threads.

### Dispatcher Workers
- **Compute Heavy**: Responsible for generating signatures (HMAC-SHA256), managing timeouts, and handling HTTP responses.
- **Isolation**: Each worker operates independently, fetching jobs from the queue.

## 3. Security: Outgoing Proxies & SSRF
To prevent **Server-Side Request Forgery (SSRF)**, Zyven workers do not call customer endpoints directly. Instead:
1. Workers send requests to an **Outgoing Proxy**.
2. Proxies enforce **IP Whitelisting** and **DNS resolution filtering**.
3. This prevents workers from being tricked into calling internal VPC metadata services or private IPs.

---

## 4. State Lifecycle
| State | Description |
| :--- | :--- |
| `RECEIVED` | Event persisted to DB, but not yet enqueued. |
| `DISPATCHING` | Event is in the queue or being handled by a worker. |
| `DELIVERED` | Target endpoint returned a 2xx status code. |
| `RETRY_SCHEDULED` | Delivery failed with a retryable error (e.g., 503, Timeout). |
| `DEAD_LETTERED` | Retries exhausted or permanent failure (4xx). |

---

## 5. Sequence Diagram (Ingestion to Delivery)

![Zyven End-to-End Flow](assets/end_to_end_flow.png)

---

[Back to Main README](../README.md)