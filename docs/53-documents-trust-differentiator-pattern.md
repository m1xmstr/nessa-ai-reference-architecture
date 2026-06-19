# Documents Trust Differentiator Pattern

## Purpose

Documents are one of the fastest ways for a private AI product to earn or lose trust. This pattern describes a public-safe way to make document handling feel accountable: private upload, organize/search, redaction, visual QA, retention controls, and security explanations all need to be visible from real document state.

This document intentionally omits product source code, private routes, storage paths, account data, prompt chains, live hostnames, private screenshots, model routing details, connector internals, and proof artifacts.

## Product Principle

A document workspace should not just say "your files are private." It should show what is true for the selected file:

- where the file is scoped from a user perspective
- whether it can be searched and categorized
- whether redaction preview/export is available
- whether OCR or visual extraction needs review
- what retention/delete controls actually do
- whether uploaded text is treated as untrusted reference material

The trust story should be part of the workflow, not buried only in legal copy.

## User-Facing Trust Center

A practical Documents trust center can use six plain-language cards:

| Card | What It Should Explain | What It Must Avoid |
|---|---|---|
| Private upload | The file belongs to the signed-in account and is stored in account-scoped document storage. | Raw storage paths, bucket names, internal user ids, or implementation secrets. |
| Organize and search | Search uses filenames, summaries, categories, readable text, and safe extracted fields. | Claims that every image has perfect OCR or that all file contents are understood. |
| Redaction | Preview masks common sensitive fields and export is available only when readable text exists. | Dead download controls or claims that redaction is legally complete. |
| Visual QA | OCR, preview, and extraction quality are visible so users know when to manually review a file. | Hiding low-confidence extraction behind a generic "processed" label. |
| Retention controls | Delete, cleanup windows, and protected-learning-material behavior are stated plainly. | Saying files are instantly or permanently deleted unless the backend proves that. |
| Security boundary | Uploaded text is reference material and cannot override system, safety, or tool rules. | Exposing prompt-injection test payloads or internal safety prompts. |

## Metadata Boundary

Document APIs often need to return helpful metadata to the owner. That metadata should be deliberately allowlisted.

Good owner-visible metadata:

- file type, size, title, category, upload/update timestamps
- extraction status, OCR confidence/quality, worksheet or document profile labels
- safe generated-asset metadata such as the user's own source prompt when it is needed for continuity
- redaction readiness and user-facing retention labels
- high-level processing status such as "standard path" or "scratch worker used"

Metadata that should not be client-visible:

- raw filesystem paths, object-storage references, internal bucket names, or NAS paths
- uploader IP addresses, emails, raw audit records, or private account identifiers
- raw file hashes, prompt hashes, output hashes, internal request ids, or replay/security nonces
- connector secrets, tool tokens, or private routing decisions

The public pattern is to expose a stable trust object and a sanitized extracted-data object. That makes the UI honest without turning the document detail endpoint into a debug dump.

## Redaction And Sharing

Redaction is a trust feature only when the user can inspect the result before sharing.

Recommended behavior:

- show a redaction preview before export
- make export availability depend on readable text
- show a clear fallback when the original file or inline preview is unavailable
- log/audit redaction preview/export server-side without exposing audit internals in the client
- keep the original file download and redacted export as separate controls

Redaction copy should avoid overclaiming. A family AI product can say it masks common sensitive fields; it should not imply compliance-grade legal review unless that has actually been designed and validated.

## Visual QA

OCR and vision extraction are probabilistic. The user should see that.

Useful states:

- text ready
- OCR limited
- no readable text
- image analysis pending
- preview unavailable because raw bytes are no longer present

The UI should give the user an obvious next action: review the original, upload a clearer copy, use manual redaction, or delete the item.

## Retention And Delete Truth

Retention copy must match the backend.

Public-safe checks:

- delete removes the active document record and stored file when present
- restore is not promised unless a restore flow exists
- protected learning/homework materials can have different raw-file cleanup behavior
- cold-storage or second-copy status can be summarized without exposing private paths or checksums
- document counts after delete agree across list, search, home, and API views

Do not use privacy copy as a substitute for product truth. If a file can remain in backups, cold storage, or audit trails for a defined window, say that in policy-level copy.

## Security Boundary

Documents can contain malicious instructions. A private AI product should treat uploaded content as untrusted reference text.

Release proof should include cases where documents try to:

- override system instructions
- request tool use
- reveal secrets
- claim to be higher-priority policy
- reference another account's document id

The expected behavior is simple: the model may summarize the document content, but the document cannot change system policy, bypass family roles, call tools, or cross account boundaries.

## Validation Checklist

Before promoting a Documents trust release:

- upload accepted file types through the real browser path
- reject unsupported and oversized files with clear copy
- reload and prove list/detail/search state persists
- verify redaction preview masks obvious sensitive fields
- verify redacted export downloads when readable text exists
- verify original download and inline preview controls are hidden or replaced when unsupported
- verify generated assets keep continuity metadata without leaking prompt/output hashes
- verify API responses include the trust object and hide storage/audit internals
- verify delete removes the document from list/detail/search and reports restore truth
- run prompt/tool-injection document cases as part of release proof
- run production smoke with disposable data and clean it up

## Public Demo Guidance

Use synthetic documents only. Good demo files include:

- a fake receipt
- a fake school worksheet
- a fake medical or insurance letter with example-only sensitive fields
- a generated image with a harmless prompt

Never use private customer files, live proof screenshots, private logs, or exact storage traces in public demos.

