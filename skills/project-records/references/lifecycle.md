# Lifecycle

Statuses: `proposed`, `accepted`, `rejected`, `superseded`, `retired`.

## Transitions

Each happens when the owner says so about that named record. The change
is the `status` key and the commit that carries it; the commit message names
the owner's act and its date. The body is not edited for a status change, so
a plan record's Completion Evidence stays `None.` until the plan is done.

- proposed to accepted
- proposed to rejected
- accepted to superseded (the replacement exists and both records carry the
  reciprocal `supersedes` / `superseded_by`)
- accepted to retired (the obligation no longer applies)
- rejected to proposed (reopened)

No other transition. An agent never sets a status on its own.

## Editing

- A new record is `proposed`.
- Editing a record keeps its status. The same knowledge unit is edited in
  the same file; a revision to an accepted record is reported in the hand-off
  so the owner sees it.
- A real replacement is a new `proposed` record with `supersedes` set; the
  old record gets `superseded_by` and stays `accepted` until the owner marks
  it `superseded`.
- `rejected`, `superseded` and `retired` records are not edited.
- Identifiers are never deleted, renumbered or reused.

Status describes a record. It never conditions work: nothing waits for a
record to become `accepted`.
