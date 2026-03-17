# The Core-Only Phase

## Goal
Before adding any dedicated AI worker, the goal was to prove that a real AI application could run on a modest OpenShift cluster and still feel usable.

## What Worked Well
### 1. Small and medium prompts were viable
CPU-first inference was good enough for:
- greetings and lightweight prompts
- short summaries
- support-style answers
- admin and platform responses
- application flow validation

### 2. OpenShift provided real operational value immediately
Even without premium inference hardware, OpenShift helped with:
- repeatable rollouts
- stable routing and ingress
- health checks and readiness gates
- PVC-backed state
- controlled restarts during upgrades
- operator visibility

### 3. The application could mature before the hardware did
This is one of the most important lessons from the project.

You do not need the final inference hardware on day one to improve:
- UX
- routing correctness
- logging quality
- database stability
- deployment discipline
- cluster hygiene

## What Became Painful
### 1. Long-form prompts had higher variance
On the compact CPU-oriented phase, longer prompts could drift into:
- 20s to 30s response times
- more noticeable latency variance
- more sensitivity to model size and token budgets

### 2. Control-plane headroom was easy to waste
When control-plane nodes also carry application workloads, sloppy limits show up fast.

Examples of bad habits we had to correct:
- oversized pod memory limits
- weak disruption budgets
- noisy periodic jobs and logs
- treating theoretical limits like live consumption

### 3. Storage and inference should not fight each other
The compact cluster tolerated mixed workloads, but it became obvious that storage and heavier inference should not be treated as the same class of workload forever.

## What We Would Tell Others
If you only have compact CPU-centric nodes today, you can still do useful work.

Focus on:
- stable app deployment
- PostgreSQL correctness
- good logs
- SSE or streaming correctness
- predictable fallback behavior
- modest model sizes
- realistic token budgets

Do not focus on:
- running the largest models possible immediately
- pretending a small homelab cluster behaves like a large dedicated AI environment

## Representative Outcome Before a Dedicated AI Worker
A realistic compact-cluster phase can still deliver:
- a useful private AI web app
- a production-like platform operations story
- valuable OpenShift experience
- a clean upgrade path to better inference later
