# Department: Work Intake

## Department Overview
The Work Intake department is the primary entry point for all work entering the organization. This group evaluates incoming requests to determine if they need clarification, defines scope and priority, and assesses whether appropriate skills are currently allocated and available. Work Intake ensures that tasks and projects are well-understood, properly scoped, and routed to the right resources before execution begins — reducing rework, misallocation, and ambiguity downstream.

While Work Intake is the primary path for all work, direct bypasses to other departments are permitted. Departments may receive work directly when the scope is clear, the requesting party has an established relationship with the receiving department, and routing through Work Intake would add unnecessary overhead. However, bypassed work that turns out to be under-scoped or misrouted may be redirected back through Work Intake.

## Why This Department Exists
Without a dedicated intake function, work enters the organization through ad-hoc channels. Requests arrive with unclear scope, competing priorities, and no assessment of whether the organization currently has the capacity or capability to handle them. Humans must intervene to interpret, prioritize, and route — or agents receive ambiguous instructions and produce misaligned outputs. The Work Intake department centralizes these functions, ensuring that every piece of work is evaluated, scoped, prioritized, and matched to available resources before it consumes organizational capacity.

## Department Responsibilities

### Request Evaluation and Clarification
- Receive work requests from all sources (humans, other departments, internal processes)
- Assess whether requests have sufficient information to proceed
- Request clarification when scope is ambiguous or information is missing
- Validate that the stated objective is achievable and well-defined

### Scope Definition
- Develop vague or loosely defined requests into structured, actionable plans
- Define what is in scope and out of scope for each piece of work
- Identify deliverables, dependencies, and required roles
- Determine the appropriate level of detail for each plan

### Priority Assessment
- Apply a transparent prioritization framework to all incoming work
- Balance urgency, impact, dependencies, and capacity constraints
- Make priority decisions visible so stakeholders understand ordering rationale
- Reprioritize dynamically as new work arrives or circumstances change

### Skill and Capacity Matching
- Assess whether the organization currently has the roles and capacity to handle incoming work
- Identify capability gaps when work requires skills not represented in the current organization
- Route work to the appropriate agents or departments based on role match and availability
- Provide early warning when demand is likely to exceed organizational capacity

### Human Work Triage
- Help individual humans evaluate their personal task loads
- Recommend which tasks humans should delegate, defer, or discard
- Maintain personalized context for each human served to improve triage accuracy over time

## Assigned Roles

### Core Department Roles
| Role | Primary Function | Notes |
|---|---|---|
| **Intake & Prioritization Manager** | Manages the department's work queue — triages, prioritizes, and dispatches work to appropriate agents | System-facing; owns the queue and routing logic |
| **Scope & Decision Delegate** | Makes routine scope and filtering decisions autonomously, escalating only when genuinely ambiguous or high-stakes | Reduces mandatory human decision points |
| **Work Triage Advisor** | Helps humans evaluate their personal tasks using a Delegate/Delete/Defer model | Human-facing; serves individual humans |
| **Work Scope Developer** | Develops brief task descriptions into structured plans with identified roles and dependencies | Scoping function; transforms vague inputs into actionable plans |

### Associated Roles (Functional Assignment)
| Role | Primary Function | Notes |
|---|---|---|
| **Inter-Department Liaison** | Manages cross-department communication, request routing, and relationship management | Organizationally assigned to Work Intake for approvals and structural alignment, but functionally operates across all departments. May spend the majority of operational time outside the department. |

## Internal Workflow

### Standard Intake Flow
```
Request Received
      │
      ▼
  Is the request clear and complete?
      │
   Yes │           No
      │            │
      ▼            ▼
  Prioritize    Return to source
      │         with specific questions
      ▼            │
  Is scope         │
  defined?         │
      │            │
   Yes │     No    │
      │      │     │
      ▼      ▼     │
  Route to   Work Scope Developer
  execution  develops plan
      │            │
      ▼            ▼
  Intake &    Plan returned to
  Prioritization  requester for
  Manager         approval
  dispatches      │
                  ▼
              Approved → Route to execution
              Feedback → Iterate plan
```

### Role Interactions Within Department
- **Work Triage Advisor → Intake & Prioritization Manager**: Delegated work items flow from triage to the queue for routing
- **Work Triage Advisor → Work Scope Developer**: Ambiguous tasks are sent for scope clarification before triage can complete
- **Work Scope Developer → Intake & Prioritization Manager**: Finalized plans are handed off for execution routing
- **Scope & Decision Delegate → Intake & Prioritization Manager**: Scope decisions inform dispatch decisions
- **Inter-Department Liaison → Intake & Prioritization Manager**: Cross-department requests enter the queue through the Liaison
- **Inter-Department Liaison → Work Scope Developer**: Cross-department work may need scoping before it can be routed

## Department Interfaces

### Inbound
- **From Humans**: Direct work requests, task submissions for triage, project ideas for scoping
- **From Other Departments**: Cross-department requests routed through the Inter-Department Liaison
- **From Internal Processes**: Follow-up tasks, recurring work, process-generated items

### Outbound
- **To All Departments**: Scoped, prioritized work packages ready for execution
- **To Humans**: Triage recommendations, Delete candidate batches, scope clarification requests, plan drafts for approval
- **To Workflow Architect**: Role gap proposals when plans identify missing capabilities
- **To Strategy, Architecture, and Oversight**: Patterns in incoming work that suggest structural or strategic considerations

## Department Metrics

### Throughput
- Work items received, scoped, and dispatched per period
- Average time from request receipt to dispatch (intake-to-execution time)
- Queue depth and aging trends

### Quality
- Percentage of dispatched work that executes without scope-related rework
- Percentage of plans accepted by requesters without major revision
- Dispatch accuracy (work correctly assigned to the right role/department on first attempt)

### Capacity Management
- Percentage of incoming work matched to available capacity
- Frequency and lead time of capacity constraint early warnings
- Gap detection rate (capability gaps identified during intake that would have caused execution failures)

---

**Work Intake is the organization's front door. By centralizing evaluation, scoping, prioritization, and routing, this department ensures that every piece of work is well-understood and properly directed before resources are committed — making the entire organization more efficient, responsive, and intentional about how it spends its capacity.**
