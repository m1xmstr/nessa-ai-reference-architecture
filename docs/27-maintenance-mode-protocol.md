# Maintenance Mode Protocol

## Purpose

This document captures public-safe maintenance-mode patterns from Nessa AI. It does not publish private routes, production namespaces, hostnames, IP addresses, account data, notification channels, internal scripts, credentials, or live run artifacts.

The core lesson is simple: maintenance mode is a product state, not just an operator note. If users can see it, the app metadata, app banner, cleanup logic, and owner notifications should all agree.

## Maintenance Start Contract

A maintenance start should include enough context for a user, owner, or operator to understand why the state exists.

Required fields:

- reason
- affected surfaces
- expected user impact
- owner or run id
- planned closeout condition
- start time
- stable maintenance record id
- source of truth

The user-facing banner can stay short. The detailed context should still be available to diagnostics, owner notifications, and accessibility metadata.

## Maintenance Clear Contract

A maintenance clear should be treated as a closeout, not a casual "all good" message.

Required fields:

- original start reason
- matching record id, run id, or start time
- duration
- closeout proof
- whether users were impacted
- current version truth
- health status
- owner action needed, or "No action"

If the system cannot find matching start context, the clear should be blocked or downgraded to an owner-only diagnostic. Sending "maintenance cleared" without the original reason and proof trains owners to distrust operational messages.

## Metadata And Banner Agreement

The application metadata endpoint should expose a structured maintenance payload, not only a boolean.

Useful public-safe shape:

```json
{
  "maintenance": {
    "active": true,
    "message": "Maintenance: model-cache benchmark in progress",
    "reason": "model-cache benchmark",
    "affected_surfaces": ["chat", "image generation"],
    "expected_user_impact": "responses may be slower",
    "owner_run_id": "run-123",
    "planned_closeout_condition": "clear after health and latency proof",
    "started_at": "2026-01-01T12:00:00Z",
    "record_id": "example-record",
    "source": "config"
  }
}
```

The app banner should render from the same payload. Cleanup should fail if metadata says maintenance is on while the banner is hidden, or if a banner says maintenance is on while metadata says it is off.

## When To Use Maintenance Mode

Maintenance mode is appropriate before user-visible or performance-sensitive operational work such as:

- storage layout changes
- model-cache path moves
- repeated routing or transport benchmarks
- direct-attached high-throughput link tests
- slow model migrations
- planned dependency maintenance that may affect response time

It should not be used for ordinary copy changes, routine UI polish, or harmless background docs work unless user-visible latency or outage is likely.

## Owner Notification Pattern

Owner notifications should be sparse and contextual.

Start message should state:

- what changed
- why it matters
- expected impact
- owner action, if any
- planned closeout condition
- proof or run id

Clear message should state:

- original reason
- duration
- closeout proof
- whether users were impacted
- current health/version truth
- owner action, if any

Repeated or non-actionable maintenance updates belong in a run digest, not in every push channel.

## Cleanup Guardrails

Run cleanup should check:

- metadata maintenance state
- banner-visible state when practical
- runtime environment or config state
- whether an active maintenance record is older than expected
- whether a clear notification has matching start context

A run should not close cleanly when maintenance is stuck on, silently off while a banner is visible, or cleared without proof.

## Public Boundary

This repo intentionally omits:

- exact production route names and namespaces
- private run ids, record ids, account identifiers, and notification channels
- private scripts, cluster credentials, and config values
- private screenshots and runtime payloads

The reusable pattern is the discipline: maintenance mode should have a lifecycle, a source of truth, clear user impact, and proof-based closeout.
