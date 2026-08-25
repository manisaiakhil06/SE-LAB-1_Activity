# SE Lab 1: Requirements Engineering & UML Use-Case Modelling

**Course:** Software Engineering Lab  
**Institution:** PES University - Dept. of CSE  
**Student:** Nallamalli Kanaka Mani Sai Akhil  
**USN:** PES1UG24CS290  
**Problem Statement #44:** Database Query Performance Profiler  

---

## 📌 Deliverables Overview

- 📄 **Problem Statement:** [`docs/Problem_Statement.pdf`](docs/Problem_Statement.pdf)
- 📋 **Requirements Specification:** [`docs/Requirements_Specification.md`](docs/Requirements_Specification.md)
- 📝 **Requirements Table:** [`docs/Requirements_Table.docx`](docs/Requirements_Table.docx)
- 📐 **Use Case Specification:** [`docs/Use_Case_Specification.md`](docs/Use_Case_Specification.md)
- 📄 **Use Case Flow PDF:** [`docs/Use_Case_Flow.pdf`](docs/Use_Case_Flow.pdf)
- 📊 **UML Diagram:** [`diagrams/UML_Use_Case_Diagram.pdf`](diagrams/UML_Use_Case_Diagram.pdf)
- 🖼️ **UML Diagram Image:** [`diagrams/UML_Use_Case_Diagram.png`](diagrams/UML_Use_Case_Diagram.png)
- 💻 **UML Diagram Source:** [`diagrams/use_case_diagram.puml`](diagrams/use_case_diagram.puml)

---

## 📋 1. Requirements Table

### Functional Requirements (FR-001 to FR-005)

| ID | Type / Category | Description | Priority | Acceptance Criteria | Rationale |
|---|---|---|---|---|---|
| **FR-001** | Plan Analysis & Index Recommendation | The system shall parse PostgreSQL and MySQL EXPLAIN plans, identify sequential scans on large tables, and recommend index column definitions. | High | **Pass:** A sequential scan on a table with at least 100,000 rows produces an index recommendation. **Fail:** The scan is marked optimized or no recommendation is produced. | Converts execution-plan evidence into an actionable optimization recommendation. |
| **FR-002** | Log Ingestion | The system shall ingest slow-query logs from configured PostgreSQL and MySQL sources and retain query text, duration, timestamp, database, and normalized query fingerprint. | High | **Pass:** All 100 submitted valid log records are stored with required fields. **Fail:** Any record or required field is missing. | Reliable log data is required for analysis and trends. |
| **FR-003** | Query Ranking | The system shall rank slow-query fingerprints by cumulative execution time, average execution time, and execution count. | High | **Pass:** Ranking and metrics match independently calculated test data. **Fail:** Displayed ordering or metrics are incorrect. | Helps teams prioritize the highest-impact queries. |
| **FR-004** | Recommendation Review | The system shall allow a Database Administrator to accept, dismiss, or annotate each generated index recommendation. | Medium | **Pass:** An authorized administrator can update and retain a recommendation decision. **Fail:** An unauthorized user changes a decision or the change is not retained. | Keeps tuning decisions under administrator control with an audit trail. |
| **FR-005** | Weekly Digest | The system shall generate a weekly performance optimization digest containing top slow queries, plan issues, recommendation status, and week-over-week changes. | Medium | **Pass:** The generated digest contains all required sections for the previous seven days. **Fail:** A required section or comparison is missing. | Provides a repeatable optimization summary without manual reporting. |

### Non-Functional Requirements (NFR-001 and NFR-002)

| ID | Type / Category | Description | Priority | Acceptance Criteria | Rationale |
|---|---|---|---|---|---|
| **NFR-001** | Performance & Security | Slow-query log ingestion shall process up to 5,000 records per minute without dropping records or causing significant database-host degradation. | High | **Pass:** During a 15-minute test at 5,000 records/minute, no records are lost and host query latency increases by no more than 5%. **Fail:** Any record is lost or latency exceeds the threshold. | The profiler must not become a production performance risk. |
| **NFR-002** | Security & Access Control | The system shall encrypt stored query logs and restrict access to data and recommendation actions using role-based access control. | High | **Pass:** Logs are encrypted at rest; Backend Leads cannot approve recommendations; access events are audited. **Fail:** Plaintext storage, unauthorized action, or missing audit event occurs. | Query logs may contain sensitive data and approvals are privileged actions. |

---

## 📊 2. UML Use-Case Diagram

![UML Use-Case Diagram](diagrams/UML_Use_Case_Diagram.png)

[Download UML Use-Case Diagram PDF](diagrams/UML_Use_Case_Diagram.pdf)

---

## 📐 3. Core Use-Case Flow Specification (UC-01)

- **Use Case ID:** UC-01  
- **Use Case Name:** Analyse Query Execution Plan  
- **Primary Actor:** Database Administrator  
- **Secondary Actors:** Backend Lead, DBMS / Log Source  

### Preconditions

- The Database Administrator is authenticated.
- A slow-query record and valid PostgreSQL or MySQL EXPLAIN plan are available.

### Postconditions

- The analysis result, detected plan issues, and generated index recommendation are stored in the query profile.
- The Database Administrator’s decision and annotation are retained for auditing.

### Main Success Scenario (MSS)

1. The Database Administrator opens a slow-query record.
2. The system displays normalized query details and the execution plan.
3. The Database Administrator selects **Analyse Plan**.
4. The system validates and parses plan operators, predicates, estimated rows, and costs.
5. The system identifies sequential scans and evaluates filtered or join columns for missing suitable indexes.
6. The system generates a proposed index definition and confidence level.
7. The system displays the identified issue, supporting evidence, and recommendation.
8. The Database Administrator accepts, dismisses, or annotates the recommendation.
9. The system records the decision, user, timestamp, and optional annotation.
10. The system confirms that the analysis and decision were saved.

### Alternate Flow

**AF-1: Unsupported or Invalid Plan**

- At Step 4, if the plan cannot be parsed or is unsupported, the system marks the analysis as failed and displays a validation error.
- The system retains the original query record and records an audit event.
- The Database Administrator may upload a corrected plan or exit without creating a recommendation.
