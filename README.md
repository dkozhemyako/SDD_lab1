## 🧱 Part 1 — Component Diagram (30%)

### Component Diagram (Mermaid)

```mermaid
graph LR
  ClientA --> API
  API --> Auth
  API --> MessageService
  MessageService --> DB[(Messages DB)]
  MessageService --> Queue
  Queue --> DeliveryService
  DeliveryService --> ClientB
  ClientB --> API
