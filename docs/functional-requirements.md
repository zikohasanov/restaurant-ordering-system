# Functional Requirements

Each requirement is written so a tester can observe pass or fail behavior. Priorities use Must / Should / Could for the fictional first-release scope.

| ID | Requirement | Priority | Related business requirement |
|---|---|---|---|
| FR-01 | The system shall display only menu items marked published, with current price, configured options, and availability. | Must | BR-01 |
| FR-02 | The customer shall be able to add an available item, choose required options, set quantity from 1 to 20, and remove it before submission. | Must | BR-02 |
| FR-03 | The system shall calculate subtotal from the item price snapshot and selected options; the displayed total shall identify any tax or charge treatment configured for the location. | Must | BR-02, BR-07 |
| FR-04 | Before submission, the customer shall provide a name, a valid contact value for the configured verification channel, and acknowledge the pickup and allergy notices. | Must | BR-02, BR-04 |
| FR-05 | On submission, the system shall revalidate item availability and prices, create one order reference, store a price snapshot, and show that the order is awaiting staff review. | Must | BR-02, BR-03, BR-07 |
| FR-06 | Repeated submission of the same request caused by a retry shall not create duplicate orders. | Must | BR-02 |
| FR-07 | Authorized staff shall see new orders in receipt-time order with items, options, notes, submitted total, and contact-independent reference. | Must | BR-02 |
| FR-08 | Authorized staff shall accept an awaiting order or reject it with a required reason from a controlled list and optional detail. | Must | BR-03, BR-05 |
| FR-09 | An accepted order shall progress only through Preparing, Ready, and Collected; a state transition shall record actor and timestamp. | Must | BR-06 |
| FR-10 | Staff shall be able to cancel an order before it is collected, with a reason; the system shall notify the customer and record the event. | Should | BR-05, BR-06 |
| FR-11 | The customer shall retrieve status using an unguessable reference and a one-time verification step. | Must | BR-04 |
| FR-12 | The system shall notify the customer when staff accept, reject, mark ready, or cancel the order, subject to delivery-channel availability. | Should | BR-04, BR-05 |
| FR-13 | A customer-facing notification shall not include unnecessary contact details, payment credentials, or staff-only notes. | Must | BR-04 |
| FR-14 | Staff shall be able to mark an item unavailable; subsequent submissions shall be blocked for that item, while existing order snapshots remain unchanged. | Must | BR-01, BR-07 |
| FR-15 | The system shall retain an append-only event history for order creation and state changes. | Should | BR-06, BR-08 |
| FR-16 | The system shall display clear validation and recovery guidance when submission, verification, or notification fails. | Must | BR-02, BR-04 |

## Business rules

| ID | Rule |
|---|---|
| RULE-01 | Order states are `AwaitingReview`, `Accepted`, `Preparing`, `Ready`, `Collected`, `Rejected`, and `Cancelled`. `Rejected`, `Collected`, and `Cancelled` are terminal. |
| RULE-02 | Only `AwaitingReview` can transition to `Accepted` or `Rejected`. |
| RULE-03 | `Accepted → Preparing → Ready → Collected` is the normal fulfilment path. No state may be skipped. |
| RULE-04 | Staff may cancel `Accepted`, `Preparing`, or `Ready` orders; a cancellation requires a reason. Rejected, collected, and cancelled orders cannot be reopened. |
| RULE-05 | A submission is valid only if every item is published, available, within quantity limits, and has all required options selected at the time of final validation. |
| RULE-06 | The amount shown at submission is based on a stored snapshot. A later menu price change does not alter an existing order. |
| RULE-07 | Payment is due at pickup in the first release; the ordering service does not collect or store payment credentials. |
| RULE-08 | A customer can view only an order for which the lookup reference and verification challenge are both valid. |
| RULE-09 | Free-text notes are limited to 250 characters and are not a substitute for an allergy safety confirmation. |
| RULE-10 | A request retry with the same idempotency key returns the original order result within the key-retention window; a different deliberate submission creates a new order. |

## Validation and exception behavior

- If an item becomes unavailable between cart display and submission, reject the submission without creating an order and identify the affected item.
- If a configured option is removed or repriced before submission, require the customer to review the changed selection and total.
- If the notification provider is unavailable after order creation, preserve the order and show the reference in the browser; retry notification asynchronously and expose delivery status to staff.
- If staff do not decide within the configured review window, keep the order in `AwaitingReview`, flag it for staff, and show the customer a neutral pending message. Do not auto-accept.
- A stale staff page must not overwrite a newer order state; reject the transition and refresh the current state.
