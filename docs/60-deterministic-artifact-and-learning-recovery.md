# Deterministic Artifact Validation and Learning Recovery

## Purpose

This public-safe pattern covers two related product-trust problems:

- an AI-generated artifact looks plausible but is structurally invalid
- a Learning session still exists while its worksheet or preview is unavailable

The common rule is that visible product state must be backed by verified data. A label, button, or thumbnail is not proof.

## Deterministic Artifact Contract

Generated code and structured files should pass a type-specific validator before the product calls them usable. Examples include an abstract-syntax-tree parse for source code and a strict parse plus schema check for structured data.

The write path should be:

1. Generate and normalize the candidate without changing meaningful content.
2. Validate structure with a deterministic parser.
3. Validate task fit separately; valid syntax can still be the wrong deliverable.
4. Apply a bounded repair only for understood failure classes.
5. Revalidate both structure and task fit.
6. Persist and display the artifact only with the resulting evidence state.

If repair fails, the UI should say so plainly and avoid presenting the artifact as verified. A safe, task-appropriate fallback is better than an unbounded repair loop.

Streaming adds one more requirement: validation must run against the complete final payload. The bytes validated, persisted, downloaded, and shown in the artifact view should agree. Transport finish states must not turn a truncated stream into a successful artifact.

## Honest Artifact UX

A useful artifact card distinguishes these states:

- structure verified and task fit verified
- structure verified but task fit needs review
- structure failed, with repair available when it is safe
- generation incomplete or blocked

Do not show a green structure badge based on model confidence, markdown fencing, filename extension, or a partial stream. Keep technical detail available for advanced diagnostics without forcing family users to interpret parser output.

## Repairing Existing Persisted Artifacts

Historical repair should be narrow and auditable. Before changing a stored response:

- create a protected backup
- verify account and conversation ownership
- guard the update with the expected record and content identity
- replace only the proven-bad artifact
- rebuild any derived search or retrieval index
- reopen the exact user route and validate the downloadable content

This prevents a cleanup job from silently rewriting a newer response or another user's data.

## Learning Material Inventory

Learning recovery starts with a relationship inventory, not a thumbnail sweep. For each session, classify every referenced worksheet or attachment as:

- present in primary storage and readable
- present in an approved cold-storage tier
- recoverable from a verified owner-scoped source
- metadata-only or irrecoverable

Check both directions: sessions referencing missing documents and documents that no longer have a valid session relationship. Never infer ownership from a filename alone.

## Safe Recovery Ladder

Use the least ambiguous source first:

1. Primary account-scoped storage
2. Approved cold storage or an immutable backup
3. A verified owner-scoped original or export
4. A unique content match supported by multiple signals

Useful matching signals include a checksum, document type, page count, dimensions, normalized text, and OCR. Names and timestamps can narrow candidates, but they should not be enough to attach a worksheet. If the match is not unique, leave it unrecovered.

For a recovered document:

- restore through a temporary path and atomically finalize it
- constrain both source and destination to expected roots
- verify checksum, readability, media type, and page rendering
- restore the account-scoped metadata relationship
- generate the preview from the recovered file
- reopen the Learning session as the owning user

A preview is derived state. Recover the source document first; do not create a decorative thumbnail that points to nothing.

## Irrecoverable Sessions

If the underlying material cannot be recovered, the product should not leave a resume card that opens a broken lesson. Follow the documented retention policy: preserve the protected recovery record, transition the session out of active views, and remove it only through the authorized archive/delete lifecycle.

The user-facing result should be a truthful clean state. Avoid dead resume links, generic placeholder thumbnails, and sessions that appear restorable when no source material exists.

## Release Proof

The release gate should cover both API truth and real browser behavior:

- new generated artifacts pass the correct deterministic validator
- malformed and truncated candidates do not receive verified status
- task-fit failure remains distinct from structure failure
- the persisted artifact matches the validated payload
- Learning rows load real previews with non-zero rendered dimensions
- preview URLs enforce the same account authorization as the source document
- recovered sessions survive reload and open the correct worksheet
- irrecoverable sessions no longer appear as resumable work
- narrow phones, tablets, and desktop show usable controls and honest states

Run these checks on the exact staged image, promote that image unchanged, and repeat the highest-risk user paths after production rollout.

## Public Boundary

Public evidence may describe validators, recovery stages, retention rules, and aggregate pass/fail behavior. It should not publish user artifacts, account or conversation identifiers, storage locations, checksums, internal routes, backup destinations, credentials, or private screenshots.
