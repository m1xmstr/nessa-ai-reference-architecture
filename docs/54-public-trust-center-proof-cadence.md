# Public Trust Center and Proof Cadence Pattern

Date: 2026-06-19

This public-safe pattern documents how a private family AI product can make trust reviewable without exposing private source code, account data, connector internals, prompts, secrets, or production topology.

## Positioning

The clearest public position is:

> Private AI for family life.

That is more defensible than a generic AI platform claim. It narrows the product promise to workflows families understand:

- learning and homework help
- private document handling and redaction
- linked-device private compute
- family roles and controls
- visible security and retention boundaries

## Trust Center Shape

A real Trust Center should bring the following into one public route:

- security posture
- family safety
- data handling
- retention controls
- reporting paths
- changelog
- public proof cadence

The Trust Center should link to live policies, release notes, support/reporting, and sanitized architecture notes. It should not expose fake upload controls, fake checkout, placeholder plan buttons, private run artifacts, or unauthenticated demos that imply access to private user data.

## Proof Flows

Public launch proof works better when it is built around three complete flows instead of a broad feature list:

1. Homework Buddy
   - worksheet upload
   - problem mapping
   - step-by-step help
   - parent-safe summaries
   - progress memory
   - teach-first anti-cheat boundaries

2. Document redaction and organizer
   - private upload
   - organize/search
   - redaction readiness
   - visual QA
   - retention controls
   - clear security explanations

3. Linked Devices private compute
   - simple setup story
   - health status
   - privacy state
   - workload status
   - honest fallback when private hardware is unavailable

## Public Proof Cadence

Each material release should leave public-safe proof in at least one of these places:

- user-facing release notes
- Trust Center or policy update
- sanitized architecture note
- measured security or quality gate note

The private release record can contain deeper evidence, but the public artifact should keep only reusable lessons and sanitized proof shape.

## Security Gate Categories

For private AI products, the Trust Center should explain that security is broader than model content filters. Public-safe categories include:

- prompt and tool injection
- cross-account identifier fuzzing
- connector replay protection
- browser-origin enforcement
- Smart Home or network scan rate limits
- data retention and delete/export controls
- audit-friendly support/reporting paths

## Conversion Boundary

The public conversion path should be obvious and truthful:

- Try free account
- Request Pro
- Family setup after sign-in
- Support/reporting
- Public reference architecture

If checkout, plan upgrade, upload, invite, or device setup is not available to the current visitor, the public page should route to the correct real path rather than showing a dead control.

## Redaction Boundary

Do not publish:

- private source code
- production account ids or private object ids
- raw logs
- Secure Connector internals
- prompt chains
- proprietary lesson-state logic
- production hostnames, IP addresses, routes, tokens, secrets, or configmaps
- screenshots containing private customer data

Public docs should describe the pattern, not the private recipe.
