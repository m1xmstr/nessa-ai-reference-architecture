# Repo Scope and Redactions

## Why This Repository Is Sanitized
This repository is based on a real private AI platform. It exists to share the infrastructure and operations lessons without exposing the parts that make the underlying product proprietary.

## What We Intentionally Do Not Share
We do not publish:
- production domains, routes, IP addresses, cluster names, or node hostnames
- secrets, certificates, tokens, OAuth details, or provider credentials
- proprietary routing logic and task-selection heuristics
- private multi-device scheduling or compute-sharing behavior
- prompt templates, agent framework internals, or product-specific automation logic
- encryption design, tenant separation internals, or customer data workflows
- billing rules, tier logic, or private business metrics

## What We Do Share
We do share:
- cluster topology patterns
- deployment and rollout approaches
- storage and observability lessons
- CPU-first operating constraints
- what changed after adding a dedicated AI worker
- sanitized example manifests and automation
- realistic next-step hardware guidance

## Naming Conventions in This Repo
The examples in this repository use generic names such as:
- `private-ai-app`
- `ai-web`
- `ai-inference-gateway`
- `ai-worker-1`
- `control-plane-1`

These are placeholders, not our real production resource names.

## How to Use This Repo Safely
Treat these materials as:
- a reference architecture
- an operations playbook
- a design pattern catalog

Do not assume the example manifests are drop-in production artifacts. They are meant to be adapted to your cluster, storage classes, security requirements, and hardware.
