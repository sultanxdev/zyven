# ⚡ Zyven

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Under Development](https://img.shields.io/badge/Status-Under%20Development-orange.svg)](#)
[![Tech Stack](https://img.shields.io/badge/Stack-Next.js%20%7C%20Node.js%20%7C%20PostgreSQL-blue.svg)](#)

Zyven is a high-performance **Reliability Middleware** designed to sit between your application and external webhooks. It guarantees delivery, provides deep visibility, and ensures automatic recovery for mission-critical event-driven architectures.

> [!IMPORTANT]
> **Work in Progress**: Zyven is currently in active development. Features and APIs are subject to change.

---

## 🚀 Key Features

- **✅ At-Least-Once Delivery**: Guaranteed event delivery even if the receiver is temporarily down.
- **✅ Durable Persistence**: Every event is persisted to a source-of-truth database before processing.
- **✅ Idempotency Protection**: Built-in mechanisms to prevent duplicate processing of the same event.
- **✅ Bounded Retries**: Intelligent exponential backoff with configurable retry limits.
- **✅ Dead Letter Queue (DLQ)**: Failed events are automatically routed to a DLQ for manual inspection.
- **✅ Observability**: Full audit logs for every delivery attempt and response code.
- **✅ Manual Replay**: Easily trigger replays for failed events after resolving downstream issues.

---

## 🏗 System Architecture

Zyven uses a decoupled architecture to ensure high availability and durability.

### High-Level Flow
![Architecture Diagram](../server/src/assets/my-d.png)

### End To End Flow
![End To End Flow](../server/src/assets/my-d.png)
    
### Data Model
![Data Model](../server/src/assets/my-d.png)

## 🛠 Tech Stack

- **Frontend**: [Next.js](https://nextjs.org/) + [TypeScript](https://www.typescriptlang.org/) + [Tailwind CSS](https://tailwindcss.com/)
- **Backend**: [Node.js](https://nodejs.org/) + [Express](https://expressjs.com/)
- **Database**: [PostgreSQL](https://www.postgresql.org/)
- **Queue/Jobs**: [Redis](https://redis.io/) + BullMQ
- **Infrastructure**: [Docker](https://www.docker.com/) + [AWS](https://aws.amazon.com/)

---


## 📊 How to Add Your Own Diagrams

You can add diagrams to this README in two main ways:

### 1. Using Mermaid (Recommended)
GitHub natively supports **Mermaid.js**. You can write your diagrams in plain text right inside your Markdown files.
- **Pro**: Easy to update, version-controlled, and accessible.
- **Example**:
  \```mermaid
  graph LR
    A[Start] --> B[Finish]
  \```

### 2. Using Custom Images
If you prefer using tools like **Excalidraw**, **Lucidchart**, or **Canva**:
1. Export your diagram as a `.png` or `.svg`.
2. Create an `assets` folder in your project root.
3. Upload the image there.
4. Add it to the README using:
   `![Architecture Diagram](./assets/my-diagram.png)`

---

## 🤝 Contributing

Contributions are what make the open-source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## ✉️ Contact

**Sultan Alam** - [sultanalamdev@gmail.com](mailto:sultanalamdev@gmail.com)

Project Link: [https://github.com/sultanxdev/zyven](https://github.com/sultanxdev/zyven)

---
*Built with ❤️ for reliable webhooks.*


