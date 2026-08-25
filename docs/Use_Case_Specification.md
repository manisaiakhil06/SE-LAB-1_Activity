# Use-Case Flow Specification

## UC-01: Analyse Query Execution Plan

**Primary Actor:** Database Administrator  
**Supporting Actor:** Backend Lead  
**Trigger:** The Database Administrator selects an ingested slow query and requests plan analysis.

### Preconditions

- The Database Administrator is authenticated.
- A slow-query record and valid PostgreSQL or MySQL EXPLAIN plan are available.

### Postconditions

- The analysis result, detected issues, and any recommendation are stored in the query profile.

### Main Success Scenario

1. The Database Administrator opens a slow-query record.
2. The system displays normalized query details and the execution plan.
3. The Database Administrator selects **Analyse Plan**.
4. The system validates and parses plan operators, predicates, estimated rows, and costs.
5. The system identifies sequential scans and evaluates filtered or join columns for a missing suitable index.
6. The system calculates recommendation confidence and prepares a proposed index definition.
7. The system displays the evidence, proposed index, and confidence level.
8. The Database Administrator selects **Accept**, **Dismiss**, or **Annotate**.
9. The system records the decision, user, timestamp, and optional annotation.
10. The system confirms that the analysis and decision were saved.

### Alternate Flow A1: Unsupported or Invalid Plan

1. At step 4, if the plan cannot be parsed or is unsupported, the system marks analysis as failed and displays the validation error.
2. The system retains the original query and writes an audit event.
3. The Database Administrator may upload a corrected plan or exit without creating a recommendation.
