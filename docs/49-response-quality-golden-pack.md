# Response Quality Golden Pack

## Purpose

This document captures a public-safe release pattern for improving AI answer quality without publishing private prompts, source code, routing heuristics, account data, cluster routes, model filenames, or production proof artifacts.

The pattern is a small, repeatable "golden pack" of user-facing tasks that must keep working before a private AI platform promotes a release.

## Why A Golden Pack Exists

General chat smoke tests are too weak for a family AI product. A model can answer one easy prompt while still failing the everyday work users rely on:

- homework explanations
- short and long creative writing
- email drafts
- code explanation and repair
- practical planning
- factual explanations
- follow-up continuity
- image or worksheet attachment handling

The golden pack turns those workflows into release checks with explicit pass/fail criteria. It is not a benchmark leaderboard. It is a product-trust gate.

## Recommended Shape

A useful response-quality golden pack should include:

- short factual answers that must avoid irrelevant specialty context
- homework and study-help prompts that must teach step by step
- writing prompts with minimum usefulness and formatting requirements
- creative prompts with word-count or completeness floors
- code prompts that require explanation, not only code blocks
- follow-up prompts that prove same-thread continuity
- attachment-adjacent prompts that ask for the missing file instead of inventing image details
- failure-mode prompts that previously regressed

Each case should record:

- workflow category
- input prompt class, not necessarily the private prompt text
- expected user-visible signals
- disallowed contamination signals
- response length or completeness floor where relevant
- route or capability class in sanitized form
- elapsed time and timeout classification
- pass/fail reason

## Quality Cleanup Loop

The release loop should be boring and repeatable:

1. Run the pack against the current production baseline.
2. Fix the highest-impact failures with targeted changes.
3. Add or update regression coverage for the failure class.
4. Run the full pack locally or against staging.
5. Deploy the exact candidate to staging.
6. Run the pack against staging and save the artifact.
7. Run the normal staging release-truth gate against the same digest.
8. Promote only the digest that passed.
9. Run the pack against production after promotion.

If the golden pack improves but still has known failures, document the exact residual risk. Do not describe the release as fully quality-clean.

## Deterministic Repair Is Allowed

Not every quality repair needs a larger model. Some common answer classes are better protected with deterministic finalization or bounded fallback:

- arithmetic checks
- one-step algebra tutoring
- standard study-plan scaffolds
- missing-attachment guidance
- code explanation shape
- email sign-off and audience tone
- story underfill or prompt-echo rejection

The public lesson is to protect user trust first. A deterministic guardrail is appropriate when the correct structure or answer class is known, testable, and safer than asking a model to rediscover it every time.

## Attachment And Vision Quality

Image, worksheet, and document flows need their own quality assertions. A healthy gate should prove:

- the upload appears in the composer or workspace before send
- the answer is based on the attached material
- unavailable or unreadable media produces honest retry/crop guidance
- worksheet context remains attached through the conversation
- stale context from a prior problem does not leak into the current answer
- production does not silently fall back to a generic chat answer when a media route is required

Vision cold-start behavior should be tested with realistic timeout budgets. If a model or runtime can fail on cold start, the release gate should catch it before production users do.

## Release Truth

A golden pack is only useful when bound to release truth:

- stage first
- record the image digest or immutable build id that passed
- promote that same artifact
- rerun the pack in production after promotion
- keep the failed baseline and final pass artifacts
- scan logs for quality-path timeouts or fallback reasons after rollout
- document known platform warnings separately from product failures

The result should be a clear before/after quality record, not a vague claim that "responses are better."

## Public Boundary

This repository intentionally omits:

- private prompt text from production users
- source code, route heuristics, and model policy
- private hostnames, digests, account ids, and screenshots
- worksheet parser internals or tutor-state machinery
- raw logs that could expose user data

The reusable pattern is the discipline: maintain a small golden pack, bind it to exact release artifacts, fix concrete failures, and make production quality measurable after every material answer-quality change.
