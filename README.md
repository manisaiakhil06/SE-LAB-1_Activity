# SE Lab 1: Requirements Engineering & UML Use-Case Modelling

**Course:** Software Engineering Lab (UE24CS341A)  
**Institution:** PES University - Department of CSE  
**Student:** Nallamalli Kanaka Mani Sai Akhil  
**USN:** PES1UG24CS290  
**Problem Statement #44:** Database Query Performance Profiler  

---

## System Overview

The Database Query Performance Profiler is a database observability tool that ingests slow-query logs, parses PostgreSQL and MySQL execution plans, identifies possible missing indexes, and generates weekly performance-optimization digests.

**Primary stakeholders:**

- Database Administrator
- Backend Lead

---

## Deliverables

- [Problem Statement](docs/Problem_Statement.pdf)
- [Requirements Specification](docs/Requirements_Specification.md)
- [Requirements Table](docs/Requirements_Table.docx)
- [UML Use-Case Diagram](diagrams/UML_Use_Case_Diagram.pdf)
- [UML Diagram Source](diagrams/use_case_diagram.puml)
- [Use-Case Flow Specification](docs/Use_Case_Specification.md)
- [Use-Case Flow Document](docs/Use_Case_Flow.docx)
- [Use-Case Flow PDF](docs/Use_Case_Flow.pdf)

---

## Functional Requirements

| ID | Description | Priority |
|---|---|---|
| FR-001 | The system shall parse PostgreSQL and MySQL EXPLAIN plans, identify sequential scans on large tables, and recommend index column definitions. | High |
| FR-002 | The system shall ingest slow-query logs from configured PostgreSQL and MySQL sources. | High |
| FR-003 | The system shall rank slow-query fingerprints by cumulative execution time, average execution time, and execution count. | High |
| FR-004 | The system shall allow a Database Administrator to accept, dismiss, or annotate generated index recommendations. | Medium |
| FR-005 | The system shall generate a weekly optimization digest with query, plan-issue, and recommendation information. | Medium |

---

## Non-Functional Requirements

| ID | Type | Description | Priority |
|---|---|---|---|
| NFR-001 | Performance & Security | The system shall ingest up to 5,000 log records per minute without data loss or significant host database degradation. | High |
| NFR-002 | Security | The system shall encrypt stored query logs and enforce role-based access control for profiler data and actions. | High |

---

## UML Use-Case Model

**Actors:**

- Database Administrator
- Backend Lead
- DBMS / Log Source

**Core use cases:**

1. Ingest Slow Query Logs  
2. Rank Slow Queries  
3. Analyse Query Execution Plan  
4. Generate Index Recommendation  
5. Review / Decide Recommendation  
6. Generate Weekly Optimization Digest  

The model includes:

- `<<include>>` relationship: Analyse Query Execution Plan includes Generate Index Recommendation.
- `<<extend>>` relationship: Review / Decide Recommendation extends Generate Index Recommendation.

---

## Core Use Case

### UC-01: Analyse Query Execution Plan

**Primary Actor:** Database Administrator

**Preconditions:**

- The Database Administrator is authenticated.
- A slow-query record and a valid PostgreSQL or MySQL EXPLAIN plan are available.

**Postconditions:**

- The analysis result, detected issues, and generated recommendation are saved in the query profile.

**Alternate Flow:**

If the execution plan is invalid or unsupported, the system displays the validation error, records an audit event, and allows the Database Administrator to upload a corrected plan or exit without creating a recommendation.
