# AAP Example

This example shows a small Ansible Automation Platform style workflow for collecting a platform health snapshot.

Use it as a starting point for:
- daily cluster health collection
- route checks
- pod status snapshots
- pre-demo validation
- scheduled maintenance that should not run as a long-lived EDA activation

Example playbook:

- `nessa_ai_health_snapshot.yml`

For the public-safe division between AAP scheduled jobs and Event-Driven Ansible activations, see [../../docs/48-red-hat-platform-deep-dive.md](../../docs/48-red-hat-platform-deep-dive.md).
