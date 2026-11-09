# Non-Functional Requirements

These are proposed acceptance targets for a pilot. They need technical and business validation before implementation commitments.

| ID | Quality area | Requirement / measure | Rationale |
|---|---|---|---|
| NFR-01 | Performance | For 95% of menu and order-status requests under 100 concurrent sessions, server response time shall be under 2 seconds, excluding third-party notification delivery. | Keeps common customer tasks responsive. |
| NFR-02 | Availability | During published opening hours, monthly service availability shall be at least 99.5%, excluding announced maintenance. | Orders are time-sensitive; staff need a fallback. |
| NFR-03 | Security | Staff access shall require authenticated accounts, role checks, and secure session handling; unauthenticated users shall never access staff order queues. | Order contact information is personal data. |
| NFR-04 | Security | Customer lookup tokens shall be generated with sufficient entropy, expire after 15 minutes, be single-use, and be protected by rate limiting. | Reduces guessing and replay risk. |
| NFR-05 | Privacy | Collect only name, chosen contact channel, order details, and operational events needed for fulfilment; retention duration shall be configurable after jurisdictional review. | Minimizes personal-data exposure. |
| NFR-06 | Integrity | Every accepted state transition shall be persisted atomically with actor and timestamp; duplicate submit retries shall not duplicate orders. | Supports reliable fulfilment and audit. |
| NFR-07 | Accessibility | Customer and staff workflows shall target WCAG 2.2 AA; keyboard navigation, visible focus, labels, and error announcements shall be included in acceptance review. | Supports customers and staff using assistive technology. |
| NFR-08 | Compatibility | Customer ordering shall support current and previous major versions of Chrome, Safari, Firefox, and Edge on mobile and desktop, subject to pilot analytics. | Covers common browser use without an app install. |
| NFR-09 | Recovery | The service shall have documented backup and restore procedures with a proposed recovery point objective of 24 hours and recovery time objective of 4 hours. | Makes data loss and outage recovery expectations explicit. |
| NFR-10 | Observability | Operational logs shall record correlation ID, event type, outcome, and timestamp while excluding full contact values and verification tokens. | Enables support without logging secrets. |
| NFR-11 | Scalability | The first release shall support 100 concurrent customer sessions and 20 concurrent staff users without breaching NFR-01. | Sized for a single-location pilot with headroom. |
| NFR-12 | Maintainability | Business rules, state transitions, and retention settings shall be documented and configurable where practical; changes shall be traceable to a release. | Reduces risk when operating rules change. |

## Verification notes

Performance targets should be tested with representative menu data and a documented workload. Availability must be calculated from monitored service time during published hours. Security, privacy, retention, and accessibility targets require specialist review before launch. A pilot must not be described as compliant merely because these requirements are written down.
