# Architecture Baseline — Notes (v1)

## Entity Classification
All 8 planned entities are **custom object candidates** — none have a
close standard Salesforce object match for this domain:
- Plant, Work Center, Production Order, Material Requirement,
  Production Run, Quality Inspection, Downtime Event, Finished Good

One open question worth revisiting in Week 4: whether **Production
Order** should instead extend the standard **WorkOrder** object
(Field Service), and whether **Finished Good** could relate to the
standard **Product2** object as its catalog entry. Deferred until data
modeling in Week 4.

## Assumptions
1. A single Salesforce org can support all Plants in scope for this
   phase — no need for a multi-org strategy yet.
2. Inventory/stock data will remain **manually entered** in this phase;
   the external inventory/ERP integration shown as "Future Integration"
   is out of scope until Weeks 11-13.
3. All four personas (Production Planner, Line Supervisor, Quality
   Inspector, Plant Manager) will use the standard Salesforce Lightning
   UI on desktop/tablet — no dedicated mobile app is assumed for this
   phase.

## Risks
1. **Data staleness risk:** Since stock/material data is manual in this
   phase (see Assumption 2), Material Requirement availability could
   show as "ready" when it isn't — directly called out as a possible
   failure in the process map (Phase 2).
2. **Cross-plant data exposure risk:** As Plants scale, sharing rules
   must correctly restrict Plant Manager visibility to their own plant
   only (already reflected as a security acceptance criterion on
   US-10). Misconfigured sharing here would expose confidential
   cross-plant performance data.
3. **Scope creep risk:** The Lightning App shell currently uses only
   standard navigation items (Home, Tasks, Reports, Dashboards, Files).
   Once custom objects exist, there's a risk of over-building
   navigation/UI before the MVP (US-01, US-03, US-05, US-07) is
   functionally complete end to end.