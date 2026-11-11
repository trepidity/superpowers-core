# Project Records in Superpowers Core — Design (revision 2)

Status: approved by the owner on 2026-11-11 for planning and implementation.
Revision 2 replaces the 2026-11-11 first draft after the owner's review:
native brainstorming is kept as is, records are extracted at planning time,
and nothing in this design is a validator, a script-enforced check, or an
authority gate.

## Intent

Superpowers-core already does brainstorming, design documents and plans
well. PKC's lesson is where knowledge should live afterwards: one record per
requirement, decision, specification and plan, named by domain, type and
topic, so nothing governing is buried in a long narrative and an agent
working on "x" can find and read only the files about "x".

This design adds that record layer to superpowers-core as pure markdown
instructions. The brainstorming conversation and its design document stay
native. When the owner approves the design and the plan is written, the
approved content is broken out into records and the plan is written as a
record that cites them. Execution and review read the records the plan
names.

## Goals

1. Brainstorming is unchanged except that its recon reads existing records
   by filename and its design document is the approval artifact it is today.
2. Writing-plans extracts requirements, decisions and specifications from
   the approved design into records, then writes the plan as a plan record
   whose Governing Records cite them.
3. Executors and reviewers read the governing records the plan names and
   may read any other record by filename when they judge it relevant.
4. One shared markdown reference (layout, record types, lifecycle,
   templates) serves every skill.

## Non-goals

- No validator, no hooks, no CLI, no receipts, no gate, no hash comparison,
  no network retrieval. The only script edit is a one-line fix to an
  existing helper (`task-brief`, below).
- No migration of existing `docs/superpowers/` files in any project.
- No new step that blocks on a missing owner, release, status or
  authorization. Status and metadata describe; they never stop work.
- No change to the brainstorming conversation, its sizing, its builder
  check or its HARD-GATE.

## Anti-goals

- Requirements becoming containers for decisions, alternatives or
  investigation narrative.
- An agent setting a record to accepted, rejected, superseded or retired
  when the owner did not say so about that record.
- A record layer so heavy that the plan-and-execute workflow costs more
  tokens than superpowers-core does today. Measured, not assumed.
- Forced reading: an instruction to traverse every domain, every
  dependency chain or every legacy document regardless of the task.

## Record model (from PKC v0.3, as instructions only)

Source: PKC `docs/domains/contract/specs/spec-002-typed-document-models.md`
and `spec-001-filesystem-and-authority-model.md` at PKC main `eafe717`.

| Record | Path | Owns |
| --- | --- | --- |
| Requirement | `docs/domains/<domain>/requirements/req-NNN-<topic>.md` | Required outcomes, scope, acceptance criteria |
| Decision | `docs/domains/<domain>/decisions/adr-NNN-<topic>.md` | One choice among alternatives: question, evidence, alternatives, rationale, consequences |
| Specification | `docs/domains/<domain>/specs/spec-NNN-<topic>.md` | Interfaces, inputs and outputs, states, error behavior, verification obligations |
| Plan | `docs/domains/<domain>/plans/plan-NNN-<topic>.md` | Execution tasks and observed completion evidence |

- **Identity.** `<PREFIX>-(REQ|ADR|SPEC|PLAN)-NNN`; filename suffix equals the
  ID suffix; IDs are never reused or renumbered. The same knowledge unit is
  edited in the same file. A replacement gets a new ID with reciprocal
  `supersedes` / `superseded_by`.
- **Domain and prefix.** The agent uses the repository's existing domains
  and prefixes. For a new domain it picks a short unused prefix and states
  it in the plan hand-off. No confirmation step.
