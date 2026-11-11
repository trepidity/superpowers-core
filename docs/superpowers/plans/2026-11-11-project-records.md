# Project Records Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers-core:subagent-driven-development (recommended) or superpowers-core:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a markdown-only record layer to superpowers-core so writing-plans extracts PKC-shaped requirement, decision and specification records from an approved design and writes the plan as a plan record, and executors and reviewers read the records the plan names.

**Architecture:** One new skill, `skills/project-records/`, holds the record model (layout, lifecycle, templates, extraction table, "when a change owes a record"). Existing skills gain short pointers into it at the points where they read or write a design or plan. The only script change is a one-condition fix to `task-brief` so a task section ends at the next heading of equal or higher level.

**Tech Stack:** Markdown skill files; bash (awk) for the helper fix; the existing `tests/claude-code/test-sdd-workspace.sh` as a maintenance check.

**Verification:** UAT-only, authority superpowers-core `AGENTS.md` ("manual acceptance scenarios; do not add automated behavioral tests"). Required checks: each task's acceptance scenario read against the changed text; `bash tests/claude-code/test-sdd-workspace.sh` after the helper fix. The owner (Jared) accepts UAT from fresh-session scenarios 1–8 in the design.

**Spec:** `docs/superpowers/specs/2026-11-11-project-records-design.md`

## Global Constraints

- No validator, hook, CLI, gate, hash comparison or network retrieval is added. Status and metadata describe; nothing stops work because of a missing owner, release, status or authorization.
- Brainstorming's conversation, sizing, builder check and HARD-GATE text are unchanged; the design document stays at `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`.
- Record paths, IDs, metadata keys and H2 section names are exactly PKC v0.3's (design "Record model" table and `references/templates.md`). New records are `proposed`; a status changes only when the owner says so about that record.
- Plans under `docs/superpowers/plans/` with a `Spec:` line keep working everywhere a Governing Records section is accepted.
- Every edit to an existing skill is a replacement or addition at a named anchor; no restructuring of surrounding text. Codex and Claude read the same skill files; no manifest lists skills by name (`"skills": "./skills/"`), so manifests are untouched.
- Writing style of existing skills is kept: second person, no em-dashes in new text, no process jargon in brainstorming.

## Review Focus

- A plan record whose last task is followed by Dependencies and Risks, Verification, Completion Evidence and Open Questions: the brief for that task must end at `## Dependencies and Risks`, and a `### Task N` heading inside a fenced block must not start or end a section.
- A legacy `Spec:` plan with no Governing Records: SDD and executing-plans must behave exactly as today, including the "no reachable spec" ledger note.
- A design that restates an existing record: writing-plans must edit that file, not create `req-002-<same-topic>`.
- A repo with no `docs/domains/`: brainstorming recon must say nothing about records and write nothing; writing-plans creates the first domain and states the prefix.
- A task's `Governing:` line naming a record section that does not exist: the brief still carries the line and the implementer reports NEEDS_CONTEXT rather than searching for a substitute.

---

### Task 1: The project-records skill

**Files:**
- Create: `skills/project-records/SKILL.md`
- Create: `skills/project-records/references/layout.md`
- Create: `skills/project-records/references/lifecycle.md`
- Create: `skills/project-records/references/templates.md`

**Interfaces:**
- Produces: the section names other tasks point at: `## Reading records`, `## Extracting records from an approved design`, `## When a change owes a record`, `## Lifecycle`, `## Identity and prefixes`.

**Verification:** UAT-only, superpowers-core AGENTS.md.

- [ ] **Step 1: Acceptance scenario**

Setup: an agent in a repo with `docs/domains/auth/requirements/req-001-login.md` is asked to plan a feature touching login.
Actions: it reads `SKILL.md` and follows "Reading records".
Expected: it lists `docs/domains/` filenames once, reads `req-001-login.md` and any record whose name or `applies_to` says project-wide, follows `depends_on` only where it judges it matters, and reports what it read. It writes no catalogue.

- [ ] **Step 2: Write SKILL.md**

