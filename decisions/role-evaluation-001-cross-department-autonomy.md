# Role Evaluation: Cross-Department Autonomy Architecture

**Document Type**: Architectural Decision Record (Role Evaluation)
**Date**: 2026-02-14
**Status**: Accepted
**Participants**: Human (department owner), AI collaborator

---

## Context

The repository defines a Product Evaluation department composed of four agent roles: Product Data Collector, Product Quality Evaluator, Product Purchase Advisor, and Process Efficiency Coordinator. These roles are supported by a workflow document defining handoffs, escalation paths, human decision loops, and data formats.

The department owner requested:
1. Elevate the Process Efficiency Coordinator to a cross-department role that interviews agents and improves work descriptions across department boundaries
2. Recommend three roles that would help the department work more autonomously with other agent departments, reducing human input and decision-making
3. Propose a Presenter role that transforms AI-structured data (JSON, structured tables) into human-consumable formats (executive summaries, product matrices, decision trees)

This evaluation documents the collaborative discussion that refined these roles, tested the architecture through analogy, identified a structural gap, and resulted in the addition of a sixth role (Workflow Architect) to address that gap.

---

## Decisions Made

### Decision 1: Process Efficiency Coordinator Elevated to Cross-Department Role

**What changed**: The Process Efficiency Coordinator was rewritten from a department-scoped optimization role to a cross-department shared service with authority to interview agents in any department, review and improve job descriptions across departments, and standardize cross-department communication protocols.

**Rationale**: As additional departments are added, the biggest friction points will be at department boundaries — incompatible formats, lost information in handoffs, and divergent terminology. A coordinator embedded in one department cannot see or fix these cross-boundary issues. The role must operate across all departments to have visibility into systemic inefficiencies.

**Key addition**: A new primary responsibility — "Job Description and Process Document Improvement" — reflecting the explicit mandate to keep role documentation accurate and actionable as the organization evolves.

### Decision 2: Three Autonomy-Enabling Roles Recommended and Accepted

#### Inter-Department Liaison

**Proposed as**: The department's external-facing representative, managing all cross-department communication without human mediation.

**Evaluated as**: *Logistics coordinator (decider) AND relationship manager (record-keeper), with work-domain translation.*

**Assessment**: The logistics analogy captures the routing and dispatching function well. The "relationship manager" dimension adds the stateful aspect — this role doesn't just ship packages, it maintains the bilateral agreements, terminology glossaries, and contact protocols that make future shipments smoother. The domain translation function is critical: different departments will use different terminology for overlapping concepts, and the Liaison must bridge that gap without losing meaning.

**Accepted with this understanding**: The Liaison is not a passive router. It is an active translator and relationship steward with decision authority over routing, format conversion, and minor timeline negotiation.

#### Autonomous Decision Delegate

**Proposed as**: An autonomous decision-maker at workflow gate points, using precedent libraries, decision frameworks, and confidence thresholds to resolve routine decisions without human input.

**Evaluated as**: *Automated decision-maker based on precedent and confidence of evaluated data.*

**Assessment**: This accurately describes the core mechanic. However, the evaluation immediately surfaced a critical question: **Who oversees when this role gets it wrong or misses something?**

The original design relied on:
1. Self-calibration (the Delegate tracks its own accuracy)
2. Post-hoc human review (users catch errors at the final decision point)
3. Process Efficiency Coordinator review (periodic audit of calibration data)

These mechanisms are insufficient for several failure modes:
- **Silent scope exclusion**: Confidently filtering out something the user would have wanted, with no signal that it happened
- **Confidence miscalibration**: The Delegate doesn't know what it doesn't know
- **Domain boundary crossings**: Requests touching safety, legal, or compliance that the Delegate has no framework to recognize

