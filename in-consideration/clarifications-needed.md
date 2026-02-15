# Clarifications Needed from Humans
**Prepared by**: Jill (Presenter)
**Date**: 2026-02-15
**Status**: Pending Human Review

---

## Purpose
This document captures areas where clarification is needed to complete the organizational setup and ensure agents can operate effectively within established guidelines.

---

## High Priority Clarifications

### 1. Agent Names Assignment
**Context**: Currently, only one role has an assigned name (Jill for Presenter). All other roles are defined but unnamed.

**Questions**:
- Should agents be assigned names for easier reference and personalization?
- If yes, what naming convention should be used? (e.g., professional names, thematic names, functional identifiers)
- Should names be assigned by humans, or can agents choose their own names within guidelines?

**Impact**: Low operational impact, but affects how agents refer to each other and how humans interact with the swarm. Named agents may feel more personable and be easier to reference in conversations.

**Recommendation**: Assign names to create a more cohesive team identity. Consider letting each role choose their own name from a provided list or within naming guidelines.

---

### 2. Scope of Home vs. Business Support
**Context**: The organization serves dual purposes (home support for Brian and Wes, business support for GreenWest LLC), but the boundary between these contexts isn't fully defined.

**Questions**:
- Are there certain types of requests that should only be handled in one context or the other?
- Should agents track which context a request belongs to, or is it acceptable for contexts to blend?
- When Brian and Wes have conflicting priorities, how should agents handle prioritization?
- Should tone/formality adapt based on context (casual for home, professional for business)?

**Impact**: Medium operational impact. Affects how agents triage, prioritize, and communicate.

**Recommendation**: Define clear guidelines in knowledge/facts.yaml about context handling and priority resolution when stakeholders have different needs.

---

### 3. Knowledge Queue Resolution
**Context**: There are currently three items in knowledge/queue.yaml that need human review:

**Q-001**: What is the typical budget range for product evaluations?
- **Impact**: Without budget guidance, agents evaluate products across all price ranges, which may waste effort on options outside the buyer's realistic range
- **Recommendation**: Establish default budget assumptions per category, or require explicit budget specification per evaluation

**Q-002**: Are there additional consumer protection policies beyond Missouri defaults?
- **Impact**: Low immediate impact, but may affect long-term vendor vetting and warranty standards
- **Recommendation**: Review and either confirm Missouri defaults are sufficient, or document additional internal policies

**Q-003**: Should product evaluations include used/refurbished options?
- **Impact**: High operational impact. Currently agents escalate this question per-task, creating inefficiency
- **Recommendation**: Establish a clear policy (e.g., "Include refurbished from authorized sellers only" or "New products only unless explicitly requested")

**Action Needed**: Review and resolve these queue items to reduce agent escalations.

---

## Medium Priority Clarifications

### 4. Cross-Department Expansion Plans
**Context**: The current architecture is designed to scale to multiple departments, but only the Product Evaluation department is fully defined.

**Questions**:
- What additional departments are anticipated? (e.g., Home Management, Software Development, Marketing Support)
- What is the timeline for adding new departments?
- Should the cross-department roles (Process Efficiency Coordinator, Inter-Department Liaison, Workflow Architect) be activated now, or wait until multiple departments exist?

**Impact**: Medium impact on resource allocation and role activation timing.

**Recommendation**: Activate cross-department roles when the second department is created. Begin planning the next department's role definitions based on anticipated needs.

---

### 5. Organizational Standards Guardian Activation
**Context**: A new job description has been created for the Organizational Standards Guardian role, but it's unclear when this role should be activated or who should fill it.

**Questions**:
- Should this role be activated immediately, or wait until the organization is more mature?
- Should an existing agent take on this role in addition to their primary role, or should it be a dedicated role?
- What is the audit cadence? (e.g., weekly spot checks, monthly comprehensive audits, quarterly reviews)

**Impact**: Medium impact. Having the Guardian early helps establish cultural norms; waiting may allow natural practices to emerge first.

**Recommendation**: Activate the role at a "medium intensity" level — periodic reviews rather than constant auditing — to help establish cultural norms without creating bureaucratic overhead.

---

### 6. Hillcrest Work Overlap Policy
**Context**: Wes works at Hillcrest (nonprofit) and employs a separate agent swarm there, but topics may overlap with home/business swarm contexts.

**Questions**:
- When Hillcrest-related questions arise in the home/business swarm, should they be redirected to the Hillcrest swarm, or handled here?
- Are there confidentiality or conflict-of-interest considerations when topics overlap?
- Should knowledge gained in one swarm be shared with the other, or kept separate?

