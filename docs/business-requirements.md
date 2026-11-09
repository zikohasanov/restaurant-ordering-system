# Business Requirements

**Document status:** Draft for a fictional portfolio case study  
**Product:** Harbor & Hearth pickup ordering  
**Version:** 1.0

## 1. Purpose and problem

Harbor & Hearth is a fictional casual restaurant considering a simple web channel for pickup orders. During peak periods, phone orders are written down and transcribed into the point-of-sale terminal. That handoff can introduce item, modifier, and quantity mistakes, while customers have to call to learn whether an order is ready.

The proposed service lets a customer browse a published menu, build a pickup order, submit it, and see status updates. Staff review incoming orders and manage the preparation state. This is a bounded analysis scenario, not a claim that the restaurant or project exists in the real world.

## 2. Desired outcomes and measures

Targets below are proposed pilot measures. A baseline must be collected before comparing results; the figures are not measured results.

| ID | Outcome | Proposed measure | Initial target |
|---|---|---|---|
| BO-01 | Reduce errors caused by re-keying phone orders | Weekly count of confirmed order corrections attributable to transcription, normalized per 100 orders | 30% lower than a four-week baseline after an eight-week pilot |
| BO-02 | Reduce status-check interruptions | Staff log of customer calls asking whether an order is ready, per 100 pickup orders | 25% lower than baseline after eight weeks |
| BO-03 | Keep ordering usable under normal peak load | Completion rate for valid submitted orders; exclude customer abandonment before submission | At least 98% successful processing during pilot operating hours |
| BO-04 | Keep staff workload manageable | Median staff handling time from receipt to acceptance or rejection | No more than 90 seconds per order during pilot |

These targets need owner approval and a measurement method before being treated as commitments. Seasonality, channel mix, and changes to staffing can affect comparisons.

## 3. Scope

### In scope for the first release

- One restaurant location, one published menu, and one configured local time zone.
- Customer browsing without account creation.
- Pickup orders for immediate preparation, subject to staff acceptance.
- Item options and notes limited to configured choices; free-form allergy claims are not treated as guarantees.
- Order submission, a unique reference, staff accept/reject action, and status updates.
- Customer lookup using the reference plus a verification detail sent to the submitted contact channel.
- Pay at pickup; no payment card data is collected or stored by the service.
- Staff view of incoming orders and a basic audit trail of state changes.

### Out of scope

- Delivery, courier dispatch, table reservations, dine-in ordering, and scheduled future orders.
- Customer profiles, saved payment methods, loyalty points, promotions, gift cards, and reviews.
- POS, inventory, accounting, kitchen display, or third-party delivery integrations.
- Automatic capacity promises, dynamic prep-time estimation, and multi-location routing.
- Medical or allergen safety certification. Menu information can identify declared ingredients, but cross-contamination and suitability must be confirmed by restaurant staff.

## 4. Stakeholders

| Stakeholder | Interest / responsibility | Involvement |
|---|---|---|
| Restaurant owner | Commercial outcomes, menu policy, operating hours, approval of pilot measures | Accountable for business decisions |
| Shift lead | Order acceptance, capacity, exceptions, and staff access | Validates operational workflow |
| Kitchen staff | Clear item, modifier, quantity, and note presentation | Reviews preparation handoff |
| Cashier / front-of-house staff | Pickup identity check and payment collection | Uses ready and collected states |
| Customer | Accurate menu, clear order confirmation, useful status | Uses public ordering and status views |
| Product / analyst role | Elicit, document, prioritize, and trace requirements | Portfolio author role-play; not a real appointment |
| Implementation team | Design, build, secure, and operate the service | Consulted on feasibility and quality targets |

## 5. Assumptions

- **A-01:** The restaurant has stable internet and a staff device available during service.
- **A-02:** The owner can maintain prices, availability, and item options in an admin interface.
- **A-03:** Customers can access the service on a current mobile or desktop browser.
- **A-04:** Staff can make a pickup order decision within a defined service window.
- **A-05:** The restaurant collects payment at pickup using its existing process.
- **A-06:** The restaurant can define a channel (email or SMS) for one-time order verification; channel/provider selection is a discovery item.
- **A-07:** Currency, tax treatment, retention period, and applicable privacy notices are confirmed before a real deployment.

## 6. Constraints and dependencies

- A customer-facing status must not expose personal contact details or be guessable by sequential order number alone.
- Any contact channel requires consent language, delivery-failure handling, and an operational fallback.
- Menu, tax, privacy, accessibility, and records requirements vary by jurisdiction and require local review.
- Staff must be able to continue accepting phone and walk-in orders if the ordering service is unavailable.
- The initial release depends on accurate menu and availability maintenance by restaurant staff.

## 7. Business requirements

| ID | Requirement | Priority | Rationale |
|---|---|---|---|
| BR-01 | The service shall present the restaurant's current published menu, prices, options, and availability to customers. | Must | Customers need a dependable source of orderable items. |
| BR-02 | The service shall capture pickup orders in a structured format that staff can review without re-keying item details. | Must | Addresses the transcription problem. |
| BR-03 | A submitted order shall remain unconfirmed until staff accepts it. | Must | Prevents the service from promising kitchen capacity it cannot verify. |
| BR-04 | Customers shall be able to see the current state of an order using a secure lookup method. | Must | Reduces status-check calls while protecting order information. |
| BR-05 | Staff shall be able to reject an order with a reason and communicate the outcome. | Must | Provides a clear path for capacity or stock exceptions. |
| BR-06 | Staff shall be able to move an accepted order through preparation, ready, and collected states. | Must | Aligns customer visibility with the operating workflow. |
| BR-07 | The service shall retain the submitted item and price snapshot for each order. | Must | Preserves what was submitted if the menu changes later. |
| BR-08 | The service shall support a pilot measurement of order corrections and status-check interruptions without claiming causality from raw counts alone. | Should | Allows evidence-based evaluation. |

## 8. Risks and responses

| Risk | Effect | Proposed response |
|---|---|---|
| Staff do not monitor the order queue | Customers wait without a decision | Set an order review cadence; notify staff; define timeout and fallback message |
| Menu availability becomes stale | Orders are rejected after submission | Recheck on staff acceptance; make stock maintenance part of shift opening/closing |
| Customers infer allergen safety from menu text | Potential harm and legal exposure | Use explicit limitations; route allergy questions to staff; validate content with owner |
| Verification message is delayed or intercepted | Customer cannot access status or another person can | Use high-entropy one-time lookup token, short expiry, rate limits, and support fallback |
| Pilot metric changes for unrelated reasons | Misleading success claim | Record baseline, channel mix, hours, and staffing; report limitations |

## 9. Open questions before real-world use

1. Which contact channel is available and acceptable to customers, and what is the consent wording?
2. What is the order review timeout and what should a customer be told if it expires?
3. How are taxes, service charges, refunds, and rejected orders handled under local rules?
4. Who may edit menus and availability, and what audit record is needed?
5. How long should order and contact data be retained?
6. What accessibility standard and supported browsers will be used for acceptance?
7. What is the fallback process when the staff device or internet is unavailable?
