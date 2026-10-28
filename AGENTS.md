# Superpowers Core maintenance

This is a customized fork. Read UPSTREAM.md and README.md for its boundaries.
The shared verification-policy skill owns verification mode. Core workflows
must not demand automated tests or load the optional testing module themselves.

For this extraction, behavior is qualified through manual acceptance scenarios.
Do not add automated behavioral tests. Existing upstream infrastructure tests
remain preserved and may be run as separate maintenance checks for changed
helpers; they do not impose test creation in consumer projects.

Preserve the original MIT license and provenance. Keep core and optional module
skill discovery roots separate. Update both Codex and Claude manifests together.
Review actual installed content and session behavior before claiming activation.
Do not publish changes upstream or use inherited publishing scripts.
