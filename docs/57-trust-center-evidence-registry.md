# Trust Center Evidence Registry

Public-safe pattern from Nessa AI, 2026-06-20.

## Pattern

A Trust Center should not only state security, safety, privacy, and quality claims. It should also maintain an evidence registry that maps each major claim to public-safe proof.

For a private family AI product, the evidence registry is the operating layer between marketing copy and private release artifacts. It gives families, testers, partners, and support reviewers a durable way to see what was verified without exposing private implementation details.

## What The Registry Should Contain

Use structured rows that are specific enough to audit but safe enough to publish:

| Field | Purpose |
|---|---|
| Evidence ID | Stable public row identifier, such as a release-gate or proof-flow row. |
| Category | Security, family safety, data handling, product quality, public proof, or release operations. |
| Status | Active, pass, public-safe pass, limitation, or retired. |
| Release and run | Version and release-run reference that produced or last verified the row. |
| Scope | What the evidence covers. |
| Checks | Public-safe validation categories, not raw payloads. |
| Links | Public routes, policies, seeded demos, release notes, and sanitized reference docs. |
| Limitation | What the row does not prove or what is intentionally withheld. |

The registry should be rendered on the Trust Center and, when useful, exposed as a read-only JSON endpoint so release checks, support scripts, and reviewers can inspect the same source of truth.

## Public-Safe Evidence Rows

For Nessa, the initial registry categories map to the product story:

- security release gate for prompt/tool injection, account-scope fuzzing, connector replay resistance, and scan limits
- browser and automation hardening for origin controls and server-to-server boundaries
- Homework Buddy proof flow for worksheet upload, guided help, parent summary, progress memory, and anti-cheat boundaries
- Documents proof flow for private upload, organize/search, redaction, visual QA, retention, and security explanations
- Linked Devices proof flow for private compute setup, health, privacy, workload status, and fallback truth
- signed-in family setup checklist for roles, child-safe defaults, invites, parent dashboard review, export, and delete/remove controls
- safe seeded proof-flow demo mode for public walkthroughs with synthetic data only
- public Trust Center cadence and operational scorecards

## JSON Endpoint Guardrails

A public registry endpoint should be boring and constrained:

- `GET` only
- no account-specific data
- no private identifiers
- no raw test payloads
- no exploit strings
- no connector internals
- no infrastructure hostnames, IP addresses, secrets, logs, or screenshots
- no mutation semantics
- no dependence on signed-in state
- clear no-store or freshness headers if the registry is release-coupled

The JSON endpoint should mirror the visible Trust Center rows. If the page and endpoint diverge, the release should fail.

## Browser Proof

The Trust Center registry should be browser-tested in staging and production:

- desktop and mobile rendering
- visible registry heading and row count
- category labels
- representative evidence IDs
- public-safe links
- JSON endpoint status and headers
- no horizontal overflow
- no file inputs, forms, or live-action controls in the public registry
- no same-origin mutating requests during page load

This prevents the registry from becoming another static document that looks complete but is not operationally connected to the product.

## Redaction Boundary

Do not publish:

- exploit payloads
- raw prompt-injection strings
- account, family, document, connector, or device identifiers
- connector signatures, nonces, shared secrets, or job schemas
- private route names
- model-routing heuristics
- infrastructure hostnames, IP addresses, namespaces, logs, traces, or screenshots
- private source code or production test artifacts

Publish the release run, public route, evidence class, limitation, and sanitized architecture lesson instead.

## Why It Matters

Private family AI has to earn trust with proof, not vague posture language. A registry makes the Trust Center measurable. It lets a product say, "Here is the claim, here is the release that checked it, here is the public proof, and here is what we are not exposing."

That is stronger than generic AI-platform copy because it matches how families actually evaluate risk: Can I understand what this does, what it does not do, who controls the data, and where I report a problem?
