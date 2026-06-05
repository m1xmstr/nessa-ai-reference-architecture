# AAP Example

This example shows a small Ansible Automation Platform style workflow for collecting a platform health snapshot.

Use it as a starting point for:
- daily cluster health collection
- route checks
- pod status snapshots
- pre-demo validation
- scheduled maintenance that should not run as a long-lived EDA activation
- daily feedback or hotword sweeps that summarize evidence instead of launching one job per note

Example playbook:

- `nessa_ai_health_snapshot.yml`

## Scheduled Sweep Pattern

For routine collection, prefer an AAP schedule:

```text
Daily sweep schedule
  -> select current pods/endpoints
  -> collect route/log/database evidence
  -> record parked vs awake environment state
  -> send one bounded summary
```

A scheduled sweep should tolerate intentionally parked non-production environments. If staging is scaled down on purpose, the sweep can record `parked` and continue. A different readiness audit can fail when staging is supposed to be awake but has no ready pod.

## EDA Watcher Pattern

For explicit events, use EDA to launch a named AAP template:

```text
Webhook event
  -> EDA watcher with a business-facing name
  -> bounded AAP triage template
  -> summary and audit trail
```

Keep watcher names tied to the event domain, such as `Alert Responder`, `Release Gate`, or `Hotword Watcher`. Avoid names that describe the tool that created the automation.

After refreshing EDA activations, verify:

- the activation is running
- the Service selector points at the current activation pod
- the Route points at the current Service
- retired routes and services are removed
- a safe proof event launches the expected Controller template

For the public-safe division between AAP scheduled jobs and Event-Driven Ansible activations, see [../../docs/48-red-hat-platform-deep-dive.md](../../docs/48-red-hat-platform-deep-dive.md).
