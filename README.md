## 🧱 Part 1 — Component Diagram (30%)

### Component Diagram (Mermaid)

```mermaid
graph LR
  A[Client A (Web/Mobile)] -->|HTTP| API[Backend API]
  B[Client B (Web/Mobile)] -->|HTTP/WebSocket| API

  API --> Auth[Auth / Session]
  API --> MS[Message Service]
  MS --> DB[(Messages DB)]
  MS --> Q[(Message Queue)]
  Q --> DS[Delivery Service]
  DS -->|WebSocket push / Push notification| B

  B -->|ACK delivered/read (HTTP)| API
