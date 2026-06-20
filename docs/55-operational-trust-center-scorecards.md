# Operational Trust Center Scorecards

Public-safe pattern from Nessa AI, 2026-06-19.

## Pattern

A Trust Center should be operational evidence, not a static marketing page.

For a private family AI product, the Trust Center needs four durable layers:

1. public posture
2. known limitations
3. report path
4. measured gates

The goal is to let a family, reviewer, tester, or partner see what is claimed, what is still limited, where to report a security or safety issue, and what release gates were measured.

## Public-Safe Trust Center Fields

Recommended public fields:

- last verified date
- release/version reference
- known limitations
- report a security or safety issue
- release notes link
- security updates link
- sanitized architecture notes link
- measured security gates
- measured product quality gates

Do not publish exploit payloads, private account identifiers, raw device identifiers, exact connector job schemas, internal prompt chains, private network topology, or production screenshots containing user data.

## Security Scorecard Examples

Public-safe rows can name categories without exposing payloads:

| Gate | Public description |
|---|---|
| Prompt/tool injection | Covers document, web retrieval, Smart Home, connector, email, and agent-artifact boundaries. |
| Cross-account identifier fuzzing | Checks private account, family, document, learning, device, and Smart Home identifiers for scope failures. |
| Connector replay protection | Confirms signed timestamp, nonce, and signature boundaries reject replay. |
| Scan rate limits | Confirms household discovery scans are bounded by account and IP rate limits. |

## Product Quality Scorecard Examples

Public-safe rows should match real flagship workflows:

| Gate | Public description |
|---|---|
| Homework Buddy | Worksheet upload, guided help, parent summary, progress memory, and anti-cheat boundaries. |
| Documents | Private upload, organize/search, redaction, visual QA, retention controls, and security explanations. |
| Linked Devices | Setup, health, privacy, workload status, and honest fallback for private compute at home. |

## UI Stability Gate

Trust is also affected by small first-impression defects.

If a public or signed-in home screen renders one thing and then changes seconds later, users read that as instability. The safer pattern is:

- choose the home greeting once per visible home session
- update supporting dashboard data without swapping the headline
- keep long dynamic prompts short or allow wrapping
- verify desktop and mobile screenshots after the async refresh window
- test no horizontal overflow, not only that text exists

## Release Discipline

The run record should explicitly state:

- what was locally tested
- what was browser-tested in staging
- what was browser-tested in production
- which broad gates were skipped and why
- whether exact staged artifacts were promoted
- what limitations remain

This keeps public trust copy tied to evidence instead of becoming a claim pile.
