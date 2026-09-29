# User Story Backlog — Production & Quality Operations App

## MVP (First Usable Release)
The following 4 stories represent the minimum needed for a usable
end-to-end flow — one story per critical phase, enough to take a
production order from creation to a quality-checked outcome:
- US-01 (Create Production Order)
- US-03 (Schedule to Work Center)
- US-05 (Log Production Run)
- US-07 (Record Quality Inspection)

---

### US-01 — Create Production Order
**As a** Production Planner, **I want** to create a Production Order, **so that** the team knows what to build and how much.
- **Priority:** Must
- **Given** a valid product and quantity, **When** I submit a new Production Order, **Then** it is saved with status "Planned".
- **Given** a Production Order with a missing quantity, **When** I try to submit it, **Then** the system blocks submission with a validation error.
- **Given** a saved Production Order, **When** I view it, **Then** I see product, quantity, and requested date.

### US-02 — Check Material Requirement Availability
**As a** Production Planner, **I want** to see whether required materials are in stock, **so that** I don't schedule an order I can't fulfill.
- **Priority:** Must
- **Given** a Production Order with linked Material Requirements, **When** I open the order, **Then** I see a stock-availability status for each material.
- **Given** insufficient stock for a required material, **When** I view the order, **Then** a shortage warning is displayed.
- **Given** all materials are available, **When** I view the order, **Then** it shows "Ready to Schedule".

### US-03 — Schedule Production Order to a Work Center
**As a** Production Planner, **I want** to assign a Production Order to a Work Center and time slot, **so that** execution can begin at the right place and time.
- **Priority:** Must
- **Given** a Work Center with open capacity, **When** I schedule the order to it, **Then** the order status updates to "Scheduled".
- **Given** a Work Center already at capacity for that slot, **When** I try to schedule into it, **Then** the system warns of a conflict.
- **Given** a scheduled order, **When** I view the Work Center calendar, **Then** the order appears in the correct slot.

### US-04 — View Work Center Capacity
**As a** Production Planner, **I want** to see current and upcoming Work Center load, **so that** I can schedule orders without overbooking.
- **Priority:** Should
- **Given** existing scheduled orders, **When** I open a Work Center's schedule, **Then** I see all committed time slots.
- **Given** a fully booked slot, **When** I view the calendar, **Then** it is visually marked as unavailable.
- **Given** an empty slot, **When** I view the calendar, **Then** it is marked as open.

### US-05 — Log a Production Run
**As a** Line Supervisor, **I want** to start and record a Production Run, **so that** actual execution is tracked against the plan.
- **Priority:** Must
- **Given** a scheduled Production Order, **When** I start a Production Run, **Then** its status changes to "In Progress" with a start timestamp.
- **Given** an active Production Run, **When** I mark it complete, **Then** actual output quantity is recorded.
- **Given** a completed run, **When** I view the Production Order, **Then** planned vs. actual quantity is visible.

### US-06 — Log a Downtime Event
**As a** Line Supervisor, **I want** to record a stoppage with its cause and duration, **so that** downtime is tracked and reportable.
- **Priority:** Must
- **Given** an active Production Run experiencing a stoppage, **When** I log a Downtime Event, **Then** it's saved with cause, start time, and linked run.
- **Given** an open Downtime Event, **When** the line resumes, **Then** I can close it with an end time and duration is calculated automatically.
- **Given** a closed Downtime Event, **When** I view the Production Run, **Then** total downtime is reflected in the run summary.

### US-07 — Record a Quality Inspection
**As a** Quality Inspector, **I want** to record a pass/fail