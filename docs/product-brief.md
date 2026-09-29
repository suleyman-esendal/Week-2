# Product Brief: Production & Quality Operations App

## What This Is
A Salesforce application for a manufacturing operation to plan, execute,
and track production while ensuring quality and visibility across the
shop floor. It gives production planners, work-center operators, and
quality inspectors a shared system for managing production orders from
planning through finished-goods release.

## Target Process
The application supports the full manufacturing lifecycle:

1. **Production Planning** — a Production Order is created, defining what
   to build and how much.
2. **Material Readiness** — Material Requirements are checked against
   available stock before a run can start.
3. **Work-Center Scheduling** — the order is scheduled onto a specific
   Work Center within a Plant.
4. **Execution** — a Production Run captures the actual work being
   performed at the work center.
5. **Quality Inspection** — output from the run goes through a Quality
   Inspection before being accepted.
6. **Downtime Response** — any stoppage during execution is logged as a
   Downtime Event, capturing cause and duration.
7. **Finished-Goods Release** — inspected, passing output becomes a
   Finished Good, ready for release.

## In Scope
- Core objects: Plant, Work Center, Production Order, Material
  Requirement, Production Run, Quality Inspection, Downtime Event,
  Finished Good
- Tracking a production order from creation through finished-goods
  release
- Recording quality inspection outcomes (pass/fail) against production
  output
- Logging downtime events tied to a work center and production run
- Role-based visibility for production planners, operators, and quality
  inspectors

## Out of Scope
- Integration with external ERP or MES systems (may be addressed in a
  later phase — Weeks 11-13 cover external API integration)
- Financial/costing calculations tied to production
- Supplier and procurement management
- Predictive maintenance or IoT sensor data
- Mobile-specific UI (assumed desktop/tablet use on the shop floor for
  now)