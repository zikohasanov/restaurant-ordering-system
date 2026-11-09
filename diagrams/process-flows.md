# Process Flows

## As-is: phone order and manual re-entry

```mermaid
flowchart LR
  C[Customer calls] --> S[Staff writes order on paper]
  S --> K[Kitchen receives verbal or paper handoff]
  S --> POS[Staff re-enters order in POS]
  POS --> E{Transcription or availability issue?}
  E -- yes --> R[Staff calls customer to clarify]
  E -- no --> P[Prepare order]
  P --> Q[Customer calls to ask status]
  Q --> H[Staff checks kitchen and answers]
  P --> U[Customer picks up and pays]
```

**Observed problem hypothesis:** handwritten capture, re-entry, and status calls add handoffs. This is a hypothesis to validate with staff observation, not measured evidence.

## To-be: web request with explicit staff decision

```mermaid
flowchart TD
  C[Customer browses menu] --> CART[Build cart and choose options]
  CART --> VALID{Items and prices still valid?}
  VALID -- no --> FIX[Show changes; customer reviews cart]
  FIX --> CART
  VALID -- yes --> SUB[Submit pickup request]
  SUB --> AR[Awaiting staff review]
  AR --> DEC{Staff decision}
  DEC -- reject --> REJ[Record reason and notify customer]
  DEC -- accept --> ACC[Accept and notify customer]
  ACC --> PREP[Preparing]
  PREP --> READY[Ready; notify customer]
  READY --> PICK[Verify reference; collect payment]
  PICK --> COL[Collected]
```

Staff acceptance is a deliberate control: successful submission means the request was received, not that the restaurant has committed to fulfil it.
