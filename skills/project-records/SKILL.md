---
name: project-records
description: Use when a project keeps requirements, decisions, specifications and plans as records under docs/domains/, when extracting records from an approved design, or when deciding whether a change owes a record.
---

# Project Records

A project that keeps records stores its governing knowledge as one file per
requirement, decision, specification and plan, named by domain, type and
topic. Nothing governing is buried in a long narrative, and an agent working
on "x" finds the files about "x" by name and reads only those. Brainstorming
still produces its design document; the records are broken out of that
document when the plan is written.

| Record | Path | Owns |
| --- | --- | --- |
| Requirement | `docs/domains/<domain>/requirements/req-NNN-<topic>.md` | Required outcomes, scope, acceptance criteria |
| Decision | `docs/domains/<domain>/decisions/adr-NNN-<topic>.md` | One choice among alternatives: question, evidence, alternatives, rationale, consequences |
| Specification | `docs/domains/<domain>/specs/spec-NNN-<topic>.md` | Interfaces, inputs and outputs, states, error behavior, verification obligations |
| Plan | `docs/domains/<domain>/plans/plan-NNN-<topic>.md` | Execution tasks and observed completion evidence |

Paths and metadata: [layout](references/layout.md). Shapes:
[templates](references/templates.md). Statuses: [lifecycle](references/lifecycle.md).

## Identity and prefixes

A record's ID is `<PREFIX>-(REQ|ADR|SPEC|PLAN)-NNN`; the filename number is
the ID number. IDs are never reused or renumbered. The same knowledge unit is
edited in the same file. A real replacement gets a new ID, and the two files
point at each other through `supersedes` and `superseded_by`.

Use the repository's existing domains and prefixes. For a new domain, pick a
short prefix no record uses yet and state it in the plan hand-off. No
confirmation step follows.

## Reading records

The filenames are the index. List `docs/domains/` once, read the records
whose names bear on the task, and read any requirement whose name or
`applies_to` says it is project-wide. Follow a record's `depends_on` only
where it matters to the task. Say what you read and what governs. If nothing
applies, say so; do not invent constraints. Keep no catalogue: the listing is
transient.

## Extracting records from an approved design

Done by writing-plans, after the owner approves the design document.

1. **Match existing records.** List `docs/domains/`. Where the design
   restates, revises or contradicts an existing record, edit that record in
   place (same knowledge unit) or write a replacement with `supersedes`.
   Never write a duplicate. A contradiction with an accepted record is named
   in the hand-off for the owner; the plan is still written.
2. **Extract.** Using the table below. One requirement per independent
   obligation, one decision per choice, one specification per cohesive
   contract. Each record's Evidence carries one line:
   `Source: <design path> at <commit>, approved by <owner> on <YYYY-MM-DD>.`
   New records are `proposed`.
3. **Write the plan as a plan record** whose Governing Records cite each
   record and the design document by path.
4. **Append `## Records`** to the design document listing the IDs and paths
   extracted from it. The design is not otherwise edited. From here on the
   records are what later work reads and edits.

| Design content | Record and section |
| --- | --- |
| Intent and why, goals | Requirement: Business Requirements, Functional Requirements |
| Non-goals, anti-goals | Requirement: Scope (Excluded line); an observable anti-goal becomes a negative acceptance criterion |
| Constraints | The requirement or specification they constrain; a constraint chosen among alternatives is a decision |
| Choices the owner made among alternatives | One decision each: Question and Context, Alternatives, Rationale, Consequences |
| Interfaces, states, error behavior, data contracts | One specification per cohesive contract, single-domain included |
| What is left to the builder | Specification Scope: a `Builder's discretion:` line |
| Calls the agent made for the owner to check | Lasting choices: proposed decisions. Anything a later worker must know: the relevant specification. Routine choices: no record |

## Lifecycle

Statuses are `proposed`, `accepted`, `rejected`, `superseded`, `retired`. A
new record is `proposed`. A status changes only when the owner says so about
that record ("accept CORE-REQ-004"); the commit that changes `status` names
the act and its date, and the body is untouched. An existing
record keeps its status when edited. Status describes the record; it never
conditions work. A plan executes against proposed records exactly as against
accepted ones, because the owner approved the design and the plan.

## When a change owes a record

Classify by what the change means, not what it is called.

- A new or changed required outcome: requirement (new, or the same unit
  updated).
- A lasting choice among alternatives, a different architecture included:
  decision.
- A new or changed interface, data contract, state machine or failure
  behavior: specification.
- Multi-step work: plan.
- Restoring documented behavior: nothing.

## Metadata defaults

`owner` and `release_scope` come from the project's instruction file
(AGENTS.md or CLAUDE.md) when it declares them; otherwise `project-owner`
and `[unresolved]`. Nothing depends on either value. `external_dependencies`
pins a source in another repository by URL, path and commit; read it from a
local clone when one is present, otherwise note that it was not read. Never
fetch it.
