# Model Governance And Admin Surface Guardrails

## Purpose

This document captures public-safe patterns for exposing model governance in a private family AI product. It does not publish private routes, source code, account data, model-routing heuristics, admin identities, screenshots, tokens, or operational endpoints.

The core lesson is simple: model governance is an operations surface, not a normal family-user feature. Normal users need clear modes. Owners and administrators need auditable controls. Children and restricted members need fail-closed access.

## User-Facing Modes

Basic family users should see a small set of understandable choices:

- **Fast**: quick everyday answers.
- **Auto**: let the system choose the best normal route.
- **Thinking**: slower, deeper reasoning for harder work.
- **Gemma 4**: a named quality mode when the product intentionally exposes it.
- **Extended Thinking**: longer reasoning for qualified plans or roles.

Basic mode should not expose raw model inventory, device-specific model strings, internal route names, token counts, registry states, or owner/admin labels in the DOM.

Advanced mode can expose more technical detail, but only when the viewer is authenticated, eligible, and not a child or restricted family member.

## Governance Access

Governance surfaces should be role-gated at more than one layer:

- Hide governance entry points from normal Basic users.
- Redirect or show a branded safe page for direct URL attempts.
- Return `401` for unauthenticated API calls.
- Return `403` for authenticated users who are not owners or administrators.
- Keep child and restricted users fail-closed even when account state is incomplete.
- Reject API-key bearer tokens for interactive owner/admin management routes unless the product has a separate explicitly approved automation path.

The browser UI is not the access control boundary. Backend APIs must enforce the same rule.

## Dangerous Actions

Model changes can affect quality, cost, privacy posture, and safety behavior. Promotion and rollback flows should therefore require explicit confirmation.

Good patterns:

- Require typed confirmation or an equivalent deliberate action for production promotion and rollback.
- Treat staging promotion as a real action too, not a casual button press.
- Record actor, action, target model/version, previous state, new state, timestamp, and decision source.
- Make rollback a first-class path.
- Preserve chat defaults unless the governance action is explicitly promoted and auditable.
- Never silently promote a model because a benchmark looked good once.

## Staging Role Matrix

Governance validation should be staging-only and cover at least:

- owner on an active family or pro plan
- platform administrator
- adult family member
- child or restricted member
- free user
- guest

Useful checks:

- owner/admin can open governance.
- owner/admin registry API returns non-500.
- non-owner direct URL attempts do not expose raw errors.
- child/restricted direct URL attempts fail closed.
- guest attempts require sign-in.
- Basic model selector contains only friendly modes.
- promotion and rollback require confirmation.
- screenshots and videos are retained only for failing staging cases or sanitized proof.
- test users and family state are removed after the run.

## Audit And Reversibility

Model governance needs operational memory:

- active production model version
- previous production model version
- staging-eval state
- staging-approved state
- rejected state and reason
- production promotion actor and timestamp
- rollback actor and timestamp
- eval gate result or documented override reason

The public architecture lesson is not a specific registry schema. It is that model changes need an audit trail and a reversible path.

## Public Boundary

Public documentation can describe the guardrail pattern. It should not publish:

- private admin routes or hostnames
- exact registry payloads
- prompt chains or model-routing heuristics
- production account roles
- private model inventory beyond intentionally published examples
- screenshots, videos, logs, tokens, keys, cookies, or invite links

The reusable pattern is role gating, user-language modes, confirmation for dangerous actions, and staging proof before production.
