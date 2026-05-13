# NVMe Scratch Worker Pattern

This note documents a public-safe pattern for using a node-local NVMe drive to accelerate heavy document, OCR, vision, redaction, and generated-asset post-processing work while keeping canonical artifacts on durable shared storage.

The important design choice is separation of concerns:

- web replicas stay multi-node and durable-storage backed
- heavy jobs move through a dedicated worker lane pinned to the NVMe node
- inputs are staged to local scratch
- final artifacts, metadata, and audit rows return to canonical storage
- failure falls back honestly to the standard path

## When It Helps

Local NVMe scratch is useful when the workload repeatedly reads or writes large temporary files, batches image/OCR operations, or creates intermediate artifacts that do not need to live in shared storage.

Good candidates:

- document OCR
- document redaction preview/export
- worksheet photo extraction
- image generation post-processing
- large attachment analysis
- batch document/image scratch work

Poor candidates:

- tiny text files
- metadata-only operations
- ordinary chat messages
- authentication, billing, or profile writes
- anything where the bottleneck is model compute rather than file staging

## Job States

Expose job progress as a user-facing state machine instead of making the UI look stuck.

Recommended internal states:

- `queued`
- `staged`
- `processing`
- `writing_artifact`
- `complete`
- `failed_safe`

Recommended user copy:

- Preparing file
- Reading document
- Creating redacted copy
- Saving result

## Storage Contract

The scratch worker should never become the source of record.

Recommended contract:

- canonical storage keeps the uploaded original and final artifact
- scratch storage keeps only temporary staged inputs/intermediates
- redaction and deletion decisions are written to an audit trail
- scratch cleanup runs after every job
- logs contain job id, type, duration, byte counts, and hashes, but not file contents or secrets
- external provider fallback is disabled unless the user explicitly allows it

## Fallback Contract

The product should keep working if the NVMe node is unavailable.

Recommended fallback behavior:

- if worker health fails, use the existing shared-storage path
- if the worker times out, mark the job `failed_safe` and return clear copy
- if a job falls back, record the reason in metadata
- owner dashboards should say "standard document path" rather than implying NVMe was used

Do not show "NVMe accelerated" unless the worker actually handled that job.

## Sideband Contract

If a workstation-to-node sideband exists, use it only for eligible payload movement. Do not put normal app or Kubernetes service traffic on the sideband.

Good sideband candidates:

- direct artifact sync
- large context transfer
- model sync
- OCR scratch payloads
- replay payloads

The sideband remains optional. A travel-safe product must continue working over the standard network path.

## Sanitized Proof Pattern

A representative deployment used a pinned worker on an AI/storage node with a local NVMe-backed scratch path. The web application continued to write canonical files and metadata to shared storage.

Observed proof from production-style fixtures:

- worksheet photo extraction improved by about 2.3 seconds versus the standard path
- receipt/image analysis improved by about 1.5 seconds versus the standard path
- a broader workflow probe showed gains for worksheet-photo and image-batch processing
- small text and generated PDF probes did not improve because worker hop overhead exceeded any storage benefit
- scratch cleanup removed staged job files after each run
- canonical artifacts remained available through the normal Documents workflow

The correct conclusion is not "everything gets faster." The correct conclusion is "OCR/image-heavy scratch workloads can benefit, tiny or CPU-bound work should stay on the simpler path."

## Operational Guardrails

Use maintenance mode before moving storage paths, changing model-cache mounts, or running repeated throughput tests that may affect user-visible processing latency.

Before closeout, verify:

- worker health
- canonical storage availability
- scratch writability and free space
- scratch cleanup
- app health
- platform storage health
- recent logs for tracebacks or 5xx errors
- staging is parked if it was only needed for proof

Do not silence platform alerts to make a dashboard look clean. Resolve the underlying condition or document the remaining advisory truth.
