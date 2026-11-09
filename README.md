# Restaurant Ordering System — BA Case Study

> **Portfolio case study | Self-initiated | Fictional business context**  
> This project is an independent analysis exercise. It does not describe paid employment, a real client, or a production system.

## The situation

Harbor & Hearth is a fictional, single-location casual restaurant. Phone orders are written on paper during busy periods, staff re-enter them into a point-of-sale terminal, and customers have limited visibility into preparation or pickup status. The resulting handoffs create avoidable errors and interrupt front-of-house work.

This case study defines a small web ordering service for pickup orders. The intent is to reduce order transcription and make the customer and restaurant views agree on the same order state. Delivery, table service, loyalty, and POS integration are deliberately excluded from the first release.

## What is included

| Artifact | Purpose |
|---|---|
| [Business requirements](docs/business-requirements.md) | Context, outcomes, scope, stakeholders, assumptions, constraints, and business requirements |
| [Functional requirements](docs/functional-requirements.md) | Testable behavior and rules for the first release |
| [Non-functional requirements](docs/non-functional-requirements.md) | Measurable quality targets and operational constraints |
| [Use cases](docs/use-cases.md) | Main and alternate flows for placing, accepting, and cancelling an order |
| [User stories and acceptance criteria](docs/user-stories.md) | Traceable story slices with Given/When/Then examples |
| [Process flows](diagrams/process-flows.md) | As-is and to-be process views |
| [UML](diagrams/uml.md) | Use-case, activity, and sequence views |
| [Test scenarios](docs/test-cases.md) | Positive, negative, boundary, and state-transition checks |
| [Traceability](docs/traceability.md) | Link from business outcomes through requirements, stories, and tests |

## Operating model

The customer submits a pickup order and receives a reference number. Restaurant staff review and accept or reject it. Accepted orders move through `Accepted → Preparing → Ready → Collected`; rejected or cancelled orders are terminal. Payment is collected at pickup in the initial release, avoiding storage of payment credentials and reducing payment-provider dependencies.

The system must not promise a pickup time that the restaurant has not confirmed. Menu availability is checked at submission and again at staff acceptance because stock can change between those events. A price snapshot is retained on each order so later menu edits cannot rewrite the customer's submitted total.

## Decisions and trade-offs

- **Pickup only:** keeps the first release focused on one fulfilment path and avoids delivery dispatch, address validation, and courier handoffs.
- **Pay at pickup:** lowers integration and payment-data scope; the trade-off is that no-shows remain a business risk.
- **Staff acceptance:** protects kitchen capacity and catches unavailable items; customers wait for confirmation instead of receiving an unconditional promise.
- **No POS integration:** staff use a separate queue in the pilot. This adds duplicate entry for in-person orders but allows the workflow to be evaluated before integration investment.
- **Single location and one currency:** reduces configuration complexity. Multi-location tax, menu, and capacity rules require discovery before expansion.

## How to review

Read the business requirements first, then follow identifiers such as `BR-01`, `FR-04`, `US-03`, and `TC-07` through the traceability matrix. The diagrams are written in Mermaid and render on GitHub. Requirements marked as assumptions are proposals for this fictional exercise, not validated stakeholder decisions.

## Status

Analysis package for an interview portfolio. It is not an implementation specification for a real restaurant. The next useful validation step would be a short discovery session with an owner, shift lead, cashier, and a small sample of customers.
