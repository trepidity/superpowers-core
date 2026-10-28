---
name: writing-skills
description: Use when creating or updating reusable agent skills, their routing descriptions, and supporting resources
---

# Writing Skills

Create instructions that help an agent make the intended decision. Resolve the
verification mode through the shared `verification-policy` before choosing how
to validate a skill. Skill authoring does not itself require TDD, automated
evaluation, or subagent dispatch.

## Scope and structure

- Identify the user requests the skill should handle and where it should not apply.
- Write a precise `name` and `description` in YAML frontmatter. The description
  should identify when the skill is useful; the body carries its workflow.
- Put essential decisions and constraints in `SKILL.md`; move long references,
  examples, and reusable scripts into supporting files with clear reading triggers.
- Use paths relative to the skill for its resources. Name sibling skills only
  when needed and available in the target installation.
- Preserve direct user instructions and project authority. A workflow does not
  create new approval gates, testing obligations, or permission to publish.
- Prefer concrete steps and outputs over rationalization tables or repeated threats.
- Retain useful existing behavior when making a narrow change.

## Validate the intended use

Use the project's chosen verification method. For UAT, walk through realistic
requests and inspect the resulting decisions and artifacts. Include a case where
the skill should not activate. Record observed behavior and limitations; a
frontmatter check alone does not establish useful behavior.

Check that referenced resources resolve, scripts perform their documented action,
and descriptions route to the right skill. Existing metadata or packaging
validators are separate from behavioral acceptance.

Automated skill evaluation is optional and belongs to `superpowers-testing`.
Do not load that module or demand its test-first cycle for UAT-only work.

For a skill flow diagram, use `graphviz-conventions.dot` and `render-graphs.js`
in this directory.

## Delivery

Keep one maintained source and use the host's supported installer or link wiring.
Report what changed, what was actually exercised, and what remains unverified.
