# Adding a Strix Halo Worker

## Why We Added It
The compact core-based cluster proved the application could work.

The next bottleneck was obvious:
- long-form responses were slower than we wanted
- larger models were harder to host comfortably on compact nodes
- we wanted a dedicated AI-serving lane without turning the whole cluster into a GPU lab

A single AMD Strix Halo worker was a practical next step.

## Hardware Class
Representative system:
- AMD Ryzen AI Max+ 395
- integrated Radeon 8060S-class graphics
- 128 GB unified memory
- fast local NVMe storage

## Why This Hardware Was Interesting
The appeal was not just raw CPU speed.

The real value was the combination of:
- unified memory
- better local acceleration options
- enough headroom for serious `llama.cpp` and KServe experiments
- a single-node upgrade that did not require re-architecting the cluster

## What Changed Architecturally
We treated the Strix Halo box as:
- a dedicated OpenShift worker for heavier inference
- a serving target for KServe / OpenShift AI
- a place to pin cluster-hosted AI runtimes

We did not treat it as:
- a new storage anchor
- a place to casually dump unrelated workloads

That separation matters.

## What Improved
Representative improvements from our internal benchmark set:

| Scenario | Before dedicated AI worker | After Strix Halo lane | What changed |
|---|---:|---:|---|
| long-form story generation | ~28s | ~7s to ~8s | major latency reduction |
| research-style response | ~20s | ~12s | more stable and faster |
| technical-doc response | ~28s | ~24s | smaller gain, but more predictable |
| short email / quick generation | ~3s | ~2s | modest improvement |

The exact numbers will vary by model, prompt, and runtime. The important takeaway is that one stronger AI worker created a visibly better experience for the prompts users actually notice.

## What Was Not Automatic
Adding the hardware did not instantly solve everything.

We still had to validate:
- BIOS settings
- kernel parameters
- device access (`/dev/kfd`, `/dev/dri`)
- backend runtime behavior
- memory reporting and GTT behavior
- pod placement and resource limits

## Key Technical Lessons
### 1. AMD unified-memory inference is real, but tuning matters
The platform worked, but it required validation instead of assumptions.

### 2. Keep a CPU fallback lane
A dedicated AI worker is great. A dedicated AI worker plus a fallback path is much better.

### 3. Protect the rest of the cluster
The biggest success was isolating heavy inference to the AI worker so the compact control-plane-heavy cluster stayed calm.

## Recommendation
If you already have a stable compact OpenShift cluster and want one meaningful upgrade, adding one stronger dedicated AI worker is a very rational move.

It gives you:
- better latency
- better model headroom
- a stronger OpenShift AI story
- less risk than trying to scale out several mixed experimental nodes at once