Frontmatter `name: project-records`, description: "Use when a project keeps requirements, decisions, specifications and plans as records under docs/domains/, when extracting records from an approved design, or when deciding whether a change owes a record." Sections, in order: Overview (the design's Intent paragraph, four lines); the record table from the design; `## Identity and prefixes`; `## Reading records` (filenames as index, pull by judgment, report what was read, say so when nothing applies); `## Extracting records from an approved design` (the design's "Writing-plans" steps 1–4 and the extraction table, plus the Evidence line format `Source: <design path> at <commit>, approved by <owner> on <YYYY-MM-DD>.`); `## Lifecycle` (five statuses, new is `proposed`, owner-stated changes only, edit in place keeps status, replacement gets new ID with reciprocal `supersedes`/`superseded_by`, status never conditions work); `## When a change owes a record`; `## Metadata defaults` (`owner` and `release_scope` from the instruction file else `project-owner` and `[unresolved]`; `external_dependencies` read from a local clone if present, else noted as not read, never fetched). Under 120 lines.

- [ ] **Step 3: Write the references**

`layout.md`: PKC's layout reference with the overview/glossary/root-document lines kept as "also recognised, not created by this skill", and the "Unknown keys ... invalid" line replaced by "Fill every key; use `[]` for none." `lifecycle.md`: the five statuses, the allowed transitions as a short list (proposed→accepted/rejected; accepted→superseded/retired; rejected→proposed), each "when the owner says so about that record"; same-unit edit versus replacement; no `pkc check` wording. `templates.md`: the requirement, decision, specification and plan templates verbatim from PKC, except the plan template drops the Steps table, the package table, the package sentence and the fenced entry-format block, and its `## Steps` section holds the writing-plans Task H3 shape. Glossary and architecture templates are omitted.

- [ ] **Step 4: Commit** `Add project-records skill (record model, extraction, lifecycle)`.

### Task 2: Brainstorming recon reads records

**Files:**
- Modify: `skills/brainstorming/SKILL.md` ("**Offer recon.**" paragraph; "**Where it goes:**" line)

**Verification:** UAT-only, superpowers-core AGENTS.md.

- [ ] **Step 1: Acceptance scenario**

Setup: repo with records in two domains. Actions: brainstorming reaches the recon step. Expected: the agent lists `docs/domains/` filenames, reads the records about the topic, says what it read and what governs; the conversation, sizing, builder check and HARD-GATE are otherwise word-for-word unchanged. In a repo without `docs/domains/` nothing about records is said.

- [ ] **Step 2: Add one paragraph after "Offer recon."**

Text: "**Records.** When the project keeps records under `docs/domains/`, part of looking is reading them: list the record filenames once, read the ones whose names bear on the topic and any requirement whose name or `applies_to` says it is project-wide, and follow a record's `depends_on` only where it matters. Say what you read and what governs. Nothing in `docs/domains/` means nothing to read. The project-records skill describes the files."

- [ ] **Step 3: Extend "Where it goes:"**

Append one sentence to the existing line: "Requirements, decisions and specifications are broken out of the approved design into records when the plan is written, not here."

- [ ] **Step 4: Commit** `Brainstorming: recon reads project records by filename`.

### Task 3: Writing-plans extracts records and writes a plan record

**Files:**
- Modify: `skills/writing-plans/SKILL.md` ("Save plans to", new `## Records` section after `## Scope Check`, `## Plan Document Header`, `## Task Structure`, `## Self-Review` item 1, `## Execution Handoff`)

**Interfaces:**
- Consumes: Task 1 section names.
- Produces: the plan record shape that Tasks 4–6 read: PKC H2s with `### Task N` H3s under `## Steps`, a `Governing:` line per task, `### Global Constraints` under `## Governing Records`, `### Review Focus` under `## Dependencies and Risks`.

**Verification:** UAT-only, superpowers-core AGENTS.md.

- [ ] **Step 1: Acceptance scenario**

Setup: an approved design in a repo without `docs/domains/`. Actions: writing-plans runs. Expected: a requirement, the decisions the design made and a specification exist under `docs/domains/<domain>/`, all `proposed`, Evidence citing the design; the plan is at `docs/domains/<domain>/plans/plan-001-<topic>.md` with PKC's seven H2s, the banner between H1 and the first H2, Task H3s under Steps each with a `Governing:` line; the design gained `## Records`; the hand-off lists the records, the prefix, and says "Say `accept <IDs>` to mark any of these accepted." A design restating an existing record edits that record in place.

- [ ] **Step 2: Replace "Save plans to"**

"**Save plans to:** `docs/domains/<domain>/plans/plan-NNN-<topic>.md` as a plan record (shape below) when the project keeps records or the design is being broken out into records; otherwise `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`. User preferences override both."

- [ ] **Step 3: Add `## Records` after Scope Check**

Four numbered steps as the design's "Writing-plans" section states (match existing records; extract per the project-records table; write the plan record; append `## Records` to the design), a pointer "Follow superpowers-core:project-records, section Extracting records from an approved design", and the sentence "A contradiction with an accepted record is named in the hand-off for the owner; the plan is still written."

