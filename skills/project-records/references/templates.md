# Record templates

Copy the shape for the record type, replace every angle-bracket placeholder,
and keep the sections in order. A section with nothing to say reads `None.`
The example metadata uses domain `secrets` and prefix `SEC`.

## requirement

```markdown
---
id: SEC-REQ-001
type: requirement
domain: secrets
status: proposed
owner: project-owner
release_scope: [mvp]
applies_to: [secrets]
depends_on: []
external_dependencies: []
supersedes: []
superseded_by: []
---
# <One cohesive obligation>

## Scope
- Included: <bounded outcome and applicability>.
- Excluded: <explicit exclusions, or None.>.

## Business Requirements
1. <BR-1 — required business outcome and why it matters, or Not applicable: <reason>.>

## Functional Requirements
1. <FR-1 — observable system behavior that delivers a business requirement, or Not applicable: <reason>.>

## Acceptance Criteria
1. AC-1 — Given <precondition>, when <action>, then <observable result>; verified by <method>.

## Dependencies
None.

## Evidence
None.

## Open Questions
None.
```

## decision

```markdown
---
id: SEC-ADR-001
type: decision
domain: secrets
status: proposed
owner: project-owner
release_scope: [mvp]
applies_to: [secrets]
depends_on: []
external_dependencies: []
supersedes: []
superseded_by: []
---
# <One decision topic>

## Scope
<Decision boundary, affected concern, and explicit exclusions.>

## Question and Context
<The decision to make and conditions that matter.>

## Decision
<Chosen option, or Unresolved: specific choice pending.>

## Alternatives
| Option | Benefits | Costs or limitations |
| --- | --- | --- |
| <candidate> | <benefits> | <costs> |

## Rationale
<Why the proposed/chosen option meets the relevant constraints.>

## Consequences
- Positive: <effect, or None.>.
- Negative: <effect, or None.>.
- Follow-up: <necessary action, or None.>.

## Dependencies
None.

## Evidence
None.

## Open Questions
None.
```

## specification

```markdown
---
id: SEC-SPEC-001
type: specification
domain: secrets
status: proposed
owner: project-owner
release_scope: [mvp]
applies_to: [secrets]
depends_on: [SEC-REQ-001]
external_dependencies: []
supersedes: []
superseded_by: []
---
# <One bounded capability or change contract>

## Scope
- Included: <capability boundary>.
- Excluded: <non-goals, or None.>.

## Governing Records
- SEC-REQ-001 — <constraint governing this contract>.

## Inputs and Outputs
| Name | Direction | Type or shape | Constraints |
| --- | --- | --- | --- |
| <name> | <input/output> | <exact data shape> | <validation and cardinality> |

## States and Transitions
| From | Trigger | Guard | To | Observable effect |
| --- | --- | --- | --- | --- |
| <state> | <event> | <condition or None.> | <state> | <effect> |

## Interfaces
| Interface | Participants | Contract |
| --- | --- | --- |
| <name> | <producer/consumer> | <boundary, operation and version/compatibility behavior> |

## Error Behavior
| Condition | Observable response | Recovery or retry |
| --- | --- | --- |
| <failure> | <typed error/result and side effects> | <allowed behavior> |

## Verification Obligations
1. V-1 — Verify <governing ID and local criterion, or contract clause> using <method>; expected result: <result>.

## Evidence
None.

## Open Questions
None.
```

## plan

The `## Steps` section holds the writing-plans task shape: one `### Task N`
H3 per task. `## Completion Evidence` stays `None.` until the plan is done;
the executor writes it once from its ledger.

```markdown
---
id: SEC-PLAN-001
type: plan
domain: secrets
status: proposed
owner: project-owner
release_scope: [mvp]
applies_to: [secrets]
depends_on: [SEC-REQ-001, SEC-SPEC-001]
external_dependencies: []
supersedes: []
superseded_by: []
---
# <One bounded delivery effort>

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers-core:subagent-driven-development (recommended) or superpowers-core:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

## Outcome and Scope
- Outcome: <observable delivery result>.
- Architecture: <2-3 sentences about approach>.
- Tech stack: <key technologies/libraries>.
- Included: <bounded work>.
- Excluded: <non-goals, or None.>.

## Governing Records
- SEC-REQ-001 — `docs/domains/secrets/requirements/req-001-<topic>.md` — <obligation being delivered>.
- SEC-SPEC-001 — `docs/domains/secrets/specs/spec-001-<topic>.md` — <contract being implemented>.
- Design: `docs/superpowers/specs/<YYYY-MM-DD>-<topic>-design.md`.

### Global Constraints
- <project-wide requirement, exact value, one line each>

## Steps

### Task 1: <Component Name>

**Files:**
- Create: `exact/path/to/file`

**Interfaces:**
- Consumes: <exact signatures from earlier tasks>
- Produces: <exact signatures later tasks use>

**Governing:** <record IDs and sections that constrain this task>

**Verification:** <selected mode and authority source>

- [ ] **Step 1: <one action with a checkable result>**

## Dependencies and Risks
- Dependency: <prerequisite and effect, or None.>.
- Risk: <condition, effect, mitigation and owner, or None.>.

### Review Focus
- <input class or failure mode no task's scenarios exercise>

## Verification
<Mode, authority source, required checks, and who accepts UAT.>

## Completion Evidence
None.

## Open Questions
None.
```