- **Metadata.** PKC's keys, carried by the templates: `id, type, domain,
  status, owner, release_scope, applies_to, depends_on,
  external_dependencies, supersedes, superseded_by`. `owner` and
  `release_scope` come from the project instruction file when it declares
  them, otherwise `project-owner` and `[unresolved]`. Nothing downstream
  depends on either value. `external_dependencies` pins a source in another
  repository by URL, path and commit; the agent reads it from a local clone
  if one is present and otherwise notes that it was not read. It never
  fetches.
- **Sections.** Each type's PKC H2 sections, in PKC's order, from the
  template. A section with nothing to say reads `None.` Optional H3s
  subdivide. No placeholder rules, no size rule beyond one cohesive topic
  per file.
- **Lifecycle.** `proposed, accepted, rejected, superseded, retired`. A
  new record is `proposed`. Status changes only when the owner says so
  about that record; the commit that changes `status` names the act and
  its date, and the body is untouched. An
  existing record keeps its status when edited. These are descriptions of
  the record, not conditions for work: a plan executes against proposed
  records exactly as against accepted ones, because the owner approved the
  design and the plan.

## How the workflow changes

### Brainstorming (native, one addition)

The design document stays the brainstorming output: narrative, approved by
the owner, committed at `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`.
The builder check and the HARD-GATE are unchanged.

One addition at the current-state and recon step: when the repository has
`docs/domains/`, list its record filenames (one directory listing) and read
the records whose names bear on the topic, plus any requirement whose name
or `applies_to` says it is project-wide. Follow `depends_on` only where the
agent judges it matters. Say what was read and what governs. If nothing
applies, say so and move on. Filenames are the index; there is no
catalogue to maintain.

### Writing-plans (the extraction step)

Writing-plans gains a Records step between its Scope Check and File
Structure:

1. **Match existing records.** List `docs/domains/` filenames. Where the
   approved design restates, revises or contradicts an existing record,
   edit that record in place (same knowledge unit) or write a replacement
   with `supersedes`; do not write a duplicate. A contradiction with an
   accepted record is named in the hand-off for the owner to rule on; the
   plan still gets written.
2. **Extract records from the approved design.** Using the table below.
   Each record's Evidence cites the design document path, its commit and
   the approval date. Records are `proposed`.
3. **Write the plan as a plan record** at
   `docs/domains/<domain>/plans/plan-NNN-<topic>.md`, Governing Records
   citing each record and the design document by path.
4. **Footer on the design document.** Append `## Records` listing the IDs
   and paths extracted from it, so a reader of the narrative finds the
   living records. The design document is not otherwise edited; after
   extraction the records are what later work reads and edits.

| Design content | Record and section |
| --- | --- |
| Intent and why, goals | Requirement: Business Requirements, Functional Requirements. One requirement per independent obligation |
| Non-goals, anti-goals | Requirement: Scope Excluded; an observable anti-goal becomes a negative acceptance criterion |
| Constraints | The requirement or specification they constrain; a constraint chosen among alternatives is a decision |
| Choices the owner made among alternatives | One decision each: Question and Context, Alternatives, Rationale, Consequences |
| Interfaces, states, error behavior, data contracts | One specification per cohesive contract, single-domain included |
| What is left to the builder | Specification Scope: a `Builder's discretion:` line |
| Calls the agent made that the owner is to check | Lasting choices: proposed decisions. Anything a later worker must know: the relevant specification. Routine choices: no record |

The plan's Governing Records has one line per record: ID, path, status,
what it constrains, then a `### Global Constraints` H3 as today. Each
`### Task N` carries a `Governing:` line naming the records and sections
that constrain it. The self-review checks coverage against every governing
record, not only the design document.

Hand-off text lists the records created or changed, the prefix if new, any
contradiction found, and then the existing execution-method question. The
same message says: "Say `accept <IDs>` to mark any of these accepted." One
utterance, no separate round, no gate.

### Plan record shape

| PKC H2 | Superpowers content |
| --- | --- |
| (between H1 and first H2) | The agentic-worker banner |
| Outcome and Scope | Goal, Architecture, Tech Stack |
| Governing Records | Record lines, design document path, `### Global Constraints` |
| Steps | One `### Task N: <name>` H3 per task with Files, Interfaces, Governing, Verification and checkbox steps. No Steps table and no Packages table: both served the dropped tooling |
| Dependencies and Risks | `### Review Focus` H3 |
| Verification | Mode, authority source, required checks, who accepts UAT |
| Completion Evidence | `None.` until the end. Observations live in the workspace ledger during execution; the controller writes this section once from the ledger when the plan completes, committed per the user's commit instructions |
| Open Questions | As PKC |

Plans under `docs/superpowers/plans/` with a `Spec:` line keep working;
executors accept either a `Spec:` line or Governing Records.

### Execution and review

- SDD controller and executing-plans: where the skill says "if the plan
  names a Spec", read the Governing Records instead. Each task's brief
  carries the task's `Governing:` records and the Global Constraints.
  Nothing is verified before Task 1 beyond what the skills check today.
- Implementer, task-reviewer and re-review prompts: the "spec/design" slot
  takes the governing record paths. Subagents read what the brief cites.
  They may also read any other record by filename when the code they touch
  points at it, and must report any record they relied on that the brief
  did not name. A conflict between a record and the plan is NEEDS_CONTEXT
  or a finding, as today.
- Final whole-branch reviewer and requesting-code-review:
  `PLAN_OR_REQUIREMENTS` takes record paths; "the spec is a vision
  document" wording refers to the governing records.
- finishing-a-development-branch: before offering the integration options,
  list records created or changed on the branch, and apply "when a change
  owes a record" once to anything the branch did that no record covers.
  Status is never changed here.

### When a change owes a record

Classify by meaning, not label: a new or changed required outcome owes a
requirement; a lasting choice among alternatives owes a decision; a new or
changed interface, data contract, state machine or failure behavior owes a
specification; multi-step work owes a plan; restoring documented behavior
owes nothing.

