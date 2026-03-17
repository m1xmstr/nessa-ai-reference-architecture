# OpenShift AI and AAP

## Why OpenShift AI Was Worth Adding
Once the base application was stable, OpenShift AI became valuable for two reasons:
- it gave us a first-class model-serving story with KServe
- it made demos and platform discussions much more credible

## How We Used OpenShift AI
We treated OpenShift AI as the enterprise model-serving layer, not as a replacement for the whole application.

The pattern was:
- the application still owns UX, auth, routing, and product behavior
- OpenShift AI owns a dedicated serving lane for cluster-hosted models

## KServe Lessons
What worked well:
- RawDeployment mode for controlled model serving on a dedicated worker
- internal OpenAI-compatible endpoints
- explicit node placement for the serving workload
- clean separation between app plane and inference plane

What mattered operationally:
- confirm the InferenceService is `Ready`
- verify the service from inside the cluster, not just from the UI
- keep a notebook or validation workspace available for demos and sanity checks

## Notebook / Demo Workspace Pattern
A small notebook workspace added a lot of value.

It gave us a way to demonstrate:
- public app health
- internal serving endpoint health
- simple evaluation prompts
- latency comparisons

The lesson is simple: a notebook becomes much more useful when it is pre-seeded with a practical script instead of being an empty Jupyter environment.

## AAP Role
AAP was most useful as an operations automation layer around the AI platform.

Good early workflows:
- cluster and app health snapshots
- route and deployment verification
- model-serving readiness checks
- pre-demo validation jobs

This was a better use of AAP than trying to force it into application inference logic.

## Recommendation
If you already have OpenShift AI and AAP available, use them to strengthen the platform story:
- OpenShift AI for serving and evaluation
- AAP for repeatable operations

That combination is more compelling than either product standing alone.
