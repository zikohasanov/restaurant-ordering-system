# User Stories and Acceptance Criteria

| ID | Story | Priority | Requirements |
|---|---|---|---|
| US-01 | As a customer, I want to see current menu items and prices so that I can decide what to order. | Must | FR-01 |
| US-02 | As a customer, I want to configure item options and quantities so that my request reflects what I want. | Must | FR-02, RULE-05 |
| US-03 | As a customer, I want to review the full amount and pickup terms before submitting so that I can make an informed request. | Must | FR-03, FR-04 |
| US-04 | As a customer, I want a unique reference and clear pending status so that I know my request reached the restaurant without assuming it is accepted. | Must | FR-05, FR-06 |
| US-05 | As a shift lead, I want to accept or reject each new order with a reason when rejecting so that the kitchen commits only to feasible work. | Must | FR-07, FR-08 |
| US-06 | As kitchen staff, I want to move accepted orders through preparation and ready states so that the customer and cashier see consistent progress. | Must | FR-09 |
| US-07 | As a customer, I want to retrieve order status securely so that I can check progress without calling. | Must | FR-11, FR-13 |
| US-08 | As staff, I want a changed item to be unavailable for new orders while existing snapshots remain stable so that menu edits do not silently alter commitments. | Must | FR-14, RULE-06 |
| US-09 | As an owner, I want to review pilot measures and their limitations so that I can decide whether to improve or expand the service. | Should | FR-15 |

## US-04 — Submission and duplicate protection

**Given** the customer has a valid cart, contact value, and required acknowledgements  
**When** the customer submits and the system accepts the request  
**Then** exactly one order is created in `AwaitingReview` with a unique reference  
**And** the displayed confirmation says staff acceptance is still required.

**Given** the browser retries the same request with the same idempotency key  
**When** the original request has already created an order  
**Then** the system returns the original reference and does not create another order.

**Given** one cart item became unavailable before final submission  
**When** the system revalidates the cart  
**Then** no order is created  
**And** the customer is told which item needs attention.

## US-05 — Staff decision

**Given** an order is `AwaitingReview`  
**When** an authorized shift lead accepts it  
**Then** the state becomes `Accepted` with actor and timestamp recorded  
**And** a customer notification is queued.

**Given** an order is `AwaitingReview`  
**When** staff reject it without selecting a reason  
**Then** the action is blocked and the reason field is identified.

**Given** an order has already been accepted by another staff member  
**When** a stale page attempts to reject it  
**Then** the change is rejected and the current state is shown.

## US-06 — State progression

**Given** an order is `Accepted`  
**When** staff mark it `Preparing` and later `Ready`  
**Then** each transition is recorded with actor and timestamp  
**And** the customer view reflects the latest committed state.

**Given** an order is `Accepted`  
**When** staff attempt to mark it `Ready` directly  
**Then** the system blocks the skipped transition.

## US-07 — Secure status lookup

**Given** the customer has the correct reference and a valid one-time verification challenge  
**When** verification succeeds  
**Then** only the order state and pickup guidance are displayed.

**Given** a reference is valid but the challenge is wrong, expired, or rate-limited  
**When** status lookup is attempted  
**Then** no order details are disclosed  
**And** the customer receives a safe retry or support message.

## Definition of ready

- Story has an identifier, user outcome, priority, and linked requirement.
- Normal, validation, and relevant failure paths are discussed with the delivery team.
- Data, access, notification, and state-transition effects are identified.
- Acceptance examples can be exercised without relying on subjective wording such as “easy” or “fast.”

## Definition of done

- Acceptance criteria pass in the supported browsers and are linked to test evidence.
- Accessibility checks cover keyboard use, focus, labels, and announced errors.
- Security and privacy impacts are reviewed for changes to order or contact data.
- Event logging omits secrets and unnecessary personal data.
- Product owner accepts the behavior and support notes are updated.
