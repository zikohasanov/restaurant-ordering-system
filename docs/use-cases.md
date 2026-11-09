# Use Cases

## UC-01 — Submit a pickup order

**Primary actor:** Customer  
**Goal:** Submit an itemized pickup request and receive a reference.  
**Preconditions:** Restaurant is accepting online orders; at least one item is published and available.  
**Trigger:** Customer chooses Checkout.

### Main flow

1. Customer reviews menu items, prices, availability, and declared options.
2. Customer adds one or more items and selects required options.
3. System shows an itemized subtotal and configured tax or charge treatment.
4. Customer enters a name and contact value for order verification, reviews pickup and allergy notices, and submits.
5. System revalidates availability, options, and prices.
6. System creates the order once, stores the submitted price snapshot, and sets state to `AwaitingReview`.
7. System displays the order reference and explains that the request is not confirmed until staff accept it.
8. System sends a verification message if the channel is available.

### Alternate and exception flows

- **A1: Item unavailable at final validation.** System does not create an order; it identifies the item and returns the customer to the cart.
- **A2: Price or option changed.** System presents the new details and requires renewed customer confirmation before submission.
- **A3: Network retry.** System uses the idempotency key and returns the original order response instead of creating a duplicate.
- **A4: Notification delivery fails.** Order remains in the queue; browser shows the reference and advises the customer how to check status or contact the restaurant.
- **A5: Invalid contact value or missing acknowledgement.** Submission is blocked and the relevant field receives an accessible error message.

**Postcondition:** A valid order exists once in `AwaitingReview`, or no order exists if validation fails.

## UC-02 — Review and accept or reject an order

**Primary actor:** Shift lead  
**Supporting actor:** Customer  
**Goal:** Make an explicit capacity and availability decision.  
**Preconditions:** Staff member is authenticated and the order is `AwaitingReview`.

### Main flow — accept

1. Shift lead opens the queue and selects the oldest awaiting order.
2. System shows item snapshots, options, notes, total, and order time.
3. Shift lead confirms capacity and availability, then selects Accept.
4. System verifies the order is still awaiting review and records actor and timestamp.
5. System changes state to `Accepted` and queues a customer notification.

### Alternate flow — reject

1. Shift lead determines the order cannot be fulfilled.
2. Shift lead selects Reject and chooses a required reason, such as capacity or unavailable item.
3. System records the reason and changes state to `Rejected`.
4. System queues a customer notification that the order was not accepted and gives the next step.

### Exceptions

- If another staff member has already acted, the system rejects the stale transition and displays the current state.
- If notification delivery fails, state remains authoritative; staff see delivery failure and can use the documented contact fallback.

## UC-03 — Track and collect an order

**Primary actors:** Customer, kitchen staff, cashier  
**Goal:** Keep customer-visible order state aligned with fulfilment.

1. Customer enters the order reference and completes the one-time verification challenge.
2. System displays only the current state and relevant pickup guidance.
3. Staff move an accepted order to `Preparing`, then `Ready` when packed.
4. System records each actor and timestamp and queues status notifications.
5. Cashier verifies the order reference at handoff, collects payment using the restaurant's existing process, and marks `Collected`.

**Exceptions:** Invalid or expired verification reveals no order data. A customer unable to complete verification uses the restaurant's support fallback. An order cannot skip a state or be marked collected twice.