**Resolution**: The Autonomous Decision Delegate's job description was updated with:
- An explicit "Oversight and Structural Limitations" section
- Documentation that the current scope is safe for common product-evaluation use cases but **not suitable for high-stakes domains** (emergency services, critical hardware, medical devices, legal/compliance-sensitive evaluations)
- A mandatory escalation categories list that cannot be overridden by confidence level: safety-implicated, legal/regulatory, high-dollar, irreversible, and first-of-kind
- Recommended future oversight mechanisms (audit sampling, peer review gates, outcome tracking, cross-department oversight) documented for evaluation as the organization scales

This structural gap directly led to Decision 4 (Workflow Architect role).

#### Intake & Prioritization Manager

**Proposed as**: Centralized intake, triage, prioritization, and dispatch for all work entering the department.

**Evaluated as**: *Floor manager + forecasting and scheduling. A highly skilled floor manager with allocated trust and responsibility.*

**Assessment**: The floor manager analogy is the closest of the three. The role coordinates people and work based on company needs, with the added dimension of capacity forecasting — tracking throughput trends and providing early warning when demand will exceed capacity. This is more akin to a production scheduler in manufacturing than a simple dispatcher.

**Accepted with this understanding**: The Intake & Prioritization Manager is a trusted operational leader, not an administrative clerk. It has the authority to make priority decisions, redistribute work, and escalate capacity constraints proactively.

### Decision 3: Presenter Role Accepted

**Proposed as**: A translation layer that transforms structured agent outputs (JSON, data matrices, evaluation logs) into human-consumable formats designed for rapid comprehension and decision-making.

**Rationale**: Agent departments produce outputs optimized for machine processing. Humans need executive summaries, comparison tables, and decision trees. Without a dedicated role, either agents degrade their analytical work to accommodate formatting, or humans must interpret raw structured data. The Presenter specializes in this translation.

**Format library defined**: Executive Summary, Product Comparison Matrix, Decision Tree Chart, Risk/Benefit Table, Recommendation Brief, Trend Analysis Report, Scorecard Dashboard, Cost Breakdown Waterfall.

**Key design principles**: Progressive disclosure (summary-first with drill-down), data integrity rules (never invent data, never upgrade confidence, never hide uncertainty), and audience adaptation (executive, technical, end-user, cross-department).

### Decision 4: Workflow Architect Role Added

**Origin**: This role was not in the original request. It emerged from the evaluation discussion when the Autonomous Decision Delegate's oversight gap was identified.

**The question that created this role**: "Who oversees if this role missed things that should have gone to someone else (agent with scope, agent executive, safety/legal coordinator, or human executive) for consideration and input?"

**Rationale**: The Process Efficiency Coordinator optimizes *how well* workflows run. No role was responsible for evaluating *whether the workflow design itself is sound*. This is the difference between operational efficiency and structural integrity. The Autonomous Decision Delegate oversight gap was a structural problem — a missing feedback loop in the architecture — not a process efficiency problem.

As the organization scales to multiple departments with different risk profiles, structural evaluation becomes critical:
- A workflow that's structurally sound for consumer product evaluation may be dangerously unsuitable for a department handling safety-critical procurement
- Departments designed independently may interact in ways that create failure modes neither anticipated
- Competing department goals may create perverse incentives that no single department can see or resolve

**The Workflow Architect's core responsibilities**:
- Evaluate workflows for structural gaps, missing roles, and unhandled failure modes
- Conduct failure mode analysis (silent failures, cascade failures, authority gaps, boundary failures)
- Assess department risk profiles and ensure workflow structure matches risk level
- Evaluate cross-department interaction architecture, including competitive dynamics
- Design oversight mechanisms calibrated to each department's risk
- Work with the Process Efficiency Coordinator to translate structural findings into implementable changes

**Relationship to Process Efficiency Coordinator**: These two roles are the closest collaborators in the organization but have distinct mandates. The PEC asks "Is this process running well?" The WA asks "Is this process designed correctly?" Findings flow from WA to PEC for implementation.

---

## Architecture Summary After Decisions

### Role Categories

