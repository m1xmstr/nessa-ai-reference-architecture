# Writing Follow-Up Truth and Result UI

Public-safe pattern from Nessa's writing follow-up hardening.

## Problem

One-click writing actions such as `Expand this` can look premium while failing in practice if the product spends model time, rejects the generated revision late, and then shows the original draft again. That failure is worse than a normal model miss because the user waited for a visible action and received almost no product value.

The common anti-pattern is:

- the action forces a slower route that the user did not select
- post-processing replaces or blocks the generated answer after the model has already streamed a useful draft
- the UI shows a before/after comparison that adds bulk instead of helping the user read the improved result
- a quality guard preserves the original with an apology-like preface instead of producing a materially better revision or failing honestly

## Public Pattern

Treat follow-up actions as their own product contract, not as generic chat prompts.

1. Preserve the user's selected lane unless policy explicitly denies it.
2. Carry the source draft, source request, action id, and surface kind as structured payload fields.
3. Validate the revision against action-specific expectations such as output type, source continuity, minimum expansion, and no code/report drift for story prose.
4. If validation fails, use a bounded deterministic repair before attempting a second model call.
5. If repair cannot produce a better draft, fail honestly instead of pretending the original is a successful expansion.
6. Render the result as the new artifact first. Do not make the reader compare the entire before and after by default.

## Result UI

A good writing result card should answer three questions quickly:

- What changed?
- Is it materially fuller, clearer, or more polished?
- What can I do next?

It should avoid dumping the original draft beside the revision unless the user explicitly asks to compare. For creative or longform prose, the revised draft itself is the value. Metadata should be compact: a result title, a short summary, and a few improvement chips are usually enough.

## Validation Lessons

Useful browser proof for this class of feature should capture:

- the exact page or thread route used
- the selected mode before click
- the outgoing follow-up payload mode and action id
- source and revised word counts
- absence of no-op preserve language
- absence of before/after comparison clutter
- visible result-card state
- elapsed time from click to usable revised draft

The proof should fail if the action silently switches to a slower lane, returns a non-expanded draft, displays the original as the main outcome, or spends extra post-processing time after a usable model response has already completed.

## Boundary

This document is intentionally public-safe. It does not include private prompts, account ids, thread ids, model filenames, routing heuristics, production URLs, screenshots, or source code.
