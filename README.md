# ⚡ Zyven: Distributed Reliability Middleware
> **Eliminate Webhook Data Loss.** Zyven is a high-performance middleware that guarantees at-least-once delivery between your core services and external endpoints.

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status: Active Development](https://img.shields.io/badge/Status-Active--Development-orange)
![Build: Passing](https://img.shields.io/badge/Build-Passing-green)

---

## 🧐 The Engineering Problem
Calling external webhooks directly from your API creates a **tight coupling** and a **single point of failure**:
- ❌ **Downstream Downtime**: If the receiver is down, your data is lost.
- ❌ **Latency Spikes**: Waiting for a slow webhook response blocks your core API.
- ❌ **Zombies**: Failed events without logs are impossible to debug.

**Zyven acts as a shock absorber.** It separates **Acceptance** (Sync & Fast) from **Delivery** (Async & Reliable).

---

## 🏗️ System Architecture
Zyven is built for high availability and strict security isolation.

![Zyven High-Level Architecture](docs/assets/high_level_architecture.png)

---

## 🔄 Lifecycle: The Reliability Boundary
Once Zyven commits an event to the source-of-truth database, we take **100% ownership** of its delivery.

![Zyven End-to-End Flow](docs/assets/end_to_end_flow.png)

---

## 🚀 Key Features
- **🛡️ Idempotency Protection**: Never process the same event twice, even if the client retries the request.
- **🔄 Intelligent Retries**: Exponential backoff with jitter and configurable retry limits.
- **💀 Dead Letter Queue (DLQ)**: Failed events are automatically isolated for manual inspection and bulk replay.
- **🔐 HMAC Webhook Signing**: Every delivery includes a cryptographic signature for security validation.
- **🔍 Full Visibility**: Audit logs for every single HTTP attempt, including response headers and body.

---

## 📊 Data Model (ER Diagram)
Designed for high-concurrency event tracking and observability.

![Zyven Database ER Diagram](docs/assets/er_diagram.png)

---

## 🛠 Tech Stack
- **Engine**: Node.js + Express (TypeScript)
- **Infrastructure**: PostgreSQL (Persistence) + Redis (Queueing)
- **Logic**: BullMQ (Job Scheduling) + Prisma/Drizzle (ORM)
- **Security**: Dedicated Outgoing Proxy for SSRF protection

---

## 📂 Project Structure

```text
zyven/
├── client/           # Dashboard & Analytics (Next.js)
├── server/           # Core Reliability Engine (Node.js)
│   ├── src/
│   │   ├── api/      # Routes, Controllers, Validators
│   │   ├── core/     # Business Logic & Services
│   │   ├── workers/  # BullMQ Dispatch Processors
│   │   ├── models/   # DB Schemas & Persistence
│   │   ├── middleware/# Auth & Security Filters
│   │   └── config/   # Environment & DB Connections
│   └── tests/        # Integration & Load Testing
├── infra/            # Docker, K8s & Deployment Scripts
├── docs/             # Documentation & Diagrams
└── README.md
```

---

## 🔍 Visual Deep Dives
- [🧠 Technical Explainer & Logic Flows](docs/explainer.md): Detailed diagrams for Retries and Security.
- [🏗️ System Architecture Theory](docs/architecture.md): Engineering deep-dive into the "Reliability Boundary".

---

## 🏃 Quick Start (Local Development)
```bash
# 1. Clone & Install
git clone https://github.com/sultanxdev/zyven.git
cd zyven
npm install

# 2. Setup Environment
cp .env.example .env

# 3. Spin up Infrastructure
docker-compose up -d

# 4. Start Development Server
npm run dev
```

---

## 📄 License
Distributed under the MIT License. See `LICENSE` for more information.

---

## ✉️ Contact
**Sultan Alam** - [sultanalamdev@gmail.com](mailto:sultanalamdev@gmail.com)  
Project Link: [https://github.com/sultanxdev/zyven](https://github.com/sultanxdev/zyven)

---
*Built with ❤️ for mission-critical webhooks by Sultan*
