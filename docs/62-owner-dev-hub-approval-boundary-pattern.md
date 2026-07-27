# Pattern 62 — An In-Product Dev Hub With A Hard Approval Boundary

Status: implemented reference pattern; sanitized for public reuse.

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
2. **Execution lane (outside the product).** A separately credentialed local
   worker picks up approved work orders and executes only fixed phases under
   the normal release discipline. Code changes happen in a fresh worktree
   with command-network access disabled. Staging, exact-digest production
   promotion, and rollback are separate operator approvals; none can be
   inferred from approval of the previous phase.

## Rules that make it safe

- **Identity gate on every route**: interactive owner/admin session required;
  machine/API-key sessions refused even with valid credentials.
- **Feature flag kill switch** that removes all routes, plus a single state
  directory so the whole feature can be inspected, backed up, or deleted
  in one move.
- **Append-only audit journal** (JSONL) recording every view, brief,
  decision, and export with actor, UTC timestamp, and content hashes —
  verbose by design, because the audit trail is the product here.
- **State machine with refused transitions** (planned → changes running →
  changes ready → staging verified → production complete, with explicit
  failure/retry/reject/rollback states); anything else returns an error rather
  than being silently coerced.
- **Typed high-risk confirmations** for staging, production, and rollback.
  Approval to edit code is never approval to deploy it.
- **Fail-closed worker result validation**: the control plane accepts a phase
  result only when its requested phase, work-order identity, repository
  identity, changed-file policy, pushed commit, release digest, and validation
  evidence satisfy the configured contract.
- **A narrow local agent profile** that can write only inside the isolated
  run workspace and cannot read developer credentials, shell profiles,
  Kubernetes configuration, SSH keys, cloud credentials, or the worker's own
  control/release implementation.
- **The work order embeds the discipline**: the shipping checklist and the
  required-docs list ride inside every exported order, so the execution lane
  cannot "forget" the process even when it is an AI agent.

## Why not let the web process execute directly?

Because the failure mode is asymmetric. The family-facing web process should
remain a credential-free control plane: it records intent, approvals, evidence,
and audit history. The local worker owns the only execution credentials and
offers fixed verbs rather than a shell. Compromise of the web process therefore
does not create a direct source-control or cluster command path, and compromise
of an individual coding turn still cannot authorize its own deployment.

## Reusable takeaway

"Chat-driven development" inside a product is workable when the product records
intent but a separately credentialed worker can perform only explicit,
phase-scoped verbs. The durable work order, distinct human approvals, exact
release digest, protected-path policy, and append-only audit are all part of
the boundary. The useful abstraction is not “an AI with a shell”; it is a
small release state machine with an AI confined to one isolated code phase.
