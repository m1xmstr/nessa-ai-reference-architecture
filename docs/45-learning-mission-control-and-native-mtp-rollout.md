# Learning Mission Control And Native MTP Rollout

Last updated: 2026-05-28

This document captures a public-safe pattern for two related private-AI product improvements:

- turning a Learning / Homework Buddy dashboard into a real worksheet mission-control surface
- introducing native Strix Halo class MTP acceleration into a governed high-deliberation lane before allowing broader automatic routing

It intentionally omits private routes, account details, node names, model filenames, prompts, worksheet parsers, session schemas, endpoint addresses, deployment manifests, tokens, and production proof artifacts.

## Worksheet Mission Control

A Learning dashboard should not show fake progress cards. It should render problem controls only when the backend has real extracted worksheet or lesson state.

Public-safe rules:

- show an empty state when no worksheet problems are indexed
- show problem cards only from validated extracted problem metadata
- label the active file and active problem plainly
- link each problem card to a real study session
- keep teach-first actions wired to the actual tutor path
- do not create placeholder buttons that look helpful but do nothing

The useful shift is from "upload a worksheet and chat" to "open the exact problem and choose the next learning action."

## Teach-First Actions

Problem cards work best when their actions map to the learning policy:

- focus the problem without sending private filler text
- ask for a hint
- check the learner's work
- explain the steps
- create a parent recap when the viewer is allowed to see it

These actions should flow through the same lesson/session machinery as typed learner messages. A mission-control action is still a tutor turn, not a detached UI shortcut.

## Product Truth Contract

The mission-control surface should make three things obvious:

- what file or worksheet is active
- which problem is selected
- what the learner can do next

If the extraction is weak, the product should say so and ask for a better scan or a problem number. It should not pretend to know the worksheet.

## Native MTP Rollout Pattern

Native MTP acceleration should be promoted by lane, not by excitement.

A practical rollout sequence:

1. prove the runtime directly
2. prove the gateway path
3. prove the full app route
4. enable owner/admin preview
5. enable a high-deliberation mode such as Thinking
6. keep Auto off or canary-limited until quality and idle-cost data justify it

This lets the product use acceleration where the user explicitly asked for more reasoning while keeping everyday automatic routing stable.

## Full App Proof

Direct benchmark success is not enough. A native MTP lane should prove:

- no route contamination
- first-token behavior is acceptable
- streamed responses finalize visibly
- the final answer is persisted when persistence is expected
- fallback copy is plain and truthful
- child/restricted users cannot force the lane
- Basic UI does not expose raw model or hardware details
- recent logs stay clean after deployment

## Idle Cost And Power

Keeping an accelerated runtime resident can be justified only when product value exceeds idle cost.

Public-safe telemetry should answer:

- is the runtime actually needed for current traffic?
- is GPU or CPU busy at rest?
- does the node return to low activity after a request?
- are unrelated non-AI workloads accidentally placed on the AI node?
- does the health probe prove the model is ready, not merely that a socket is open?

For MTP-style runtimes, HTTP health or model-ready probes are usually better than a bare TCP readiness check because model loading can last long after the server accepts a connection.

## Release Gates

Reusable gates for this pattern:

- dashboard renders mission control only from real indexed problems
- every visible mission-control action has a working tutor/session path
- mobile and desktop Learning views keep the composer usable
- owner/admin Thinking or preview mode selects the governed MTP lane only when policy permits it
- Auto remains off or canary-limited until separately approved
- production canary proves stream finalization and persisted final-message truth
- idle telemetry confirms the AI worker returns to a reasonable rest state

## Public Lesson

The shared lesson is product truth. Learning cards should be backed by real worksheet state, and MTP routing should be backed by full app proof. A private AI product earns trust when the visible control, backend route, persistence record, and operational state all tell the same story.
