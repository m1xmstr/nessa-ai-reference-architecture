# Release Truth And Canary Gates

## Purpose

This document captures public-safe release-gate patterns from Nessa AI. It does not publish private routes, source code, cluster hostnames, credentials, account data, screenshots, or implementation-specific test users.

The core lesson is that a staging-to-production deploy should prove the product users will actually receive, not only that a container rolled out and `/health` returned `ok`.

## Release Truth

Every production candidate should have one release truth source that agrees across the runtime and user-facing surfaces.

Useful checks:

- `/api/meta` version matches the visible footer, badge, or app-shell version.
- release notes or "what changed" metadata starts with the same version.
- legal and policy files are current and do not contain placeholders.
- the promoted production image digest is exactly the staged digest that passed validation.
- staging proof artifacts include the image digest, version, checked routes, screenshots, and failure diffs.

Version mismatches should block promotion. A release note that still points at the prior version is not harmless; it tells operators and users a different truth than the runtime.

## Legal No-JS Fallback

Legal pages should be readable without JavaScript.

Public-safe checks:

- Terms, Privacy, and Usage pages render the full legal text server-side.
- The server-rendered text matches the published source file.
- Current legal files reject placeholder tokens such as TODO-style markers, template names, and internal product notes.
- Public CDN or edge transformations must not rewrite the legal text in a way that changes the source. For example, email-obfuscation features can mutate visible policy text unless the full-text block is explicitly protected.

This matters because legal pages are trust surfaces, not app-only surfaces.

## Persona Snapshots

Private family AI products need release snapshots for more than an owner account.

Public-safe personas:

- guest
- free signed-in user
- active family owner
- adult family member
- child or restricted member

Checks should confirm that the Basic user DOM does not contain owner/admin/control-plane labels and that child or restricted users cannot reach advanced controls by URL.

## Canary Pattern

A production canary gate should be read-only.

Recommended pattern:

1. Stage the candidate image.
2. Run the full staging release gate against that exact digest.
3. Wake a production canary from the same digest.
4. Run read-only production canary checks against public routing and edge behavior.
5. Park the canary.
6. Promote the same digest to production only if the staging and canary gates passed.

The canary gate should include edge-sensitive checks such as public legal rendering, branded broken routes, and version truth. It should not create documents, invites, API keys, or billing sessions in production.

## User-Path Smoke

Useful staging checks:

- send one chat message and verify markdown stays structured after streaming
- upload a small document and verify it appears after reload
- validate family-owner state and invite availability
- validate linked-device no-device, offline, and ready states
- capture mobile screenshots for narrow phone, current phone, tablet, and desktop

The goal is to prove the high-risk user paths at release time without turning every deploy into an uncontrolled broad QA pass.

## Failure Evidence

Failed gates should produce enough evidence for a developer to fix the issue without guessing:

- exact route
- screenshot when UI is involved
- string diff when text truth fails
- API status and response class
- image digest and version
- masked logs only

Do not mark a release complete if the gate fails. Fix the issue, rerun the gate, and promote only the digest that passed.

## Secrets Boundary

Release and eval automation should not pass long-lived secrets through workflow parameters, logs, screenshots, or browser storage.

Safer patterns:

- mount cluster secrets as environment variables inside task pods
- mask tokens, cookies, invite links, API keys, and provider secrets in logs
- rotate secrets if a prior workflow or log exposed them
- delete old workflows or artifacts that contain leaked secret material
- keep public reference docs pattern-level, not route-level or account-level

## Public Boundary

This repo intentionally omits:

- private route names and exact hostnames
- real account identifiers and test identities
- secret names beyond generic examples
- screenshots or videos from private production accounts
- full gate source code and private database cleanup logic

The reusable pattern is the discipline: stage first, bind proof to the exact digest, check release truth from the user's point of view, use a read-only production canary, and make failed gates produce fixable evidence.
