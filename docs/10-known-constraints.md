# Known Constraints and Tradeoffs

## Compact Cluster Reality
A three-node compact OpenShift cluster can absolutely host a useful private AI application, but it has hard limits.

Expect tradeoffs in:
- control-plane headroom
- memory pressure sensitivity
- upgrade complexity when application workloads share core nodes
- storage and inference competition if boundaries are weak

## CPU-First Reality
CPU-first inference is useful but not magical.

It is best for:
- smaller models
- shorter prompts
- operational bootstrap
- product development

It is weaker for:
- long-form generation at scale
- larger context windows
- high-concurrency heavy inference

## AMD Strix Halo Reality
A Strix Halo worker can materially improve the experience, but it is not a plug-and-play miracle box.

You still need to validate:
- BIOS settings
- kernel support
- GPU device exposure
- runtime flags
- memory behavior under load

## OpenShift AI Reality
OpenShift AI adds real value, but only if you use it intentionally.

It helps with:
- model serving
- evaluation workflows
- demo credibility
- operational clarity

It does not replace:
- application engineering
- routing correctness
- rollout discipline
- clear observability

## Public Repo Reality
This repository intentionally does not expose the proprietary logic of the private application it came from.

That means some things are deliberately generalized:
- exact request routing
- advanced product logic
- customer-facing differentiation
- private operational controls

This is a feature, not a limitation.
