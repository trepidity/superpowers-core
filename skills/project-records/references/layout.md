# Layout and metadata

## Paths

- `docs/domains/DOMAIN/requirements/req-NNN-TITLE.md`: type requirement, identifier PREFIX-REQ-NNN
- `docs/domains/DOMAIN/decisions/adr-NNN-TITLE.md`: type decision, identifier PREFIX-ADR-NNN
- `docs/domains/DOMAIN/specs/spec-NNN-TITLE.md`: type specification, identifier PREFIX-SPEC-NNN
- `docs/domains/DOMAIN/plans/plan-NNN-TITLE.md`: type plan, identifier PREFIX-PLAN-NNN

Also recognised when present, not created by this skill: root documents
(AGENTS.md, CLAUDE.md, README.md, CONTRIBUTING.md, SECURITY.md, CHANGELOG.md),
`docs/architecture.md`, `docs/infrastructure.md`,
`docs/domains/DOMAIN/architecture.md`, and glossary terms at
`docs/glossary/DOMAIN/TERM.md`.

Titles and domains are lowercase ASCII kebab-case. NNN is at least three
digits and matches the identifier suffix. Identifiers are never reused or
renumbered.

## Metadata keys

Every record carries exactly these keys, in this order:

`id, type, domain, status, owner, release_scope, applies_to, depends_on,
external_dependencies, supersedes, superseded_by`

- `type`: requirement, decision, specification or plan; agrees with the path.
- `status`: proposed, accepted, rejected, superseded or retired.
- `applies_to`: the domains the record binds, or `[all]` for a project-wide
  obligation. Not ownership.
- `depends_on`: records that constrain how this one is read or executed. Not
  relatedness.
- `external_dependencies`: list of `{repository, id, path, revision}` objects
  pinning a governing source in another repository; `release` optional.
- `supersedes` / `superseded_by`: reciprocal; both files change together.

Fill every key; use `[]` for none. Lists are YAML sequences.

## Required sections

Each type's H2 sections, in this order. Optional H3s subdivide a section. A
section with nothing to say reads `None.`

- requirement: Scope, Business Requirements, Functional Requirements, Acceptance Criteria, Dependencies, Evidence, Open Questions
- decision: Scope, Question and Context, Decision, Alternatives, Rationale, Consequences, Dependencies, Evidence, Open Questions
- specification: Scope, Governing Records, Inputs and Outputs, States and Transitions, Interfaces, Error Behavior, Verification Obligations, Evidence, Open Questions
- plan: Outcome and Scope, Governing Records, Steps, Dependencies and Risks, Verification, Completion Evidence, Open Questions
