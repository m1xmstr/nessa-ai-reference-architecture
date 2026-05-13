# Learning and Family Safety

## Purpose

Learning / Homework Buddy is a core Nessa AI product pillar. This public document describes the teaching and safety philosophy without exposing proprietary lesson-flow logic.

## Product Principle

Nessa should help a learner understand, not merely produce answers.

Useful public pattern:

- teach first
- guide step by step
- ask what the learner knows
- give hints before final answers where appropriate
- anchor the hint or explanation to the exact visible worksheet problem
- correct OCR or vision ambiguity before building the explanation
- keep explanations age and family appropriate
- preserve student trust and parent trust

## Learning Modes

Public-safe capabilities can include:

- worksheet and photo support
- document support
- guided explanations
- similar examples
- step-by-step practice
- parent/tutor-friendly summaries
- resume honesty when a prior lesson or document is being continued

These are product outcomes, not implementation details.

## Workflow Proof Pattern

Learning surfaces should be validated as guided workspaces, not as chat boxes with education-themed cards.

Public-safe workflow checks include:

- starting a new lesson with or without an attached worksheet
- resuming the latest relevant lesson with honest labels
- rejecting unsupported or oversized worksheet files with clear copy
- proving uploaded worksheets become real linked artifacts
- asking follow-up questions inside the same lesson context
- saving, completing, archiving, and restoring visibility where supported
- generating study plans and study packs as saved learning objects
- showing parent or owner progress only when the role and family settings allow it
- forcing child or restricted learners into safer/basic controls
- hiding advanced model, device, and admin controls from restricted learner views

The reusable pattern is to prove the learner and parent experience end to end in staging with disposable personas, then promote only the verified build. This repo does not publish the proprietary lesson-state model, prompt chain, worksheet parsing implementation, or anti-cheat logic.

## Trust Failures Are P0

For a family product, the following are serious defects:

- giving unsafe content to children
- dumping answers when the surface promises teaching
- pretending to understand a worksheet when OCR or vision failed
- repeating generic scaffold text instead of responding to the student's exact hint or explanation request
- losing context without saying so
- exposing private family data
- routing private learning content externally without explicit user choice

## Safety Themes

Public-safe themes:

- family-safe defaults
- stronger defaults for child profiles
- no training on private family work without explicit policy
- clear refusal and redirection for harmful content
- supportive response style for self-harm or crisis language
- zero tolerance for child exploitation content
- privacy-first handling of worksheets, photos, and documents

## What Is Not Published

This repo does not publish:

- exact pedagogical prompt chains
- worksheet parsers
- solution heuristics
- anti-cheat logic
- lesson-state schema
- tutor flow mechanics
- hidden prompts
- specific child/family data
- private Learning telemetry

## Related Doc

See [FAMILY_AI_SAFETY_PATTERNS.md](./FAMILY_AI_SAFETY_PATTERNS.md) for broader public-safe family AI safety patterns.