- [ ] **Step 4: Plan record shape**

Under `## Plan Document Header`, keep the existing header block labelled "Legacy plan (no records)" and add a second fenced block "Plan record" showing: frontmatter from the plan template; `# <title>`; the banner; `## Outcome and Scope` holding Goal, Architecture, Tech Stack; `## Governing Records` holding one line per record (`- ID — path — status — what it constrains`), the design document path, then `### Global Constraints`; `## Steps` with "one `### Task N` per task (Task Structure below)"; `## Dependencies and Risks` holding `### Review Focus`; `## Verification`; `## Completion Evidence` with `None.`; `## Open Questions`. In `## Task Structure` add a `**Governing:**` line after `**Interfaces:**`: "record IDs and sections that constrain this task, including the dependencies reached through `depends_on`".

- [ ] **Step 5: Self-review and hand-off**

Self-review item 1 becomes "Spec coverage: skim each governing record and the design..." Hand-off: before the execution question add "The plan is a record at `<path>`. Records created: `<IDs and paths>`; changed: `<IDs>`; prefix `<PREFIX>` (new). Say `accept <IDs>` to mark any of these accepted." and, when applicable, "Contradiction with `<ID>`: <one line>."

- [ ] **Step 6: Commit** `Writing-plans: extract records and write the plan as a plan record`.

### Task 4: task-brief ends a task at the next heading

**Files:**
- Modify: `skills/subagent-driven-development/scripts/task-brief` (awk block)

**Verification:** UAT-only plus the existing helper test as a maintenance check.

- [ ] **Step 1: Acceptance scenario**

Setup: a plan record whose `## Steps` holds `### Task 1` and `### Task 2`, followed by `## Dependencies and Risks` ... `## Open Questions`; a fenced block inside Task 2 containing the line `### Task 3: fake`. Actions: `bash scripts/task-brief plan.md 2`. Expected: output starts at `### Task 2`, contains the fenced `### Task 3: fake` line, and ends before `## Dependencies and Risks`. `task-brief plan.md 1` ends before `### Task 2`. The legacy plan in `tests/claude-code/test-sdd-workspace.sh` still extracts.

- [ ] **Step 2: Implement**

In the awk program, record the heading level of the matched task (`match($0, /^#+/); level = RLENGTH`) and add a rule before the print: outside fences, a heading line whose level is at most `level` and is not a `Task N` heading sets `intask = 0`. The existing `Task` rule keeps switching between tasks.

- [ ] **Step 3: Verify**

Run the scenario with a temp plan under the scratchpad; run `bash tests/claude-code/test-sdd-workspace.sh`. Expected: scenario outputs as above; test prints all passes.

- [ ] **Step 4: Commit** `task-brief: end a task at the next heading of equal or higher level`.

### Task 5: Subagent-driven development reads governing records

**Files:**
- Modify: `skills/subagent-driven-development/SKILL.md` ("If the plan names a Spec" paragraph; "Task brief" dispatch list item 3; "Reviewer inputs" and the global-constraints bullets; the conflict-ruling sentences that name "the spec")
- Modify: `skills/subagent-driven-development/implementer-prompt.md` (`## Context`)
- Modify: `skills/subagent-driven-development/task-reviewer-prompt.md` ("Global constraints from the spec/design" and the `[GLOBAL_CONSTRAINTS]` placeholder note)
- Modify: `skills/subagent-driven-development/re-review-prompt.md` (`## The Task`)

**Interfaces:**
- Consumes: the plan record shape and `Governing:` line from Task 3.

**Verification:** UAT-only, superpowers-core AGENTS.md.

- [ ] **Step 1: Acceptance scenario**

Setup: SDD on a plan record. Actions: controller reads the plan and dispatches Task 1. Expected: the controller reads the Governing Records once; each brief dispatch carries the task's `Governing:` records (paths) and Global Constraints; the implementer reads those, may read another record by filename when the code it touches points at it, reports any record it relied on that the brief did not name, and reports NEEDS_CONTEXT for a missing constraint; the reviewer receives the same records as its constraints source. A legacy `Spec:` plan behaves as today.

- [ ] **Step 2: SKILL.md**

