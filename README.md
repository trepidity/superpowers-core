# Superpowers Core

[Maintained fork](https://github.com/trepidity/superpowers-core) of Superpowers: planning, debugging, implementation,
review, and delivery follow the project's verification policy. Automated test
authoring is optional. See [UPSTREAM.md](UPSTREAM.md) for version and attribution.

## Packages

- **superpowers-core**: the root plugin, with skills under `skills/`.
- **superpowers-testing**: optional techniques under `optional/testing/skills/`.
  Core does not load this module. Installing it does not select automated mode.

The shared **verification-policy** skill in the skills repository owns UAT-only,
automated, and hybrid selection. Install that shared skill through the shared
repository's mirror installer before using this plugin. The core adapter at
`skills/using-superpowers-core/references/verification-policy.md` describes the
integration and missing-policy behavior; it does not duplicate the policy.

## Local installation

From this checkout, after installing the shared policy:

```sh
codex plugin marketplace add .
codex plugin add superpowers-core@superpowers-local
claude plugin marketplace add .
claude plugin install superpowers-core@superpowers-local --scope user
```

Disable the stock Superpowers plugin in every applicable scope. In Codex,
its existing `[plugins."superpowers@openai-curated-remote"]` setting must remain
`enabled = false`. In Claude, use `claude plugin disable` with the installed
stock plugin ID and applicable user/project scope.

Start fresh sessions after installation. Inspect a UAT-only feature, a UAT-only
bug fix, and an explicitly automated project: only the last may require test
authoring. Confirm builds and pending user acceptance are reported separately.

Install `superpowers-testing@superpowers-local` separately only when wanted.
Keep existing project tests and declared CI gates; this customization does not
delete suites or change projects' acceptance decisions.

## Maintenance

Keep source changes here, never in plugin caches. Commit reviewed changes,
refresh each host through its plugin manager, and inspect the installed version.
Use the recorded upstream commit for deliberate update reviews. Historical
upstream installation documents and scripts are not this fork's release path.
