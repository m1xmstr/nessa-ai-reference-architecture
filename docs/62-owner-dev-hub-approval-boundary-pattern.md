# Pattern 62 — An In-Product Dev Hub With A Hard Approval Boundary

Status: reference pattern (describes a design staged for the private product;
sanitized for public reuse).

## Problem

Owner/operators increasingly drive development through conversational AI
sessions ("make this change, update the docs, ship it"). Embedding that
loop directly inside a family product is attractive — the owner can steer
from the same UI their family uses — but naively giving a chat surface
shell, source, or cluster access inside a production web app is an obvious
way for things to go off the rails.

## Pattern

Split the loop into two lanes with a human approval boundary between them:

1. **Propose lane (inside the product).** An owner-only surface accepts
   plain-language briefs and turns them into *structured work orders*:
   summary, likely code areas, the house shipping checklist, risk notes, an
   explicit revert path, and the list of authoritative documents that must be
   updated. The propose lane is read/propose-only by construction — no shell,
   no source edits, no infrastructure clients, no credentials in scope.
2. **Execution lane (outside the product).** A separate operator/agent
   session picks up *approved, exported* work orders and executes them under
   the normal release discipline (staging first, real-browser proof,
   exact-digest promotion, docs, push).

## Rules that make it safe

- **Identity gate on every route**: interactive owner/admin session required;
  machine/API-key sessions refused even with valid credentials.
- **Feature flag kill switch** that removes all routes, plus a single state
  directory so the whole feature can be inspected, backed up, or deleted
  in one move.
- **Append-only audit journal** (JSONL) recording every view, brief,
  decision, and export with actor, UTC timestamp, and content hashes —
  verbose by design, because the audit trail is the product here.
- **State machine with refused transitions** (draft → planned → approved /
  rejected → executed); anything else returns an error rather than being
  silently coerced.
- **The work order embeds the discipline**: the shipping checklist and the
  required-docs list ride inside every exported order, so the execution lane
  cannot "forget" the process even when it is an AI agent.

## Why not let the in-product lane execute directly?

Because the failure mode is asymmetric. A propose-only lane that breaks
produces a bad markdown file; an execute-capable lane that breaks produces a
production incident inside the same process that serves families. Execution
capability should be added, if ever, one narrowly-scoped verb at a time
(e.g., "draft a docs PR") — each with its own proof run — rather than as a
general capability.

## Reusable takeaway

"Chat-driven development" inside a product is fine when the product's chat
can only *write intentions down*, and only a separately-credentialed lane can
*make them true*. The approval boundary is a file format plus a human click —
cheap to build, easy to audit, and trivial to revert.