## Changes by file

New skill `skills/project-records/` (markdown only):
- `SKILL.md`: the record table, identity and prefix rules, filenames-as-index
  reading, lifecycle as description, the extraction table, "when a change
  owes a record". Short.
- `references/layout.md`, `references/lifecycle.md`, `references/templates.md`:
  adapted from PKC's packaged references with validator and tooling text
  removed. Templates are the only place the full metadata key list appears.

Existing skills:
- `brainstorming/SKILL.md`: one paragraph at recon for reading records by
  filename. Nothing else.
- `writing-plans/SKILL.md`: Records step; plan path; header-to-section
  mapping; `Spec:` becomes Governing Records; self-review coverage; hand-off
  text with the record list and the accept offer.
- `subagent-driven-development/SKILL.md`, `implementer-prompt.md`,
  `task-reviewer-prompt.md`, `re-review-prompt.md`: governing-records slot;
  read-and-report rule for subagents.
- `subagent-driven-development/scripts/task-brief`: a task section ends at
  the next heading of equal or higher level. Today the last task runs to
  end of file, which in a plan record would put Dependencies and Risks,
  Verification, Completion Evidence and Open Questions into the last
  brief. One awk condition. `sdd-workspace` is unchanged: `plan-NNN-<topic>`
  names collide only when two domains share a topic name, and the existing
  marker disambiguation already handles that.
- `executing-plans/SKILL.md`: plan-path wording; Governing Records.
- `requesting-code-review/SKILL.md`, `code-reviewer.md`: record paths.
- `finishing-a-development-branch/SKILL.md`: the record listing above.
- `diagnosing-superpowers/prompts/plan-adherence.md`: find plans under
  `docs/domains/*/plans/` as well as today's locations.
- README.md and both host manifests if skill discovery lists skills
  explicitly (AGENTS.md: update Codex and Claude manifests together).

## Measurement

Deferred by the owner on 2026-11-11. When wanted: run the complete workflow
(brainstorm, plan, execute, review) on the same repository, task, model and
effort with current superpowers-core and with this integration, and score
total tokens across all sessions and subagents plus requirement recall and
scope creep against an expected list fixed before the run. No efficiency
claim is made for this integration until that runs.

## Verification for this change

Per this fork's AGENTS.md: manual acceptance scenarios; no new automated
behavioral tests. `tests/claude-code/test-sdd-workspace.sh` and the SDD
script tests run as maintenance checks after the `task-brief` edit.

Acceptance scenarios (fresh sessions, installed plugin):
1. Brainstorm in a repo with no `docs/domains/`: conversation, design
   document and builder check behave as today; nothing is written under
   `docs/domains/`.
2. Brainstorm in a repo with records in two domains: recon reports which
   records it read and what governs; it does not list every record body in
   context.
3. Writing-plans on scenario 1's approved design: a requirement, the
   decisions the design made, a specification, all `proposed`; a plan
   record with PKC H2s and Task H3s; the design document gains a `## Records`
   footer; the hand-off lists the records and offers `accept`.
4. Writing-plans where the design restates an existing record: that record
   is edited in place and no duplicate appears; a contradiction with an
   accepted record is named in the hand-off and the plan is still written.
5. Owner says `accept <ID>`: only that record's `status` changes, in a
   commit naming the act and its date; the body is untouched.
6. SDD on the scenario 3 plan: each brief carries its task's `Governing:`
   records and Global Constraints; the last task's brief contains no
   trailing plan sections; an implementer that reads an unnamed record
   reports it; no step stops for a missing owner, release or status.
7. A legacy `docs/superpowers/plans/` plan with a `Spec:` line executes
   unchanged.
8. finishing-a-development-branch lists the records the branch touched and
   changes no status.

## Left to the builder

Exact prose of the skill edits; how the project-records references are
split; the wording of the recon report and the hand-off list.

## Calls made in this design (owner to check)

- Records are extracted at writing-plans time from the approved design, not
  written during brainstorming.
- The design document stays and gets a `## Records` footer; records are the
  living text afterwards.
- New records are `proposed`; status is descriptive and never gates
  execution; design and plan approval are the only approvals.
- Reading is pull-only by filename; traversal is by judgment; subagents may
  read records and must report unnamed ones they relied on.
- Full PKC metadata keys are kept in the templates; `owner` and
  `release_scope` default when undeclared and nothing depends on them.
- No Steps or Packages table in the plan record.
- `task-brief` is the only script change; `sdd-workspace` is untouched.
- Measurement is deferred; no efficiency claim is made.

## Records

None. This repository keeps no `docs/domains/`; the plan is
`docs/superpowers/plans/2026-11-11-project-records.md`.
