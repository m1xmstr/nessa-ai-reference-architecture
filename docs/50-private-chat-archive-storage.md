# Private Chat Archive Storage Pattern

Private AI products need a real conversation lifecycle. A chat history list that only grows forever becomes noise. A delete button that physically purges data immediately can create accidental loss. An archive control that only hides rows in the UI is worse because users cannot reason about what happened.

The public-safe pattern is to make thread state explicit:

- **Active**: the normal working set shown by default.
- **Archived**: intentionally hidden from the active working set, but still restorable.
- **Deleted**: removed from normal product surfaces and excluded from quota totals where appropriate.
- **Hard-deleted**: reserved for explicit retention, cleanup, legal, or administrative flows.

## Product Contract

Users should be able to trust the history surface without learning database details.

The default view should show active work only. Archived work should move to a clearly labeled view with a restore action. Delete should mean "remove from my visible product history" unless the product explicitly says the data is being permanently erased. A permanent purge path can still exist, but it should be narrow, auditable, and intentional.

The UI should avoid fake controls:

- Do not show Archive unless the row will leave Active.
- Do not show Restore unless the row will return to Active.
- Do not leave archived rows in active counts.
- Do not let soft-deleted rows appear in search, resume, context, or recent-history surfaces.
- Do not use a bulk delete label that hides which view is being affected.

## Storage Contract

A private AI archive foundation should be simple enough to reason about and hard to misuse:

- Store state as timestamps or equivalent immutable state markers, not only client-side flags.
- Index active and archived query paths separately.
- Keep archived messages and public/private opaque thread URLs intact.
- Exclude soft-deleted rows from normal read, message, context, and title-update paths.
- If a user continues an archived thread, either restore it explicitly or apply a documented unarchive-on-continue rule.
- Keep hard-delete as a separate helper from user-visible delete.

This structure supports privacy and product quality at the same time. Users can clean up their workspace without losing recoverable work, while operators still have clear retention and cleanup boundaries.

## Validation Pattern

A useful archive release should prove the lifecycle, not just the schema.

At minimum, validate:

- create a conversation
- list it in Active
- archive it
- prove Active count drops and Archived count rises
- open the Archived view
- restore it
- prove Active count rises and Archived count drops
- soft-delete it
- prove it disappears from normal views
- prove hard-delete is only used by explicit cleanup or retention paths

Browser proof matters because archive behavior is mostly trust and workflow. API/storage proof matters because the real failure class is often a mismatch between UI state and persisted state.

## Redactions

Public examples should not publish production thread ids, account ids, private route names, user messages, database rows, cookies, session formats, live hostnames beyond public domains, or internal storage topology. Use synthetic thread titles and generic endpoint names when explaining the pattern.
