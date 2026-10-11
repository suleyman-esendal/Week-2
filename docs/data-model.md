# Data Model Worksheet — Production & Quality Operations App

Planning sheet completed before creating any objects in Salesforce.
Relationship types (lookup vs. master-detail) are finalized in Step 4.

- **Primary party/source record:** Plant
- **Main process record:** Production Order

| Entity | Purpose | Owner | Record Name | Lifecycle | Sensitive Fields | Parent | Expected Volume |
|---|---|---|---|---|---|---|---|
| Plant | Primary party/source record: a physical manufacturing site | Plant Manager | Plant name/code (text) | Active → Inactive | Address, cost-center code | None (top level) | 1–10 |
| Work Center | A production line or station inside a plant | Line Supervisor | Work Center Name (text) | Active → Under Maintenance → Retired | Capacity and rate data | Plant | 10–50 per plant |
| Production Order | Main process record: what to build, how much, by when | Production Planner | Auto-number PO-{00000} | Planned → Scheduled → In Progress → Completed (or Cancelled) | Priority, cost information | Plant | Hundreds per month |
| Material Requirement | A material needed to fulfill one order | Production Planner | Auto-number MR-{00000} | Required → Available or Short → Consumed | Supplier pricing | Production Order | 5–20 per order |
| Production Run | Actual execution of an order at a work center | Line Supervisor | Auto-number RUN-{00000} | Started → Paused → Completed | Operator performance data | Production Order | 1–3 per order |
| Quality Inspection | Pass/fail result for completed run output | Quality Inspector | Auto-number QI-{00000} | Pending → Passed or Failed | Defect and failure-reason notes | Production Run | 1 per run |
| Downtime Event | A stoppage during a run, with cause and duration | Line Supervisor | Auto-number DT-{00000} | Open → Closed (escalated if critical) | Root-cause notes | Production Run | 0–5 per run |
| Finished Good | Passed output released for inventory/shipment | Quality Inspector | Auto-number FG-{00000} | Released → Shipped | Cost and margin | Production Order | 1 per completed order |

## Relationships and Deletion Decisions

| Child | Parent | Type | Required | When parent is deleted | Why |
|---|---|---|---|---|---|
| Work Center | Plant | Master-Detail | Yes | Work centers are deleted too | A work center is meaningless without its plant. Inherits plant visibility, which supports plant-level isolation (US-10). |
| Production Order | Plant | Lookup | Yes | Deletion blocked | Order history must survive. The order keeps its own owner (Production Planner). |
| Material Requirement | Production Order | Master-Detail | Yes | Deleted with the order | A requirement has no meaning without its order. Allows roll-up of shortages. |
| Production Run | Production Order | Master-Detail | Yes | Deleted with the order | A run belongs to one order. Completed quantity can roll up from runs. |
| Production Run | Work Center | Lookup | Yes | Deletion blocked | Run history must survive. Master-Detail here would create a 4-level chain, which Salesforce does not allow. |
| Downtime Event | Production Run | Master-Detail | Yes | Deleted with the run | Total downtime must roll up onto the run (US-06). |
| Quality Inspection | Production Run | Lookup | Yes | Deletion blocked | Quality decisions need independent ownership and visibility. Reduces pressure to pass borderline output. |
| Finished Good | Production Order | Lookup | Yes | Deletion blocked | Released goods must outlive the order record. |

**Junction objects:** none needed. Production Run already connects Production Order and Work Center.
**Limits checked:** no object has more than 2 master-detail relationships. The deepest master-detail chain is Production Order, Production Run, Downtime Event (3 levels).