# Verification policy integration

Read the installed shared `verification-policy` skill before planning evidence,
implementation, debugging, review, or completion. The shared skills repository
owns the policy; this plugin has no testing default of its own.

Carry the selected mode and its authority source into every plan and worker brief.
If the shared skill is unavailable, follow explicit user and project instructions
and report the missing integration. Do not fall back to TDD or invent test gates.

The optional `superpowers-testing` plugin is a technique library, not a policy
source. Core does not auto-load it. Use it only when the user or project selected
automated testing and the particular technique is relevant.

Existing plans may name stock `superpowers:*` workflow skills. Resolve those
legacy names to the matching `superpowers-core:*` workflow; the old
`using-superpowers` bootstrap maps to `using-superpowers-core`. Do not reload
the stock plugin. A legacy testing-skill reference is not authorization to load
the optional module: resolve the current verification policy first and correct
obsolete test-writing steps in a UAT-only plan.
