# Application Patterns on OpenShift

## Services That Mattered Most
A useful private AI application on OpenShift usually needed only a few core roles:
- web/UI and API deployment
- background or singleton worker deployment
- PostgreSQL
- inference gateway
- one or more model-serving runtimes

## Web/API Pattern
What worked for us:
- Flask-style web app
- Gunicorn with thread-based workers for streaming/SSE
- horizontal scale on the web layer only after correctness was in place

Why thread-based workers mattered:
- streaming keeps connections open longer than classic request/response APIs
- sync workers can look fine at low load and then feel broken under concurrent streaming

## Database Pattern
What mattered more than the database brand:
- moving off ad hoc local state early
- keeping application truth in PostgreSQL
- using durable PVC-backed state
- separating app bugs from storage bugs by making persistence predictable

## Inference Pattern
A strong pattern was:
1. web request arrives
2. lightweight classification happens in app logic
3. inference gateway decides which lane to use
4. CPU fallback or KServe lane answers the request

This is intentionally simpler than a highly proprietary scheduler. Simplicity helped us debug the platform while it was still evolving.

## OpenShift Primitives That Paid Off
### HPA
Use autoscaling cautiously on the web layer. It helps for bursty traffic, but only after your readiness and startup behavior are clean.

### PDB
PDB mistakes show up during upgrades, not during calm periods. Validate them before cluster upgrades.

### Route health and rollout discipline
Always verify:
- pod health
- rollout completion
- route response
- logs after the deployment

### Storage
If you use ODF on a compact cluster:
- keep node eligibility explicit
- avoid accidental local-storage discovery on nodes meant for AI experiments
- document environmental constraints such as MTU or homelab network tradeoffs

## Logging Pattern
A major quality improvement came from splitting logs into:
- user-facing application logs
- background heartbeat or keepalive logs
- infrastructure/operator logs

This matters because product debugging becomes much harder when low-value periodic noise overwhelms real errors.

## Upgrade Discipline
The upgrade path for a compact cluster is part of the product story.

We learned to validate:
- cluster operators
- MCP state
- node readiness
- PDB correctness
- post-upgrade service routes
- storage health
- application logs after cluster churn
