# Data Model Worksheet — Production & Quality Operations App

Planning sheet completed before creating any objects in Salesforce.
Relationship types (lookup vs. master-detail) are finalized in Step 4.

- **Primary party/source record:** Plant
- **Main process record:** Production Order

| Entity | Purpose | Owner | Record Name | Lifecycle | Sensitive Fields | Parent | Expected Volume |
|---|---|---|---|---|---|---|---|
| Plant | Primary party/source record: a physical manufacturing site | Plant Manager | Plant name/code (text) | Active → Inactive | Address, cost-center code | None (top level) | 1–10 |
| Work Center | A production line or station inside a plant | Line Supervisor | Work Center Name (text) | Active → Under Maintenance → Retired | Capacity and rate data | Plant | 10–50 per plant |
| Production Order | Main process record: what to build, how much, by when | Production Planner | Auto-number PO-{00000} | Production Planning → Material Readiness → Work-Center Scheduling → Execution → Quality Inspection → Downtime Response → Finished-Goods Release | Priority, cost information | Plant | Hundreds per month |
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

Junction objects: none needed. Production Run already connects Production Order and Work Center, so a separate junction would duplicate that information.

**Limits checked:** no object has more than 2 master-detail relationships. The deepest master-detail chain is Production Order, Production Run, Downtime Event (3 levels).

## Calculated and Summary Fields

| Object | Field | Type | Purpose |
|---|---|---|---|
| Production Order | Completion Percent | Formula (Percent) | Progress against planned quantity |
| Production Order | Schedule Risk | Formula (Text) | Flags orders with unconfirmed materials near their start date |
| Downtime Event | Downtime Severity | Formula (Text) | Critical at 30+ minutes (supports US-08 escalation) |
| Production Run | Total Downtime Minutes | Roll-Up (SUM) | Total stoppage time per run (US-06) |
| Production Order | Material Requirement Count | Roll-Up (COUNT) | Number of materials per order |
| Production Order | Run Count | Roll-Up (COUNT) | Number of runs per order |

**Roll-up not available:** Quality Inspection is a lookup child of Production Run, so a failed-inspection count on the run cannot be a roll-up summary. Later alternative: a record-triggered Flow (or Apex trigger) that counts failed inspections and writes the result to a number field on the run.

## Status Values and Lifecycle Note

Production Order Status follows the process phases from the Week 3 process map: Production Planning, Material Readiness, Work-Center Scheduling, Execution, Quality Inspection, Downtime Response, Finished-Goods Release. The picklist is restricted, so only these values are accepted. This replaces the earlier Planned/Scheduled/In Progress wording in the backlog. Lifecycle values for the other entities are planning guidance; only Quality Inspection (Pending, Passed, Failed) is enforced by a picklist.

## Sample Data

Loaded with `sf data import tree --plan data/sample/plan.json` (files in `data/sample/`).

| Object | Records | Notes |
|---|---|---|
| Plant | 5 | Pilot Line Plant is the incomplete record: no External Id and no work centers |
| Work Center | 6 | Spread across four plants |
| Production Order | 12 | All 7 statuses covered. PO-00002 is incomplete (only plant and status, so Schedule Risk shows "No Start Date"). PO-00012 is unusual but valid (1,050 completed against 1,000 planned, 105%) |
| Material Requirement | 8 | Two each on four orders |
| Production Run | 7 | Each links an order and a work center |
| Downtime Event | 5 | RUN-00004 has a 480-minute event (full shift) |
| Quality Inspection | 5 | Pending, Passed and Failed represented |
| Finished Good | 3 | Linked to the released orders |

Verified with SOQL: Total Downtime Minutes on runs is 30, 8, 480 and 57 as expected, and related lists show the expected children (for example PO-00007 shows 2 runs and 2 material requirements).

## Schema Review and Source Control

- Schema Builder shows 8 objects, 8 relationships, no duplicates and no isolated objects. Master-Detail and Lookup lines match the table above.
- Metadata for all 8 objects, fields, tabs, list views, the compact layout and the application was retrieved into `week2-sfdx/force-app`. API names and field types were checked in the XML (restricted Status picklist, Master-Detail from Production Run to Production Order).
- No validation org was assigned for this exercise, so the deployment test step was skipped.