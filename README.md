## 🧱 Part 1 — Component Diagram (30%)

### Component Diagram (Mermaid)

```mermaid
graph LR
  A[Client A (Web or Mobile)] -->|HTTP| API[Backend API]
  B[Client B (Web or Mobile)] -->|HTTP or WebSocket| API

  API --> Auth[Auth and Session]
  API --> MS[Message Service]
  MS --> DB[(Messages DB)]
  MS --> Q[(Message Queue)]
  Q --> DS[Delivery Service]
  DS -->|WebSocket push or Push notification| B

  B -->|ACK delivered and read (HTTP)| API