Replace "If the plan names a Spec, read that too: the spec is the authority the plan argues from, and conflicts inside the plan resolve against it. A plan with no reachable spec gets a ledger note saying so — rulings made without one are provisional." with the same sentences generalised: "If the plan names a Spec or lists Governing Records, read those too: they are the authority the plan argues from, and conflicts inside the plan resolve against them. A plan with neither reachable gets a ledger note saying so — rulings made without one are provisional." Dispatch item (3) becomes "(3) the task's `Governing:` records as paths, the Global Constraints, and interfaces and decisions from earlier tasks that the brief cannot know". In Reviewer inputs: "Copy the binding requirements verbatim from the plan's Global Constraints section, the spec, or the task's governing records". Elsewhere "the spec is the binding authority" becomes "the spec or governing records are the binding authority" (three places).

- [ ] **Step 3: Prompts**

implementer-prompt `## Context`: add "Governing records for this task: [GOVERNING_RECORDS] (paths). Read them. You may read another record under `docs/domains/` when the code you touch points at it; report any record you relied on that this list did not name. A constraint you need that is in none of them is NEEDS_CONTEXT, not a search." and a `[GOVERNING_RECORDS]` placeholder line. task-reviewer-prompt: "Global constraints from the spec/design or governing records that bind this task:" and the placeholder note gains "or the task's governing records". re-review-prompt `## The Task`: add "Governing records: [GOVERNING_RECORDS]".

- [ ] **Step 4: Commit** `SDD: briefs and reviews carry the task's governing records`.

### Task 6: Executing-plans, code review, finishing, diagnosing

**Files:**
- Modify: `skills/executing-plans/SKILL.md` ("If the plan names a Spec" paragraph; "The plan is wrong" bullet; final-review dispatch sentence "the plan and spec paths")
- Modify: `skills/requesting-code-review/SKILL.md` (`{PLAN_OR_REQUIREMENTS}` placeholder line)
- Modify: `skills/requesting-code-review/code-reviewer.md` (`## The spec is a vision document` first sentence; placeholder list)
- Modify: `skills/finishing-a-development-branch/SKILL.md` (new Step 1b after Step 1)
- Modify: `skills/diagnosing-superpowers/prompts/plan-adherence.md` (item 1 file locations)

**Verification:** UAT-only, superpowers-core AGENTS.md.

- [ ] **Step 1: Acceptance scenario**

Setup: native execution of a plan record, then finishing. Expected: executing-plans reads the Governing Records as its authority and passes their paths to the final reviewer; the reviewer prompt accepts record paths in PLAN_OR_REQUIREMENTS; finishing lists records created or changed on the branch and names any change that owes a record with none, changing no status; plan-adherence finds plans under `docs/domains/*/plans/`.

- [ ] **Step 2: executing-plans**

Same three replacements as Task 5 Step 2 for the Spec paragraph and "the spec" wording; final-review dispatch: "the plan path and the spec or governing record paths".

- [ ] **Step 3: requesting-code-review**

Placeholder: "`{PLAN_OR_REQUIREMENTS}` - What it should do: plan path, task text, requirements, or the governing record paths". code-reviewer.md: "The spec, or the governing records, say what the software must do." and the `[PLAN_OR_REQUIREMENTS]` placeholder note gains "or governing record paths".

- [ ] **Step 4: finishing-a-development-branch Step 1b "Records"**

"When the project keeps records under `docs/domains/`, list the records this branch created or changed (`git diff --name-only <base>..HEAD -- docs/domains`). Apply project-records' *When a change owes a record* once to what the branch did that no record covers, and write the record or name the gap in the hand-off. Change no record's status here."

- [ ] **Step 5: plan-adherence**

Item 1: "tool calls that write under `docs/`, `docs/domains/*/plans/`, `plans/`, `specs/`, or any file the human named".

- [ ] **Step 6: Commit** `Executing, review, finishing and diagnosing read governing records`.

### Task 7: README and design footer

**Files:**
- Modify: `README.md` (after the Packages list)
- Modify: `docs/superpowers/specs/2026-11-11-project-records-design.md` (append `## Records`)

**Verification:** UAT-only, superpowers-core AGENTS.md.

- [ ] **Step 1: README**

One paragraph after Packages: "**Project records.** When a project keeps requirements, decisions, specifications and plans under `docs/domains/`, writing-plans breaks an approved design out into those records and writes the plan as a record; execution and review read the records the plan names. See `skills/project-records/`."

- [ ] **Step 2: Design footer**

Append to the design: "## Records\n\nNone. This repository keeps no `docs/domains/`; the plan is `docs/superpowers/plans/2026-11-11-project-records.md`."

- [ ] **Step 3: Commit** `README: project records; design footer`.
