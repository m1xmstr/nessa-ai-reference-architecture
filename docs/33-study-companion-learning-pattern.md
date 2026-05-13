# Study Companion Learning Pattern

This note documents a public-safe pattern for turning a private AI learning surface into a study companion rather than a chat box with education-themed controls.

It intentionally does not publish proprietary lesson-state schemas, worksheet parsers, prompt chains, answer policies, account data, screenshots, private routes, or exact tutor implementation details.

## Product Principle

A study companion should answer one practical question:

> What should the learner do next, and why?

The first screen should make the active file, current problem, useful next action, and learning goal obvious. Generic worksheet commentary should not be the default.

## Worksheet To Plan

When a learner uploads a worksheet, photo, or supported document, the public-safe workflow is:

1. accept the file only if the type and size are supported
2. extract visible topics and candidate problems
3. identify the first useful problem or section
4. create a short study plan
5. set a visible active problem focus
6. start with a helpful next action instead of generic filler

If extraction is weak, the product should say what is missing and ask for a better file or clarification. It should not pretend the worksheet was understood.

## Problem Focus Mode

Problem focus is the student-facing state that keeps help grounded.

Public-safe UI pieces:

- active file name or file label
- active problem number when known
- short normalized equation, prompt, or topic excerpt
- small focus chip when the problem changes
- visible worksheet region highlight when available
- easy actions for hint, similar example, check work, explain mistake, and answer policy

The focus should follow the student's latest strong signal. A stated problem number or restated equation should beat stale state.

## Teach-First Actions

The study companion should separate common learning intents:

- **Hint ladder**: nudge without solving immediately
- **Similar example**: teach the method on a nearby problem
- **Check my work**: validate the student's reasoning attempt first
- **Explain my mistake**: identify the specific incorrect step
- **Show final answer**: allowed only when the family or learner policy permits it

Checking a student's work is different from giving away an answer. If the student supplied an attempt, the tutor can confirm correct reasoning and correct mistakes directly while still preserving teach-first behavior.

## Parent Recap

Parents and owners need progress, not a private transcript dump.

A useful recap includes:

- what was practiced
- what the learner got right
- what still needs work
- suggested next session
- completion or archive state when relevant

The recap should avoid unnecessary private chat details and should respect role and family visibility settings.

## Mobile Pattern

On phones, split-pane learning UIs usually collapse poorly.

Public-safe mobile behavior:

- one clear next action
- problem context available without blocking the composer
- worksheet context behind a toggle or focused panel
- tap-safe teach-first actions
- no raw model, device, admin, or diagnostic controls for restricted learners

## Release Proof Pattern

A serious study companion release should prove the full workflow in staging before production:

- upload worksheet or supported file
- create study plan
- focus a problem
- provide a hint
- check a messy student reasoning attempt
- correct a mistake
- create or show a parent recap
- prove restricted learner safety
- prove mobile usability

The public lesson is that Learning quality comes from workflow discipline, not only model quality.