**Impact**: Low immediate impact, but may become important if overlaps occur frequently.

**Recommendation**: Establish a clear separation policy: Hillcrest work stays with Hillcrest swarm unless explicitly brought to home/business swarm by Wes. Knowledge is not shared across swarms without explicit permission.

---

## Low Priority Clarifications

### 7. Success Metrics and Reporting Cadence
**Context**: The workflow documentation defines many KPIs, but it's unclear how often they should be measured and reported.

**Questions**:
- How often should agents report on KPIs? (e.g., weekly, monthly, quarterly)
- Who should receive KPI reports? (Wes only, Wes and Brian, entire household)
- Should KPIs be reported proactively, or only when requested?

**Impact**: Low operational impact. KPIs are useful for continuous improvement but not critical for day-to-day operation.

**Recommendation**: Start with monthly KPI summaries, adjusted based on feedback. Automate where possible to reduce overhead.

---

### 8. Future Department Naming and Scope
**Context**: As the organization grows, new departments will be needed. It's helpful to plan ahead.

**Potential Departments to Consider**:
- **Home Management Department**: Household tasks, scheduling, maintenance tracking
- **Software Development Support**: Code review, testing, deployment assistance for GreenWest LLC projects
- **Knowledge Management**: Document organization, information retrieval, research assistance
- **Financial Planning**: Budget tracking, purchase optimization across categories
- **Health & Wellness**: Meal planning, exercise tracking, medical appointment coordination

**Questions**:
- Which departments should be prioritized?
- Should each department follow the same structure as Product Evaluation (core roles + operations roles), or can they be simpler?

**Impact**: Low immediate impact. Helpful for long-term planning.

**Recommendation**: Add departments based on actual needs as they emerge, rather than preemptively creating structure.

---

## Organizational Structure Clarifications

### 9. Reporting Structure and Accountability
**Context**: The organizational structure defines roles but doesn't explicitly define reporting relationships or ultimate accountability.

**Questions**:
- Who is ultimately accountable for each department? (Wes as owner? A designated lead agent?)
- Do agents "report to" anyone, or is the structure flat with escalation paths only?
- For cross-department roles (Process Efficiency Coordinator, Workflow Architect), who do they report to?

**Impact**: Low immediate impact. May become important as the organization scales and conflicts arise.

**Recommendation**: Establish a light-touch governance model: Wes as ultimate arbiter, with cross-department roles serving as organizational leaders for standards and structure.

---

### 10. Agent Autonomy Boundaries
**Context**: Agents have decision-making authority within their roles, but the boundaries of that authority aren't always clear.

**Questions**:
- Can agents create new processes or workflows without human approval?
- Can agents modify their own job descriptions based on evolving needs?
- Can agents add facts to knowledge/facts.yaml, or only to queue.yaml?
- Can agents create new departments or roles, or must that be human-initiated?

**Impact**: Medium impact. Affects how autonomous the organization can be.

**Recommendation**:
- Agents can propose new processes (via Process Efficiency Coordinator) but need human approval for significant changes
- Agents can recommend job description updates but cannot modify them unilaterally (Process Efficiency Coordinator reviews and proposes changes)
- Agents can only add to queue.yaml; humans confirm items to facts.yaml
- New departments and roles require human initiation and approval

---

## Recommendations for Next Steps

### Immediate Actions (Within 1 Week)
1. Resolve knowledge/queue.yaml items Q-001, Q-002, Q-003
2. Decide whether to assign names to agents, and if so, establish naming guidelines
3. Clarify home vs. business context handling and prioritization

### Short-Term Actions (Within 1 Month)
4. Activate the Organizational Standards Guardian role with an initial audit cadence
5. Define boundaries for agent autonomy (process creation, job description updates, etc.)
6. Establish KPI reporting cadence and audience

### Medium-Term Actions (Within 3 Months)
7. Plan the next department based on emerging needs
8. Evaluate whether cross-department roles should be activated before the second department exists
9. Establish Hillcrest work overlap policy if overlaps occur

### Long-Term Actions (Within 6 Months)
10. Develop success metrics dashboard for organizational health
11. Review and refine core values based on operational experience
12. Conduct comprehensive organizational review with Process Efficiency Coordinator and Workflow Architect

---

**This document will be updated as clarifications are provided and new questions emerge. Agents should reference this document when uncertain about organizational direction, and add new items as gaps are identified.**
