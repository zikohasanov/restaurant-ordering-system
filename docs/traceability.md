# Traceability Matrix

| Business outcome / requirement | Functional requirement | Story | Test case | Coverage note |
|---|---|---|---|---|
| BO-01; BR-02 structured capture | FR-02, FR-05, FR-06 | US-02, US-04 | TC-02, TC-05, TC-06 | Transcription reduction is measured in pilot; test cases verify structured and duplicate-safe capture |
| BO-02; BR-04 order visibility | FR-11, FR-12, FR-13 | US-07 | TC-13, TC-14 | Status lookup and notification are separate mechanisms; notification delivery is not guaranteed |
| BO-03; BR-01 current menu | FR-01, FR-05, FR-14 | US-01, US-08 | TC-01, TC-07, TC-16 | Menu freshness and submission-time revalidation covered |
| BO-04; BR-03 explicit decision | FR-07, FR-08 | US-05 | TC-08, TC-09 | Staff capacity decision and permissions covered |
| BR-05 reject with reason | FR-08, FR-10 | US-05 | TC-09, TC-12 | Reason required for rejection/cancellation |
| BR-06 controlled fulfilment | FR-09, FR-15 | US-06 | TC-10, TC-11 | State progression and audit history covered |
| BR-07 stable price snapshot | FR-03, FR-05, FR-14 | US-03, US-08 | TC-04, TC-16 | Snapshot survives later catalog change |
| Quality target for responsive service | NFR-01, NFR-11 | US-01, US-07 | TC-18 | Load profile and measurement method must be agreed before execution |
| Quality target for accessible journeys | NFR-07 | US-03, US-07 | TC-17 | Manual assistive-technology review still required |

Coverage shows planned links, not completed testing. Any implementation change should update the affected requirement, story, and test identifiers together.
