# UML Views

## Use-case view

```mermaid
flowchart LR
  Customer([Customer])
  Staff([Shift lead / staff])
  Notify([Notification service])
  UC1((Browse menu))
  UC2((Submit pickup order))
  UC3((Verify and track order))
  UC4((Accept or reject order))
  UC5((Update preparation state))
  UC6((Notify customer))
  Customer --- UC1
  Customer --- UC2
  Customer --- UC3
  Staff --- UC4
  Staff --- UC5
  UC2 -. "queues" .-> UC6
  UC4 -. "queues" .-> UC6
  UC5 -. "queues" .-> UC6
  UC6 --- Notify
```

## Activity view: submit order

```mermaid
flowchart TD
  A([Start]) --> M[Browse menu and build cart]
  M --> C[Enter contact and acknowledge notices]
  C --> V{Required data valid?}
  V -- no --> E[Show accessible field errors]
  E --> C
  V -- yes --> R[Revalidate availability, options, and prices]
  R --> I{Cart unchanged and valid?}
  I -- no --> U[Show changed item or total]
  U --> M
  I -- yes --> D[Create order idempotently]
  D --> S[Save price snapshot and AwaitingReview state]
  S --> O[Show reference and pending message]
  O --> Z([End])
```

## Sequence view: staff decision

```mermaid
sequenceDiagram
  actor Lead as Shift lead
  participant UI as Staff queue
  participant API as Order service
  participant DB as Order store
  participant N as Notification worker
  Lead->>UI: Accept order
  UI->>API: Transition(orderId, AwaitingReview, Accepted)
  API->>DB: Check current state and persist event atomically
  DB-->>API: Updated order and event
  API-->>UI: Accepted with timestamp
  API->>N: Queue status notification
  N-->>API: Delivery outcome (asynchronous)
```

The expected-state check prevents a stale staff screen from silently overwriting a decision made by another staff member. Notification delivery is asynchronous and does not roll back the business state.
