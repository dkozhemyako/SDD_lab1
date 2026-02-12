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
```

## 🔁 Part 2 — Sequence Diagram (25%)

### Scenario
User **A sends a message** to user **B who is offline**.  
The system supports message statuses **sent / delivered / read** via **client acknowledgements (ACK)**.

### Task
Describe the interaction sequence in time, including:
- storing the message and setting status **sent**;
- asynchronous delivery attempt;
- what happens when **ACK is missing** because user B is offline;
- later, when B becomes online, how **ACK delivered** and **ACK read** update statuses.

```mermaid
sequenceDiagram
  participant A as User A
  participant ClientA as Client A
  participant API
  participant Msg as Message Service
  participant DB
  participant Queue
  participant Delivery as Delivery Service
  participant B as User B (offline)
  participant ClientB as Client B

  A->>ClientA: Send message to B
  ClientA->>API: POST /messages (to=B)
  API->>Msg: createMessage()
  Msg->>DB: save(message, status=sent)
  Msg->>Queue: enqueue delivery
  API-->>ClientA: 202 Accepted (status=sent)

  Queue->>Delivery: deliver(messageId)
  Delivery-->>B: attempt delivery
  Note over Delivery,B: B is offline -> no ACK delivered/read\nMessage remains status=sent

  Note over B,ClientB: Later: B becomes online
  Delivery->>ClientB: deliver message
  ClientB->>API: ACK delivered (messageId)
  API->>Msg: ackDelivered(messageId)
  Msg->>DB: update status=delivered

  ClientB->>API: ACK read (messageId)
  API->>Msg: ackRead(messageId)
  Msg->>DB: update status=read
```
