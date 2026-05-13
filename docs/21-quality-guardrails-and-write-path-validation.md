# Quality Guardrails and Write-Path Validation

## Purpose

This document captures public-safe lessons from a recent Nessa quality-hardening cycle. It does not publish private routes, source code, user data, connector internals, prompt chains, account identifiers, or routing heuristics.

The theme was simple: private AI quality is not just "the model answered." Quality includes whether the product saved the right thing, hid the right control, routed honestly, failed safely, and stayed usable on mobile.

## What Changed In The Validation Model

Nessa added a broader validation pattern around user-visible state:

- account and profile write paths
- AI preference and personalization saves
- family invitations and role state
- Learning / Homework Buddy lesson lifecycle and study-plan state
- API key creation and revocation
- two-factor setup and cancel behavior
- document upload, search, redaction, download, delete, and count truth
- chat history deletion and resume behavior
- linked-device readiness and fallback truth
- model governance and owner/admin surface gating
- mobile layout across narrow phones, tablets, and desktop
- response-quality regressions where an unrelated specialty or stale context could leak into an answer

The public architecture lesson is that these are not "nice to have" checks. They are core product checks for a private family AI platform.

## Staging-Only Write-Path Suites

The safest pattern is to prove authenticated writes in staging only.

Good staging write-path suites should:

- refuse production hosts by default
- seed disposable test users and family roles
- mask credentials, tokens, invite emails, API keys, and cookies in logs
- verify request payloads before calling a write successful
- assert non-500 responses
- require clear user-facing success and failure copy
- reload the page to prove persistence
- verify failed actions do not show misleading success toasts
- clean seeded records and uploaded files after the run
- keep screenshots and videos for failures

This pattern is reusable for OpenShift-hosted products well beyond Nessa.

## Family Controls

Family AI features need role and plan proof, not just a visible tab.

Public-safe role matrix:

- owner on an active family plan
- adult family member
- restricted or child member where supported
- free user
- guest

Important checks:

- owners can invite and cancel invites
- active family owners do not see stale create-plan prompts
- non-owners do not see owner/admin controls
- child or restricted users fail closed when state is missing
- safety mode follows the user into chat and learning flows
- disabled states explain what is needed next

## Learning / Homework Buddy

Learning is a trust surface, so the product has to prove more than answer quality.

Public-safe checks:

- a learner can start a new lesson from a question, worksheet, or both
- worksheet uploads create real linked artifacts and survive reload
- unsupported or oversized worksheets fail with concrete file-type and size guidance
- a follow-up question stays in the same lesson instead of creating a duplicate session
- save, complete, archive, and restore states persist across reload
- resume labels are accurate and do not point to stale homework threads
- study plans and study packs are saved learning objects, not decorative buttons
- owner/parent progress views are role-gated and show honest zero states
- child or restricted learners get safer defaults and cannot reach advanced model/device/admin controls through Learning
- Basic learner views do not expose raw model telemetry

The public lesson is to test Learning like a workflow. A private tutor product should not rely on "the model answered once" as its release gate.

## Documents

Document workflows need evidence across the whole lifecycle:

- accepted file types upload and survive reload
- unsupported and oversized files fail with clear copy
- documents appear consistently across home and organizer views
- summaries and search work
- redaction produces a downloadable artifact and an audit trail
- delete behavior matches the actual retention policy
- generated assets show truthful titles and file sizes
- stored count, loaded count, and displayed count agree

The public lesson is to keep prompt text as metadata when it is needed, not as a long visible filename or user-facing title.

## Linked Devices

Linked Devices are powerful only when readiness is honest.

Public-safe checks:

- friendly names appear by default
- raw identifiers stay behind an advanced details control
- offline devices are not selectable as ready
- Basic mode keeps the model selector simple
- Advanced mode may expose device/model detail for appropriate users
- child or restricted users do not see raw device/model controls
- fallback from a selected device is either clearly explained or hidden behind diagnostics when the user did not explicitly select that device

The product should never show "private device ready" just because a stale heartbeat exists.

## Security UX

Security controls need backend proof and user-safe UX.

Checks include:

- logout invalidates the server-side session, not only the browser view
- re-auth returns users to the intended page
- two-factor setup cannot half-save into a confusing state
- API and provider keys are write-only after creation
- revoked keys stop working
- owner-only endpoints reject non-owner sessions
- write endpoints have CSRF/session protections
- staging or test bypasses do not exist in production
- secrets do not appear in frontend logs, server logs, screenshots, or videos

## Model Governance And Admin Surfaces

Model governance is not a normal family-user surface. Basic users need simple mode labels, not raw model inventory or owner/admin controls.

Public-safe checks:

- Basic mode shows friendly choices such as Fast, Auto, Thinking, Gemma 4, and Extended Thinking
- owner and administrator roles can open governance tools
- adult members, free users, children, restricted users, and guests cannot reach governance by URL
- API routes return safe `401` or `403` responses instead of raw errors
- child and restricted users fail closed when role state is missing
- dangerous model actions require explicit confirmation
- promotion, rollback, actor, timestamp, target model/version, and previous state are auditable
- rollback is a supported path rather than an incident-only manual fix

See [22-model-governance-and-admin-surface-guardrails.md](./22-model-governance-and-admin-surface-guardrails.md).

## Mobile First Proof

Mobile proof is not a final screenshot after desktop is done.

Useful viewports:

- narrow iPhone SE class
- current iPhone class
- Pixel class
- tablet
- desktop sanity check

Critical checks:

- no horizontal scroll
- composer remains usable
- primary actions are visible where reasonable
- destructive controls are not too easy to tap
- legal and help pages remain readable
- drawers and tabs do not create dead empty space

## Response-Quality Guardrails

Private AI systems accumulate context: preferences, specialties, documents, linked devices, and prior chats. That context improves answers only when it is scoped carefully.

Public-safe guardrail lessons:

- saved specialties should require explicit domain signals before attaching to unrelated prompts
- code prompts should not inherit professional or hobby specialties just because generic words overlap
- structured code artifacts need deterministic validation before the UI labels them safe
- repair can be deterministic for common cases such as missing standard-library imports
- historical messages should not be silently rewritten unless the user explicitly asks for that behavior

## Operations And Advisories

OpenShift advisories and operator alerts require careful truth:

- do not silence an alert as a substitute for remediation
- prefer supported z-stream updates when available
- if mitigation is interim, say it is mitigation, not a permanent fix
- check workload impact before applying cluster-wide kernel or module changes
- document why a known alert remains active when that is the safer state

This is part of product quality because a private AI product sits on real infrastructure.

## Public Boundary

This repo intentionally omits:

- private user prompts and account data
- private routes and hostnames
- connector auth, transport, pairing, heartbeat, and job internals
- exact routing heuristics
- proprietary Learning or Homework Buddy logic
- raw OpenShift manifests and configmaps
- sensitive screenshots, videos, logs, tokens, keys, or cookies

The reusable pattern is the validation discipline, not the private implementation.