**Core Product Roles** (do the department's primary work):
- Product Data Collector
- Product Quality Evaluator
- Product Purchase Advisor

**Department Operations Roles** (enable autonomous operation):
- Intake & Prioritization Manager — floor manager + forecasting
- Autonomous Decision Delegate — autonomous decision-maker with mandatory escalation constraints
- Presenter — structured-to-human data translator

**Cross-Department Roles** (enable multi-department collaboration):
- Process Efficiency Coordinator — cross-department process optimizer and job description improver
- Inter-Department Liaison — logistics, relationship management, and domain translation
- Workflow Architect — structural integrity evaluator

### How the Roles Address Human Dependency

| Human dependency | Role that addresses it |
|---|---|
| Humans broker cross-department requests | Inter-Department Liaison |
| Humans make routine scope/filtering decisions | Autonomous Decision Delegate |
| Humans schedule, assign, and prioritize work | Intake & Prioritization Manager |
| Humans interpret raw agent output | Presenter |
| Humans notice when workflow design is flawed | Workflow Architect |
| Humans optimize processes across departments | Process Efficiency Coordinator |

### Revised Human Loop Model

| Product Complexity | Previous Human Loops | Current Human Loops |
|---|---|---|
| Simple (ice cream scoop) | 2-3 mandatory | 0 expected (final decision only if needed) |
| Standard (water distiller) | 3-4 mandatory | 0-2 conditional |
| Complex (SUV category) | 4+ mandatory | 2-3 conditional + 1 mandatory (final decision) |

The only truly mandatory human decision point is now the final purchase decision. All other human loops are conditional, triggered only when the Autonomous Decision Delegate's confidence is insufficient.

---

## Risks and Mitigations

| Risk | Mitigation |
|---|---|
| Autonomous Decision Delegate makes confidently wrong decisions | Mandatory escalation categories (safety, legal, high-dollar, irreversible, first-of-kind); Workflow Architect reviews and adjusts |
| Inter-Department Liaison becomes a bottleneck | Process Efficiency Coordinator monitors throughput; Liaison can be parallelized if volume demands |
| Workflow Architect becomes bureaucratic overhead | Role focuses on risk-proportionate review, not universal gatekeeping; low-risk departments get light-touch review |
| Too many roles create coordination overhead | Intake & Prioritization Manager centralizes coordination; Process Efficiency Coordinator optimizes inter-role communication |
| Departments compete at expense of system goals | Workflow Architect evaluates competitive dynamics; designs arbitration frameworks |

---

## Future Considerations

1. **Department scaling**: The current architecture is designed for one operational department (Product Evaluation) with cross-department roles ready to serve additional departments. As new departments are added, each will need its own core roles but can share the cross-department roles (Process Efficiency Coordinator, Workflow Architect, and potentially the Inter-Department Liaison pattern).

2. **Risk profile evolution**: The Autonomous Decision Delegate's oversight mechanisms should be reviewed by the Workflow Architect before deploying autonomous decision-making in any department with a risk profile above "Low."

3. **Competitive dynamics**: The department owner anticipates departments that occasionally compete for resources or priorities. The Workflow Architect should design priority arbitration frameworks before this becomes an operational issue.

4. **Oversight mechanism implementation**: The recommended future oversight mechanisms (audit sampling, peer review gates, outcome tracking) are documented but not yet implemented. They should be evaluated and prioritized as part of the next architectural review cycle.

---

## Outcome

All decisions documented here are reflected in the repository:
- Updated: `job-descriptions/job_desc_process_efficiency_coordinator.md` (cross-department)
- Updated: `job-descriptions/job_desc_autonomous_decision_delegate.md` (oversight section added)
- Created: `job-descriptions/job_desc_inter_department_liaison.md`
- Created: `job-descriptions/job_desc_intake_and_prioritization_manager.md`
- Created: `job-descriptions/job_desc_presenter.md`
- Created: `job-descriptions/job_desc_workflow_architect.md`
- Updated: `processes/workflow_product-evaluation.md` (all new roles integrated)
- Created: `decisions/role-evaluation-001-cross-department-autonomy.md` (this document)
