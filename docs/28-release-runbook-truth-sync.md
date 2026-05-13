# Release Runbook Truth Sync

## Purpose

This document captures public-safe release-documentation patterns from Nessa AI. It does not publish private runbooks, production route names, internal artifact payloads, account data, screenshots, credentials, or private repository history.

The core lesson is simple: a release is not closed just because production is healthy. The active handoff docs, run history, release runbook, artifact index, and public stewardship record also need to agree.

## Active Docs As A Gate

Treat operator docs as a checked release surface.

Minimum active-doc set:

- current handoff
- daily/current summary
- platform state
- executive state
- run history
- release runbook
- relevant domain runbooks
- artifact index
- public reference stewardship record

The gate should fail if these docs disagree on the current production version, staging version, parked state, latest runtime run, latest docs/system run, or promoted image digest.

## Public Repo Stewardship

For each material run, record whether the public reference repo was updated.

Useful format:

- public repo update: yes/no
- reason
- public-safe commit id when yes

Not every private change belongs in a public repo. Security-sensitive internals, account behavior, exact routing logic, private screenshots, and production payloads should stay private. The important part is to make the decision explicit.

## Run History Hygiene

Run-history ledgers are high-trust operator surfaces. Avoid vague placeholders in final rows.

Good rows include:

- run name
- released version or unchanged runtime truth
- final private commit or truthful non-runtime marker
- exact digest when a runtime shipped
- public repo update decision
- artifact path
- important limitation

If an older historical row cannot be backfilled exactly, mark that truth explicitly rather than leaving an accidental template token.

## Artifact Index

An artifact index should link the important proof pack for the current run:

- release gate report
- browser screenshots or videos when used
- cleanup result
- run artifact summary
- public reference commit, when any

The index should be compact. It is a pointer map, not a second copy of every proof file.

## Public Boundary

This repo intentionally omits:

- private route names and namespaces
- private artifact JSON payloads
- account identifiers and test credentials
- notification channel ids
- production logs
- private git remotes and internal commit metadata beyond sanitized examples

The reusable pattern is the discipline: docs should be checked by automation, not trusted by memory.
