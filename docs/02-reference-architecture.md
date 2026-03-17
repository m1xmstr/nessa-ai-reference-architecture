# Reference Architecture

## Phase 1: Compact Core-Based OpenShift
The initial environment used three compact x86 systems running a small OpenShift cluster where control-plane and worker responsibilities were combined.

That approach worked because it optimized for:
- low cost
- realistic home-lab power and noise constraints
- operational simplicity
- enough redundancy to survive application failures and routine maintenance

## Baseline Topology
```mermaid
flowchart TB
    subgraph Cluster["Compact OpenShift Cluster"]
        CP1["control-plane-1\ncontrol-plane + worker"]
        CP2["control-plane-2\ncontrol-plane + worker"]
        CP3["control-plane-3\ncontrol-plane + worker"]
    end

    User["User traffic"] --> Web["Web / API deployment"]
    Web --> DB["PostgreSQL"]
    Web --> Gateway["Inference gateway"]
    Gateway --> CPU["CPU inference runtime"]
    Web --> ODF["ODF / persistent storage"]
    ODF --> CP1
    ODF --> CP2
    ODF --> CP3
```

## What Ran in the Core-Only Phase
A practical minimum set looked like this:
- web/UI and API service
- a lightweight background worker or operations service
- PostgreSQL
- a small inference gateway or model proxy
- a fallback model-serving runtime
- persistent storage
- routes, autoscaling, and disruption budgets

## Why This Architecture Worked Early
This shape worked well for early-stage product development because it let us:
- validate the end-user experience before buying bigger hardware
- build discipline around rollout safety and observability
- learn what application latency was due to software versus hardware
- keep operations understandable

## Where It Started To Hurt
The combined control-plane/worker design has real limits:
- memory pressure matters more because the same nodes carry both cluster services and app workloads
- large inference pods can create misleading overcommit warnings
- upgrades are more sensitive to poor PDBs and drain behavior
- storage and inference experiments can interfere with each other if not separated carefully

## Phase 2: Add One Dedicated AI Worker
After the core-only phase, we added a single stronger worker specifically for model serving.

That changed the cluster shape:
```mermaid
flowchart TB
    subgraph Core["Compact Core Cluster"]
        CP1["control-plane-1"]
        CP2["control-plane-2"]
        CP3["control-plane-3"]
    end

    AI["ai-worker-1\nAMD Strix Halo\nunified memory"]

    User["User traffic"] --> Web["Web / API"]
    Web --> Gateway["Inference gateway"]
    Gateway --> CPU["CPU fallback lane"]
    Gateway --> AI
    AI --> KServe["OpenShift AI / KServe lane"]
```

## Design Principle
Keep the core cluster stable first. Add acceleration second.

The biggest architectural win was not raw speed by itself. The win was separating heavy AI serving from the compact core nodes so the platform became easier to reason about.
