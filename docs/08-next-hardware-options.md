# Next Hardware Options

This section is intentionally forward-looking. These options are theory, planning, or partial evaluation only. They are not presented as production recommendations already proven by this repository.

## 1. ASUS GX10 / GB10-class systems
Potential role:
- dedicated LAN inference appliance
- strong single-box inference endpoint
- not necessarily the first OpenShift worker to add

Why it is interesting:
- high inference upside in a small package
- useful for premium serving experiments

Caution:
- better treated as a sidecar inference endpoint first than as an immediate cluster member

## 2. Dedicated GPU workstations
Examples:
- RTX 3090-class tower
- RTX 5090-class future workstation
- other single-node GPU systems with strong cooling and serviceability

Why they are interesting:
- more conventional model-serving path
- easier fit for some inference stacks
- strong upgrade path if the goal is larger models or higher concurrency

Caution:
- power, thermals, and cost all rise quickly

## 3. DGX-class systems
Examples:
- DGX Station
- DGX Spark / similar premium systems
- rack-level DGX options

Why they are interesting:
- serious enterprise-grade inference and experimentation runway
- compelling for multi-model or multi-tenant scale

Caution:
- very high cost
- needs stronger operational guardrails than a homelab or compact OpenShift cluster normally starts with
- included here as a strategic direction, not a tested recommendation from this repo

## 4. Apple Silicon sidecar systems
Examples:
- Mac Studio
- strong Apple Silicon notebooks or desktops

Why they are interesting:
- very strong perf-per-watt for some inference patterns
- useful as sidecar inference endpoints or evaluation nodes

Caution:
- not the first choice for a standard CUDA-centric OpenShift worker design
- best treated as adjacent inference capacity unless you have a clear Apple-first plan

## Recommendation Order
If you have a compact OpenShift cluster today, a sensible order is:
1. make the existing cluster operationally solid
2. add one dedicated AI worker
3. improve OpenShift AI and KServe usage
4. only then consider bigger endpoint expansion such as GB10, DGX-class, or multiple GPU workers
