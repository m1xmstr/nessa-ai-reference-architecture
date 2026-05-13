# Tutor Intent Anchor and Reasoning Guardrails

This note documents a public-safe pattern for keeping a Learning / Homework Buddy tutor anchored to the student's current problem and current request.

The failure to avoid is simple and serious:

> A student asks whether their reasoning for one worksheet problem is correct, and the tutor replies with a stale hint for a different problem.

In a family learning product, that is not a minor chat issue. It breaks trust.

## Product Principle

A tutor should answer the learner's actual current turn before doing anything else.

That means the tutor needs to resolve two things on every turn:

- **intent**: what the student is asking for now
- **anchor**: which worksheet problem, equation, file region, or prior learning object the student means

The tutor should not rely on the previous button click, previous active problem, or previous assistant message when the latest user text provides a stronger signal.

## Anchor Priority

A robust tutor should prefer signals in this order:

1. explicit problem reference in the student text, including typos and shorthand
2. equation or expression restated by the student
3. currently selected or highlighted worksheet region
4. persisted active problem focus
5. prior conversational context
6. problem 1 only when the session is new and no better signal exists

This is especially important for students, because real typed messages include misspellings, missing punctuation, shorthand, and partial reasoning.

Examples of public-safe anchor signals:

- "problem 2"
- "prob 2"
- "number two"
- "#2"
- "the second one"
- "for x squared minus 4 equals 12"
- "I think v squared equals 16"

When a stronger signal arrives, the active problem focus should update immediately.

## Intent Signals

Homework Buddy should distinguish at least these current-turn intents:

- ask for a hint
- ask for an explanation
- ask for a similar example
- ask to show the answer
- ask to check work
- ask whether the student's reasoning is correct
- ask a follow-up about a specific step

The "check my work" and "reasoning check" intents deserve first-class handling. A student who already supplied an attempt is not asking for a generic hint. They are asking whether their step is valid.

Common public-safe phrases:

- "is that correct?"
- "am I right?"
- "am I on the right track?"
- "does this make sense?"
- "my answer would be..."
- "my way of thinking..."
- "I think it is..."
- "since this, then that..."
- "check my work"
- "did I do this right?"

## Deterministic Checks Before Generative Fallback

For common school-work patterns, a deterministic verifier should run before the LLM fallback where practical.

Useful verifier classes:

- arithmetic checks
- linear equations
- simple square equations
- square roots
- basic factoring
- fractions
- simple unit checks when worksheet context includes units

The verifier does not need to replace teaching. It gives the tutor a reliable truth source for simple validation, so the final explanation can be short, correct, and anchored.

## Response Contract

When the student asks for reasoning validation, answer that request first:

- confirm the part that is right
- correct the part that is wrong
- show the smallest useful next step
- mention the resolved problem focus when helpful
- avoid unrelated hints
- avoid asking the student to retype the problem when the worksheet or message has enough context

For example, if the equation is a square equation and the student gives one square-root value, the tutor should check whether there is a missing positive or negative solution instead of just saying "good job" or repeating a prior hint.

## Stale Response Prevention

Stale tutor replies usually come from one of these places:

- the previous quick-action button remains active
- the persisted active problem is not reset
- a cached assistant turn is reused after a newer student message
- stream finalization overwrites a better live answer with older fallback text
- problem focus is inferred only from the last assistant turn

Guardrails:

- assign each tutor request a turn id
- bind finalization to the latest turn id
- mark whether a reply reused old scaffold text
- block final replies that mention a different problem than the resolved anchor
- log the resolved intent and anchor source
- treat quick-action state as a hint, not as authority over fresh user text

## UI Pattern

The interface should make the current problem focus visible without becoming noisy.

Useful pieces:

- a calm focus label such as "Problem 2"
- the normalized equation or prompt when available
- a small "Now working on problem 2" chip when the focus changes
- a first-class "Check my work" action next to "Give me a hint"
- mobile layout that keeps problem focus and composer access usable

Do not make the learner hunt through diagnostics to understand which problem the tutor is answering.

## Evaluation Pack

A good regression pack should include at least:

- typos and misspellings
- shorthand problem references
- equation-over-prior-anchor cases
- stale quick-action cases
- partial student answers
- missing positive/negative square-root cases
- arithmetic and fraction mistakes
- geometry, graphing, word problems, and units
- child/restricted learner policy checks

Score each turn on:

- correct anchor
- correct intent
- mathematical correctness
- teach-first behavior
- no stale or canned response reuse
- no unnecessary "retype the problem" fallback
- latency bucket

## Public-Safe Lesson

The key architecture lesson is that tutoring quality is not only model quality. It is state discipline.

Family learning products need deterministic anchoring, current-turn intent resolution, stale-response prevention, and small symbolic checks around the LLM. The model should then explain from that trustworthy state, not guess which worksheet problem the student meant.

