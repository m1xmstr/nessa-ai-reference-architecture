# Owner Notification Trust Firewall

## Purpose

This document captures public-safe notification patterns from Nessa AI. It does not publish private routes, source code, phone numbers, chat history, account data, tokens, channel identifiers, or production alert payloads.

The core lesson is simple: owner notifications are a trust surface. If every deploy, health check, or automation step pushes a message, the owner learns to ignore all of them.

## Notification Contract

Every pushed operational message should answer five questions:

- what changed
- why it matters
- whether owner action is needed
- where the proof lives, using a run id or sanitized artifact reference
- how long the state is expected to matter

Messages that cannot answer those questions should be logged for the run digest instead of pushed immediately.

## Severity Rules

Useful severity boundaries:

| Severity | Meaning | Push behavior |
|---|---|---|
| P0 | Active production outage, security leak, unsafe live-key risk, verified life-safety alert, rollback | Page immediately |
| P1 | Failed promotion gate, customer-impacting degradation, failed critical dependency | Push once with action and proof |
| P2 | Important but bounded issue, owner should know soon | Include in run-end digest unless action is needed |
| P3 | Non-actionable progress, routine deployment state, noisy milestone | Collapse into digest |

Repeated low-severity deploy messages should not page just because automation saw another event.

## Deduplication

Release and health events need stable dedupe keys.

Public-safe examples:

- namespace
- version
- event type
- image or release digest
- gate name
- normalized decision

The cooldown window should be configurable. The important part is that the same non-actionable event does not reach the owner through Signal, Telegram, email, and NTFY repeatedly.

## Run Start, Milestone, And Finish

Run notifications should be useful but sparse.

Recommended pattern:

1. Start: send only when the run begins a real operational action or owner-visible work.
2. Milestone: send only when the milestone changes risk or owner action.
3. Finish: send a concise closeout when there is proof, version truth, digest truth, and clear limitations.

A finish message without proof is not a finish message. A maintenance-clear message without the matching start reason and closeout evidence creates false confidence.

## Release Gate Noise

Release gates are valuable, but raw gate events are often noisy.

The owner should not receive repeated messages such as "deploy complete" with unknown decision, no gate, failed health fields, and no action item. That belongs in logs or the run digest unless it points to a real owner decision.

Promotion failures, rollback events, production health failures, and version or digest mismatches should still page.

## Preview Before Send

Every notification family should have a preview command that renders the exact message without sending it.

Preview mode should include:

- selected severity
- dedupe key
- cooldown decision
- channel decision
- rendered push text
- masked proof reference

This makes notification copy testable without training owners to ignore test traffic.

## Replay Tests

Notification systems need replay fixtures. Good fixtures include:

- repeated low-severity release-gate events
- one real failed promotion gate
- maintenance start and clear pairs
- maintenance clear without a start reason
- security or secret-leak event
- weather or life-safety event after its truth gate passes

The expected outcome should be explicit: suppress, collapse, digest, push once, or page immediately.

## Public Boundary

This repo intentionally omits:

- actual Signal, Telegram, email, or NTFY routing details
- message history from private accounts
- phone numbers, chat ids, webhook urls, and channel tokens
- private run artifact urls
- production route names and namespaces

The reusable pattern is the discipline: make every pushed message actionable, dedupe repeated operational noise, preview before sending, and reserve pages for events that genuinely need owner attention.
