# Requirements Specification

## Functional Requirements

| ID | Description | Priority | Acceptance Criteria | Rationale |
|---|---|---|---|---|
| FR-001 | The system shall parse PostgreSQL and MySQL EXPLAIN plans, identify sequential scans on large tables, and recommend index column definitions. | High | **Pass:** a plan with a sequential scan on a table of at least 100,000 rows and a filtered column produces a matching index recommendation. **Fail:** it is marked optimized or produces no recommendation. | Converts plan evidence into actionable tuning work. |
| FR-002 | The system shall ingest slow-query logs from configured PostgreSQL and MySQL sources and retain query text, duration, timestamp, database, and normalized query fingerprint. | High | **Pass:** 100 submitted valid records are retained with all required fields. **Fail:** any field is absent or a record is silently lost. | Reliable ingestion enables analysis and trending. |
| FR-003 | The system shall rank slow-query fingerprints by cumulative execution time, average execution time, and execution count for a selected period. | High | **Pass:** ranking and metrics equal independently calculated results. **Fail:** any displayed metric or ordering differs. | Focuses effort on highest-impact queries. |
| FR-004 | The system shall allow a Database Administrator to review, accept, dismiss, or annotate each generated index recommendation. | Medium | **Pass:** authorized users can set and retain all states with user and timestamp. **Fail:** an unauthorized user changes a state or a change is lost. | Maintains human control and an audit trail. |
| FR-005 | The system shall generate a weekly optimization digest with top slow queries, plan issues, recommendation status, and week-over-week changes. | Medium | **Pass:** a scheduled digest has all four sections for the preceding seven days. **Fail:** a required section or comparison is missing. | Removes manual report assembly. |

## Non-Functional Requirements

| ID | Type | Description | Priority | Acceptance Criteria | Rationale |
|---|---|---|---|---|---|
| NFR-001 | Performance & Security | Slow-query log ingestion shall process up to 5,000 records per minute without dropping records or degrading the host database. | High | **Pass:** over 15 minutes at 5,000 records/minute, all records persist and host query latency rises by no more than 5%. **Fail:** any record is lost or the threshold is exceeded. | Avoids turning observability into a production risk. |
| NFR-002 | Security | The system shall encrypt stored query logs and enforce role-based access to data and recommendation actions. | High | **Pass:** logs are encrypted at rest; Backend Leads cannot approve recommendations; access is audited. **Fail:** plaintext storage, unauthorized action, or missing audit event occurs. | Query logs may contain sensitive data and approvals are privileged. |
