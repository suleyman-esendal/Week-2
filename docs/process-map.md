markdown
# Process Map — Production & Quality Operations App

## Flow Diagram

[1. Production Planning]
↓
[2. Material Readiness]
↓
[3. Work-Center Scheduling]
↓
[4. Execution] ──(stoppage occurs)──→ [6. Downtime Response] ──(resume)──┐
↓ │
│ ←───────────────────────────────────────────────────────────────┘
↓
[5. Quality Inspection]
↓
[7. Finished-Goods Release]


## Phase Detail

### 1. Production Planning
- **Persona Responsible:** Production Planner
- **Input:** Demand/order requirement
- **Output:** Production Order created
- **Decision:** What to produce, how much, and by when
- **Possible Failure:** Inaccurate demand input leads to over- or under-production
- **Markers:** —

### 2. Material Readiness
- **Persona Responsible:** Production Planner
- **Input:** Production Order, Material Requirement list, current stock levels
- **Output:** Material availability confirmed (or shortage flagged)
- **Decision:** Proceed to scheduling, or hold for material shortage
- **Possible Failure:** Stock data is stale, so the system shows "ready" when it isn't
- **Markers:** **Integration** (depends on inventory/stock data, likely external in a full system)

### 3. Work-Center Scheduling
- **Persona Responsible:** Production Planner
- **Input:** Production Order (materials confirmed), Work Center capacity
- **Output:** Order scheduled to a specific Work Center and time slot
- **Decision:** Which Work Center and time slot to assign
- **Possible Failure:** Double-booking a Work Center due to outdated capacity data
- **Markers:** **Scheduled work**

### 4. Execution
- **Persona Responsible:** Line Supervisor
- **Input:** Scheduled Production Order
- **Output:** Production Run record with actual output
- **Decision:** When to start, pause, or complete the run
- **Possible Failure:** Run started without materials physically on hand, despite system status
- **Markers:** —

### 5. Quality Inspection
- **Persona Responsible:** Quality Inspector
- **Input:** Completed Production Run output
- **Output:** Quality Inspection record (pass/fail)
- **Decision:** Pass or fail the output
- **Possible Failure:** Borderline output passed under production-pressure without clear justification
- **Markers:** **Approval required** (gates release)

### 6. Downtime Response
- **Persona Responsible:** Line Supervisor
- **Input:** A stoppage occurring during Execution
- **Output:** Downtime Event record (cause, duration)
- **Decision:** Categorize the cause; decide whether to escalate
- **Possible Failure:** Cause miscategorized or logged late, skewing root-cause reporting
- **Markers:** **Human escalation** (for extended or critical downtime)

### 7. Finished-Goods Release
- **Persona Responsible:** Quality Inspector / Plant Manager
- **Input:** Output that passed Quality Inspection
- **Output:** Finished Good record, ready for release
- **Decision:** Release to inventory/shipment
- **Possible Failure:** Output released without a recorded inspection due to a process gap
- **Markers:** **Approval required**