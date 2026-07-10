# Learning Visual Artifacts and Private Compute Truth

Private family AI becomes more useful when explanations can produce a durable visual artifact and when private compute is explained in language a family can understand.

This public pattern combines three lessons:

- Learning visuals should be generated from explicit, validated problem structure.
- Generated visuals should behave like real account-scoped learning materials, not temporary decoration.
- Linked Devices should communicate privacy, owner priority, availability, contribution, and fallback truth without exposing connector internals.

## Visuals Are Learning Artifacts

A graph, coordinate plot, or chemistry atom-count board should have a simple product contract:

1. Detect an explicit visual request in a Learning or Homework Buddy context.
2. Parse the equation, points, or reaction into structured data.
3. Refuse to invent a visual when the structure is missing or invalid.
4. Render the visual deterministically from the parsed structure.
5. Save it with the lesson so the learner can view or download it again.
6. Explain what the visual proves and what the learner should try next.

For a chemistry reaction, the artifact can show reactants, products, coefficients, element counts on each side, and whether the counts match. The UI should say `Needs balancing` when they do not match. It should never change subscripts just to make a board look balanced.

For a function or coordinate problem, the artifact should preserve the actual expression, points, requested range, and any shaded relationship. A graphing request without an equation should return useful setup guidance instead of a fabricated graph.

## Account and Safety Boundaries

Generated learning visuals should follow the same privacy and lifecycle rules as uploaded worksheets and saved documents:

- Bind the artifact to the signed-in account and current lesson.
- Keep raw storage paths, account identifiers, and processing details out of normal UI.
- Provide a real view or download path only after the file exists.
- Keep deletion and retention behavior consistent with the product's document policy.
- Preserve teach-first guidance for children and restricted family roles.

The public architecture does not publish worksheet parsers, lesson-state logic, prompt chains, private routes, or anti-cheat heuristics.

## Linked Devices in Product Language

Private compute should be described by the promises users can verify:

- **Owner priority:** the device owner controls whether the device participates.
- **Privacy:** supported work stays on an approved private path unless the user explicitly chooses another provider.
- **Readiness:** the product distinguishes ready, busy, offline, and setup-required states.
- **Contribution:** completed work is shown as a verified contribution, not vague infrastructure activity.
- **Fallback:** ordinary network and cluster fallback remain available when a direct path is unavailable.

Avoid presenting raw endpoint names, tokens, device identifiers, routing scores, or connector logs as product value. Advanced diagnostics can exist for owners, but the first screen should answer: Is my device ready, what is it allowed to help with, did it help recently, and what happens when it is offline?

## Direct-Link Truth

Two direct USB4 or Thunderbolt paths are two independent paths. They are not automatically one combined pipe, and they do not make every chat faster.

Public-safe status should report:

- each path's current state;
- the last eligible transfer;
- observed throughput from a sanitized benchmark;
- whether normal network fallback remains healthy;
- the reason a direct path was not used.

This keeps impressive hardware from becoming misleading product copy.

## Platform Operating Model

The surrounding platform remains deliberately separated:

- OpenShift runs the application, data, routes, and durable operations surface.
- OpenShift AI is the governed serving and evaluation plane.
- Ansible Automation Platform owns scheduled and manually launched repeatable operations.
- Event-Driven Ansible owns bounded webhook reactions with clear service ownership.
- Linked Devices remain user-approved private endpoints, not OpenShift workers.

Product-polish releases should not silently mix in model swaps. Candidate models belong in a separate, default-off experiment with cold and warm load, first-token, sustained generation, multimodal quality, safety, and fallback measurements.

## Release Proof

A release that adds Learning visuals or changes Linked Devices UX should prove:

- focused parser, renderer, persistence, and account-scope tests;
- a real staging request that creates and serves the expected artifact type;
- desktop and mobile browser geometry with the primary action visible;
- Linked Devices ready/offline/no-device states without raw identifier leaks;
- exact staged-image promotion, production version and digest checks, recent logs, and cleanup;
- an explicit public-documentation decision.

The key lesson is simple: visible learning and private-compute promises must be backed by saved artifacts, measured state, and repeatable proof.
