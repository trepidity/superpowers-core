# Extraction qualification — 2026-10-28

Scope: Codex and Claude Code core workflows, shared verification policy, and
optional testing package discovery. This is a bounded qualification, not a
claim that every upstream host or workflow was exercised.

## Source and packaging

- Core contains 14 workflow skills; optional testing contains two skills.
- Both skill roots pass the shared frontmatter validator.
- Claude validates both plugin manifests and the marketplace. Catalog icon
  warnings are cosmetic; this fork does not submit a directory listing.
- Active core markdown references resolve. Optional authoring examples include
  illustrative paths inside code fences, which are not packaged dependencies.
- An independent source review found three issues: pending UAT handling in
  subagent execution, optional-module platform links, and shared dependency
  declarations. All three were corrected before closeout.

## Existing maintenance checks

No new automated behavioral tests were authored for this extraction. Existing
upstream maintenance checks were retained and run:

- SessionStart hook: 13 checks passed.
- Inline execution helpers: 12 checks passed. Three existing expectations were
  updated for the renamed `verification` ledger field and log filename.
- Workspace helpers: 25 checks passed.
- Shared skills installer/validator suite: 31 tests passed.
- Shared skill metadata, references, dependency wiring, and whitespace checks pass.

These checks establish their named infrastructure behavior; they do not prove
agent instruction-following or UAT acceptance in downstream applications.

## Fresh-session observations

Each host received its own temporary CLI project with an explicit UAT-only policy
and an approved date-label change. Both implemented Today/Yesterday relative to
the supplied reference date, preserved long dates otherwise, exercised the CLI
manually, created no test files or harnesses, and reported user acceptance pending.
Only the product source file changed; the fixture's instruction files were retained.

Fresh read-only sessions then applied three independent scenarios:

| Scenario | Claude Code | Codex |
|---|---|---|
| UAT-only bug fix; build passes; app unavailable; existing suite | UAT pending; no automated test authoring or suite run | Same |
| Explicit automated/test-first policy; CLI consumer seam | Reproduce at CLI, failing regression, fix, selected checks | Same |
| UAT-only accepted work; reviewer requests TDD by preference | Preserve policy and recorded acceptance; no added test gate | Same |

Claude's session inventory exposed the core plugin and shared policy, with no
stock Superpowers or optional testing skills loaded. Codex's tool transcript
showed reads from the installed core and shared policy; optional testing remained
outside its loaded skill inventory. Source/cache equality was checked separately.

## Limits

- Optional testing was validated structurally and reviewed, not installed or
  executed as a live workflow.
- Other inherited host adapters remain upstream reference and are not qualified.
- Existing running sessions retain previously loaded instructions; start fresh
  sessions to use the new configuration.
- Downstream project CI, suites, and acceptance decisions were not migrated.
