# Owner-Only Linked-Device MTP Preview Pattern

Last updated: 2026-05-25

This document describes a public-safe pattern for using Multi-Token Prediction (MTP) as a private linked-device inference lane.

It intentionally omits private route names, connector internals, endpoint addresses, authentication details, node names, hostnames, tokens, model cache paths, and private benchmark artifacts.

## Product Shape

MTP is best introduced as an owner/admin preview lane, not as a default model replacement.

Recommended posture:

- expose a friendly label such as `MTP Preview`
- keep it in an advanced or owner/admin selector
- keep Fast and Auto defaults unchanged until proof says otherwise
- deny child/restricted users and ordinary non-owner overrides
- hide raw model ids, GGUF filenames, hardware ids, and diagnostics from Basic UI
- show a plain fallback notice when the linked device is unavailable

The product contract matters as much as the model speed. A user should never see a dead private-compute control, a hidden route error, or a raw local endpoint failure.

## Runtime Requirements

MTP requires an MTP-aware runtime and MTP-converted model artifacts. Standard model files should not be assumed to work.

Reusable runtime checklist:

- use a runtime build that exposes `draft-mtp` support
- test a smaller practical quant before declaring the model slow
- validate draft counts separately, commonly `2` and `3`
- use a single request slot unless the runtime proves MTP concurrency support
- verify the accelerated backend is active rather than accidentally CPU-only
- record time to first token, total latency, token cadence, timeout rate, and answer quality
- compare direct linked-device API against product-gateway routing

Typical public-safe llama.cpp concepts:

- `--spec-type draft-mtp`
- `--spec-draft-n-max` tested at multiple values
- flash attention enabled when supported
- template rendering enabled when required by the model
- KV cache tuning measured rather than guessed

Exact private launch scripts, tokens, model paths, and connector routes should stay private.

## Strix Halo MTP Preview Under OpenShift

A Strix Halo class worker can host an MTP preview lane directly under OpenShift, but it should still be treated as a preview until the product path proves clean.

Reusable public-safe pattern:

- build a fresh MTP-capable `llama.cpp` runtime instead of reusing an older serving image by assumption
- verify `draft-mtp` support at runtime, not only at build time
- prove the GPU backend selected by the container, such as Vulkan/RADV, and explicitly record when a run falls back to CPU
- test the same model family, quant class, context, KV cache, template flags, and draft-count settings used by the comparison lane
- run direct runtime benchmarks before gateway benchmarks
- run product-gateway and full app-path canaries before exposing the owner selector
- keep the preview disabled when direct runtime proof passes but full app routing shows timeout, fallback, or route-contamination issues
- record owner/admin diagnostics for selected route, actual model, backend, device, first token, token cadence, latest eval p50/p95, fallback reason, stream finish reason, and persistence state

The important lesson is that "the model is fast directly" and "the product route is ready" are different proofs. An OpenShift-hosted Strix Halo MTP lane should pass both before it becomes the headline preview lane.

## Initial Strix Halo MTP Findings

The first public-safe finding is that an unfair comparison can make MTP look worse than it is.

A heavy quant-only run on the Strix Halo OpenShift path was useful as a stress datapoint, but it was not a fair comparison against a smaller, newer, better-tuned Apple Silicon MTP lane. The repeatable public-safe test standard is now:

- same model family
- comparable quant class
- same context and template behavior
- same draft-token settings
- current MTP-aware `llama.cpp`
- explicit GPU backend/offload proof
- direct runtime, gateway, and full app-path measurements

Under that standard, the Strix Halo path looked materially better with practical smaller quants and a current Vulkan/RADV-capable runtime. Direct runtime and gateway controls were promising enough to treat Strix Halo MTP as a serious owner-preview candidate, not as a failed hardware idea.

The second finding is that the product path can be the real blocker. The validated pattern was:

| Layer | Public-safe result | Lesson |
|---|---|---|
| Older heavy-quant Strix run | Too slow for owner-preview expectations | Do not judge a lane from a mismatched quant/runtime path |
| Current Strix direct runtime | Promising after matching model family, quant class, flags, and GPU backend proof | Hardware/runtime path can be viable |
| Current Strix gateway path | Promising when the gateway stays thin and fallback-free | Gateway proof is necessary but not sufficient |
| Full Nessa app path | Initially exposed routing contamination, first-token, and finalization issues | Product route isolation is the promotion gate |
| Apple Silicon MTP lane | Useful owner-only fallback/witness | A fast witness lane should not become the strategic dependency by accident |

The public architecture decision is therefore not "MTP failed" or "Strix wins." The stronger decision is:

- keep Strix Halo in OpenShift when the supported container/runtime path is good enough
- keep Mac MTP as owner-only fallback/witness, not the headline endpoint
- do not move a production OpenShift worker to an external appliance path unless the same-hardware OpenShift path is proved incapable
- do not keep large MTP runtimes artificially warm if the idle fan/noise and resource cost exceed current product value
- expose native Strix MTP only behind owner/admin preview policy after the full app route passes

## Security Boundary

A linked-device MTP lane should be private by default.

Minimum controls:

- bind to localhost or a trusted tunnel/connector path
- require authentication or signed connector access
- do not expose an unauthenticated LAN listener
- avoid logging private prompts, access tokens, endpoint URLs, and model cache paths
- require heartbeat/lease health before routing
- fail closed for non-owner route override attempts
- keep owner/admin diagnostics separate from Basic UI

## Fallback Contract

If the linked device is unavailable, the product should fall back honestly:

- do not hang the browser spinner
- do not expose raw transport errors
- route to the current approved local/native policy
- show a short user-facing notice
- record the actual backend used

The fallback answer must be a clean answer, not a stitched mix of partial linked-device output and fallback output.

## Streaming Contract

Long story and code generations need stream governance before MTP feels production-grade.

Recommended stream record:

- route category
- model family or friendly mode
- linked-device state
- first-token time
- final-token time
- token estimate
- finish reason
- whether the final message persisted
- visible completion state
- fallback reason if any

Every streamed response should complete, offer continuation, or explain why it stopped. Silent cutoffs make a fast private model feel unreliable.

## OpenShift AI Role

OpenShift AI can help even when the MTP runtime lives on a linked device.

Useful public-safe roles:

- workbenches for benchmark notebooks and analysis reports
- model registry entries for lifecycle state
- KServe or cluster-side comparison lanes
- repeatable eval pipelines
- release-gate evidence for quality, fallback, and stream completion

This keeps the private linked-device lane governed by the same platform discipline as cluster-hosted inference.

## Sideband And Throughput Claims

High-speed sideband links can improve artifact movement and reduce network friction, but direct-path claims require topology proof.

Before claiming a sideband result:

- physically map ports and cables
- isolate docks and hubs
- map OS network interfaces to each path
- run per-link and dual-link throughput tests
- document whether the path is direct, tunneled, docked, or internally shared

Do not publish sideband throughput claims from a product-gateway benchmark unless the physical topology has been proved.

## Public Lesson

The strong pattern is not "turn on MTP everywhere." The strong pattern is:

1. prove the model runtime directly
2. prove the product gateway path
3. secure the linked-device boundary
4. keep the lane owner-only first
5. prove fallback and stream finalization
6. use OpenShift AI for governance and repeatable evidence
7. promote only what the product path proves
