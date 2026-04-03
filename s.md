Nice, this will make your assignment look much more complete 👌
I’ll add a clean **architecture flowchart (Mermaid)** that matches your rewritten structure.

You can paste this directly into Markdown (GitHub, Notion, etc.) 👇

---

## 10. Architecture Flowchart

```mermaid
flowchart TD

    %% Upstream services
    subgraph Upstream_Services
        S1[Order Service]
        S2[Message Service]
        S3[User Service]
    end

    %% Entry point
    S1 --> API
    S2 --> API
    S3 --> API

    %% Core system
    subgraph Notification_System
        API[API Gateway<br/>Auth + Rate Limit]
        NS[Notification Service]

        Cache[(Redis Cache<br/>Preferences + Idempotency)]
        DB[(Cassandra DB<br/>Notifications)]

        MQ[Message Broker<br/>Kafka / RabbitMQ]

        API --> NS
        NS --> Cache
        NS --> DB
        NS --> MQ
    end

    %% Workers
    subgraph Workers
        W1[Push Worker]
        W2[Email Worker]
        W3[In-App Worker]
        Retry[Retry / DLQ]
    end

    MQ --> W1
    MQ --> W2
    MQ --> W3

    W1 -. fail .-> Retry
    W2 -. fail .-> Retry
    W3 -. fail .-> Retry
    Retry -. retry .-> MQ

    %% External providers
    subgraph Delivery
        Push[FCM / APNs]
        Email[SendGrid / SES]
        WS[WebSocket / SSE]
    end

    W1 --> Push
    W2 --> Email
    W3 --> WS
    W3 --> DB

    %% Users
    Push --> User[User Devices]
    Email --> User
    WS --> User
```

---

### 🔥 Small tip (for your submission)

If your teacher is strict, you can say something like:

> *“The following diagram illustrates the end-to-end data flow from event generation to final delivery across different channels.”*

---

If you want next level:

* I can convert this into a **beautiful PNG diagram**
* or make a **presentation slide version (like for defense)**
* or simplify it for explaining in 1–2 minutes 🎤
