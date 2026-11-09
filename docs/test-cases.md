# Test Cases

Test cases are proposed verification examples for the requirements. They are not evidence that software has been built or tested.

| ID | Requirement | Scenario | Expected result |
|---|---|---|---|
| TC-01 | FR-01 | Open menu with published, unpublished, available, and unavailable items | Only published items appear; unavailable items are clearly marked and cannot be added |
| TC-02 | FR-02 | Add item with required option and quantity 1 | Cart displays selected option and quantity |
| TC-03 | FR-02 | Submit quantity 0, 21, or missing required option | Input is blocked with a field-level explanation |
| TC-04 | FR-03, RULE-06 | Change menu price after cart creation but before submission | New price is shown; customer must confirm revised amount; saved order uses confirmed snapshot |
| TC-05 | FR-05 | Submit valid cart once | One order exists in `AwaitingReview`; reference is displayed |
| TC-06 | FR-06, RULE-10 | Retry identical submission after a network timeout | Original order/reference returned; no duplicate order created |
| TC-07 | FR-05, RULE-05 | Item becomes unavailable just before submit | Submission creates no order and identifies item |
| TC-08 | FR-07 | Staff member without role permission opens staff queue | Access is denied and event is logged without exposing order data |
| TC-09 | FR-08 | Reject awaiting order without a reason | Rejection is blocked; reason is required |
| TC-10 | FR-09, RULE-03 | Attempt `Accepted → Ready` without `Preparing` | Transition is rejected; stored state remains unchanged |
| TC-11 | FR-09 | Perform `Accepted → Preparing → Ready → Collected` | Each transition succeeds once and records actor and timestamp |
| TC-12 | FR-10, RULE-04 | Cancel a preparing order with reason | State becomes `Cancelled`; event and notification are queued |
| TC-13 | FR-11, NFR-04 | Use correct reference and valid one-time challenge | Current state and pickup guidance are shown; token cannot be reused |
| TC-14 | FR-11, NFR-04 | Use invalid reference or repeated incorrect challenge | No order data revealed; rate limit and safe message apply |
| TC-15 | FR-12, FR-16 | Notification provider fails after order creation | Order remains in queue; browser shows reference; retry/failure visible to staff |
| TC-16 | FR-14 | Mark item unavailable after an order was submitted | New submission is blocked; existing order item and price snapshot do not change |
| TC-17 | NFR-07 | Complete checkout with keyboard and screen reader | All controls have names, focus order is visible, and errors are announced |
| TC-18 | NFR-01 | Run agreed workload at 100 concurrent sessions | 95th percentile server response remains below 2 seconds, excluding notifications |

## State-transition coverage

| From | To | Expected |
|---|---|---|
| AwaitingReview | Accepted | Allowed for authorized staff |
| AwaitingReview | Rejected | Allowed with reason |
| Accepted | Preparing | Allowed |
| Preparing | Ready | Allowed |
| Ready | Collected | Allowed |
| Accepted / Preparing / Ready | Cancelled | Allowed with reason |
| Any terminal state | Any other state | Rejected |
| Accepted | Ready | Rejected because it skips a state |

## Test data and execution notes

Use fictional menu data and synthetic contact values. Do not place real customer details in screenshots, fixtures, or logs. For price and availability race cases, arrange the menu update between cart load and final submission. Record browser, build, result, and defect reference when these cases are executed against an implementation.
