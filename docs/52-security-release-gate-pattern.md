# Security Release Gate Pattern

Public-safe date: 2026-06-19

This note documents a public-safe release-gate pattern for private AI security. It intentionally omits private hostnames, routes, source code, prompts, account identifiers, credentials, digests, connector protocols, production proof artifacts, and operational runbooks.

## Purpose

A private family AI platform should not treat security as a one-time audit. Security controls need to become release blockers with replayable evidence, the same way a product release blocks on version truth, browser proof, and workflow canaries.

The public pattern is to turn high-risk private-AI abuse classes into a small, focused release gate:

- prompt and tool injection across uploads, web content, email-like content, device text, and agent artifacts
- cross-account identifier guessing for chats, documents, devices, family records, and workspaces
- replayed linked-device connector traffic
- excessive Smart Home discovery scans

## Prompt And Tool Injection Pack

Treat every external content source as untrusted data, not authority.

Useful gate cases include:

- hidden instructions inside uploaded or retrieved content
- requests to ignore product policy or reveal hidden instructions
- requests to expose tokens, logs, private routes, account data, or device data
- malicious tool requests disguised as routine help
- attempts to make connected agents send messages, write files, run shell commands, or control devices without explicit permission

The gate should assert both answer behavior and action behavior. A safe answer is not enough if a hidden tool call still fires. A blocked tool call is not enough if the model reveals private state while refusing the action.

## Cross-Account Identifier Fuzzing

Identifier fuzzing protects against a common private-product failure: one authenticated user guessing or replaying another user's object id.

The release gate should fuzz:

- chat and thread identifiers
- document and upload identifiers
- family and household identifiers
- linked-device identifiers
- Smart Home device identifiers
- agent workspace, job, or artifact identifiers

The expected result is not just `404`. The product should avoid existence leaks, keep responses consistent, and prove that unauthorized identifiers do not hydrate UI state, model context, downloads, jobs, or device actions.

## Connector Replay Protection

Linked Devices are private compute endpoints, not trusted appliances.

A public-safe connector replay pattern includes:

- account-scoped device authorization
- short-lived signed request metadata
- one-time nonces stored server-side for the replay window
- rejection of stale, malformed, invalid, or reused requests
- clear connector-side failure behavior so the device retries safely without leaking secrets

The public docs should not publish connector header names, signing strings, token formats, queue schemas, pairing flows, or exact timing. The reusable lesson is that bearer authorization alone is not enough for a private compute connector once requests can be captured or replayed.

## Smart Home Scan Rate Limits

Smart Home discovery scans can touch local networks, so they need tighter release proof than ordinary read-only UI calls.

Public-safe gate expectations:

- require the signed-in account to have Smart Home ownership or setup permission before discovery
- limit scans by account
- limit scans by source network identity where possible
- return a retry signal instead of starting another scan when the limit is hit
- apply the limit before network discovery begins
- keep monitoring and discovery separate from device control

The user experience should be calm: if scanning is limited, explain that discovery is cooling down and preserve existing known-device state.

## Release Evidence

A focused security release gate should produce a small artifact with:

- pack version and required categories
- test file or scenario names
- pass/fail status
- runtime version under test
- staged artifact identity in the private run record
- sanitized browser or API proof where relevant
- explicit limitations, especially if a broad release-truth suite was not run

Public docs should describe the evidence categories, not publish private logs, screenshots, account fixtures, connector payloads, or live runtime identifiers.

## Promotion Rule

The gate should be bound to the exact staged candidate. If the security pack passes on one candidate and a different artifact is promoted, the proof no longer describes the release.

For emergency or focused security releases, it is acceptable to run a narrower gate when the scope is clearly documented, but the run record should say that the full broad suite was skipped. Do not let a focused pass imply broader product proof that was not performed.

## Go-To-Market Lesson

Security stands out when it is productized as trust:

- private routes fail closed instead of silently falling back
- connected devices prove freshness
- family and household identifiers do not cross accounts
- Smart Home discovery is bounded
- adversarial content cannot grant itself tools
- release docs say what was actually proven

That is more defensible than claiming "secure AI" in marketing copy without release evidence.
