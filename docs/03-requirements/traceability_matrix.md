# Traceability Matrix

This matrix maps phase 02 use cases to phase 03 requirements.

| Use Case | Related Requirements |
|---|---|
| UC-01 Morning Market Brief | FR-002, FR-003, FR-004, FR-006, FR-007, FR-010, FR-012, FR-013, FR-015, FR-018 |
| UC-02 Ticker Deep Dive | FR-003, FR-004, FR-006, FR-009, FR-010, FR-011, FR-012, FR-013 |
| UC-03 Breaking Alert | FR-003, FR-004, FR-012, FR-015, FR-016, FR-018 |
| UC-04 Signal Review | FR-006, FR-010, FR-011, FR-012, NFR-004 |
| UC-05 Decision Journal | FR-013, FR-014 |
| UC-06 Watchlist Management | FR-001, FR-002, FR-017 |
| UC-07 Report Archive Review | FR-012, FR-013 |
| UC-08 Source Traceability | FR-004, FR-005, FR-012, NFR-004 |
| UC-09 Local Setup and Configuration | FR-017, FR-018, NFR-001, NFR-006 |

## Coverage Notes

- Every high-priority use case has at least one functional requirement.
- Source traceability is treated as a core requirement, not a reporting detail.
- LLM cost control is represented as both an architectural constraint and a non-functional requirement.
- Notifications are outside the core MVP and must not be required for report generation.
- UI is not required for the first deep-dive workflow, but structured storage is required so a UI can be added later.
