---
name: verification-before-completion
description: Use before claiming completion, a fix, or a passing check; match each claim to observed evidence under the project's verification policy
---

# Verification Before Completion

Evidence precedes claims. Read the shared `verification-policy` and apply the
selected mode; this skill does not impose automated testing or TDD.

## Before a claim

1. Identify the exact claim and the observation needed to establish it.
2. Perform the authorized check on the relevant revision/environment, or locate
   existing evidence and confirm it still applies to that revision and claim.
3. Read the actual result, including failures and limitations.
4. Report only what that evidence establishes. Mark unavailable checks pending.

| Claim | Evidence needed |
|---|---|
| Build succeeds | Successful build result for the changed source |
| Lint passes | Relevant linter result |
| Bug fixed | Original symptom exercised after the fix, with observed result |
| Agent-performed UAT passed | Scenario, actions, expected and observed outcomes, environment |
| User accepted | Actual user acceptance, not the agent's inference |
| Automated checks pass (when selected) | Actual results from the named checks |
| Implementation complete | Reviewed changes against the agreed scope; distinguish acceptance status |

A passing build does not establish runtime behavior. A pending UAT gate does
not prevent handing off implemented work; it prevents claiming acceptance.
Never generate an assertion script just to satisfy a completion template in
UAT-only mode. Never fabricate evidence or hide a failed required check.

When receiving delegated work, inspect the diff and the reported evidence.
Rerun a check only when a material change, gap, or concern justifies it. Keep
implementation, static checks, runtime observations, and acceptance distinct.
