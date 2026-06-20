# Safe Seeded Proof-Flow Demo Mode

Public-safe pattern from Nessa AI, 2026-06-20.

## Pattern

A public product demo should prove the intended workflow without pretending to run live private actions.

For private family AI, the safest launch demo is a seeded, synthetic, read-only proof-flow mode. It lets a visitor inspect the product story for Homework Buddy, Documents, and Linked Devices while keeping real upload, invite, connector, scan, payment, and account-write actions behind sign-in and normal product controls.

## What The Demo Should Show

Use a small number of proof flows that match the launch story:

| Flow | Public-safe demo content |
|---|---|
| Homework Buddy | synthetic worksheet, guided steps, teach-first boundary, parent summary, progress memory |
| Documents | synthetic upload card, organize/search tags, redaction/QA checklist, retention setting |
| Linked Devices | setup outline, health state, privacy boundary, workload status |

The demo should make the workflow concrete enough for a family, tester, partner, or support reviewer to understand what the real signed-in product does.

## Safety Contract

Seeded demo mode should have an explicit public contract:

- synthetic data only
- read-only UI
- no live file upload
- no persistence
- no connector dispatch
- no device claim
- no network scan
- no invite or message send
- no account write
- no payment action

Visitors should never have to guess whether a demo action is real. Conversion actions should route to sign-in, account creation, Request Pro, support, or the relevant real product surface.

## API And Browser Guardrails

If the seeded demo has a public data endpoint, keep it boring:

- `GET` only
- no user-specific data
- no cookies required
- no private ids
- no secret-bearing payloads
- no mutation verbs
- no client-side upload controls
- no form fields that imply live work
- cache policy that matches the desired freshness and privacy posture

Browser demo code should handle tabs, steps, and playback state locally. It should not use local storage for private-looking state, send beacon telemetry as proof, or call mutating same-origin routes.

## Trust Center Fit

Seeded demos belong next to the Trust Center, not as a detached marketing novelty.

The Trust Center can explain that the demo uses synthetic data and that real family documents, learning progress, invitations, and linked-device work happen only after sign-in under normal product controls. That distinction makes the demo stronger: it proves the workflow story without weakening the privacy story.

## Release Proof

A safe seeded-demo release should verify:

- desktop and mobile rendering
- no horizontal overflow
- exactly the intended proof-flow count
- the public seed contract
- absence of upload inputs and forms
- no same-origin mutating requests during demo interaction
- Trust Center, sitemap, and launch-page links
- production route proof after exact-artifact promotion

State any skipped broader gates clearly. A seeded demo normally does not require inference warmth unless it changes routing or model behavior.

## Redaction Boundary

Do not publish private product source, proof screenshots containing account data, Secure Connector details, exact internal routes, private identifiers, account state, network topology, or exploit payloads. Public docs can explain the pattern, the public demo route, and the release discipline without revealing the private implementation.
