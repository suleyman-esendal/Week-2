# Personas

## Production Planner

**Goals:** Ensure production orders are created accurately and scheduled
so material and capacity are ready before work begins.

**Daily Tasks:** Reviews demand, creates Production Orders, checks
Material Requirements against available stock, schedules orders to Work
Centers.

**Records Created:** Production Order, Material Requirement.

**Records Reviewed:** Stock/inventory levels, Work Center capacity,
existing Production Orders.

**Decisions Made:** What to produce, how much, and which Work Center and
timeframe to schedule it in.

**Must Not See:** Other plants' confidential cost/margin data not
relevant to their own plant.

**Frustration/Risk:** Scheduling an order against a Work Center that
looks available but isn't, due to stale or delayed status updates —
leading to a bottleneck discovered too late.

**Success Outcome:** 95%+ of Production Orders are scheduled with
material fully available at start of the scheduled Work Center slot, with
no last-minute rescheduling.

## Line Supervisor

**Goals:** Keep the assigned Work Center running efficiently and respond
quickly when something goes wrong.

**Daily Tasks:** Starts and monitors Production Runs, logs Downtime
Events when the line stops, coordinates with operators, escalates
material shortages.

**Records Created:** Production Run, Downtime Event.

**Records Reviewed:** Production Order details, Material Requirement
status, prior Downtime Events for their Work Center.

**Decisions Made:** When to start/pause a run, how to categorize a
downtime cause, whether to escalate an issue.

**Must Not See:** Other plants' Production Orders and performance data
outside their assigned Work Center/Plant.

**Frustration/Risk:** Logging a Downtime Event takes too long during an
active stoppage, so the real cause gets recorded inaccurately or late,
which then skews root-cause reporting.

**Success Outcome:** Average Downtime Event logging time under 2 minutes
from stoppage start, with cause code accuracy verified in weekly review.

## Quality Inspector

**Goals:** Catch defects before they reach the customer, without slowing
down the release of good product.

**Daily Tasks:** Reviews completed Production Runs, performs Quality
Inspections, records pass/fail results, flags root causes for failures.

**Records Created:** Quality Inspection.

**Records Reviewed:** Production Run output, Production Order
specifications, prior inspection history for the same Work Center or
product.

**Decisions Made:** Whether output passes inspection and can be released
as a Finished Good, or must be reworked/scrapped.

**Must Not See:** Production cost or margin data — inspection decisions
should be based on quality criteria only, not cost pressure.

**Frustration/Risk:** Pressure (real or perceived) to pass borderline
output to keep production numbers looking good, without clear
documentation to justify a fail decision.

**Success Outcome:** 100% of Production Runs have a recorded Quality
Inspection result before any Finished Good is released, with zero
inspections skipped.

## Plant Manager

**Goals:** Maintain overall plant throughput, quality, and uptime, and
make informed calls when trade-offs are needed.

**Daily Tasks:** Reviews plant-wide production and quality dashboards,
resolves escalated downtime or material issues, approves scope/priority
changes.

**Records Created:** Rarely creates operational records directly; may
approve/adjust Production Order priority.

**Records Reviewed:** All Production Orders, Production Runs, Quality
Inspections, and Downtime Events across their plant.

**Decisions Made:** Priority trade-offs across orders, staffing/resource
allocation, when to escalate a recurring issue beyond the plant level.

**Must Not See:** Not applicable at the plant level — but should not see
other plants' data outside their scope unless explicitly granted
cross-plant visibility.

**Frustration/Risk:** Getting an accurate, real-time view of plant health
is hard if data entry (downtime causes, inspection results) is
inconsistent or delayed at the source.

**Success Outcome:** Plant-wide dashboard reflects same-day data with no
more than a 1-hour lag, enabling same-day escalation decisions.