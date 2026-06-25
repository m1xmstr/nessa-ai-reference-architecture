# Private Answer Engine Governed Routes

## Purpose

This note captures a public-safe pattern for making a private family AI assistant feel simple while still using multiple governed back-end routes. It intentionally omits private route names, routing heuristics, model filenames, account data, hardware addresses, production hostnames, screenshots, tokens, and internal cluster details.

## Product Pattern

Users should choose friendly modes, not infrastructure.

Public-safe examples of user-facing modes:

- **Fast** for quick everyday answers.
- **Auto** for normal family and work help.
- **Thinking** for harder reasoning.
- **Extended Thinking** for longer planning or analysis.
- **Preview** lanes only for eligible owner or advanced users.

Behind those modes, the platform can still govern task-specific routes for quick chat, quality answers, code help, long-form generation, Learning, document reading, photo understanding, image workflows, private linked devices, and disabled-by-default external providers.

The product contract is that Basic users see stable, friendly choices while the system records enough internal truth for owners and operators to debug quality, latency, privacy, and fallback behavior.

## Route Truth Without Model Chaos

Every answer should record a normalized, privacy-safe answer record.

Useful fields:

- requested friendly mode
- actual friendly route used
- model or device class, not a private model filename
- fallback reason
- stream finish reason
- continuation state
- latency
- user-visible mode
- privacy class

Owner/admin diagnostics can show phrases such as "used current local model", "used document reader", "used image model", or "used private preview fallback" without exposing raw route IDs, private model names, filesystem paths, hardware IDs, or network details.

Basic UI should not expose:

- raw internal route identifiers
- model filenames or quantization strings
- hardware IDs
- backend names that only operators understand
- provider internals
- debug payloads meant for owners

## Preview Lane Governance

Preview lanes are useful for new local runtimes, linked devices, and speculative decoding experiments, but they should not become default product behavior until they pass quality, latency, privacy, fallback, and stream-completion gates.

Public-safe rollout pattern:

1. Keep preview lanes owner-only or advanced-only.
2. Deny child, restricted, free, and non-owner override attempts at the API layer.
3. Show friendly availability status without raw route IDs.
4. Record the actual route used and fallback reason.
5. If the preview lane is unavailable, either fail closed or fall back with explicit copy that says a safer current route was used.
6. Do not rename a fallback as the requested premium or preview route.

This lets a product test better routes without making Basic users parse model infrastructure.

## Long Generation Contract

Answer Engines should treat long story, code, and planning outputs as a stream-finalization problem, not only a model-selection problem.

Useful checks:

- first token arrives within the lane budget
- heartbeats keep the browser from looking frozen
- final stream state is recorded
- completed messages are persisted
- partial messages are preserved on timeout or transport failure
- users get a clear continuation path when the generation cannot finish
- code fences remain balanced or the assistant offers continuation instead of silently cutting off

This contract matters for every route, but it is especially important for preview and linked-device lanes because model runtime, transport, gateway, and browser streams can fail independently.

## Finalization And Fallback Drift

Finalization should preserve the answer that matches the request, not blindly prefer the longest postprocessed or fallback text.

Public-safe checks:

- preserve the streamed response as a candidate
- compare final candidates against the requested task type, topic, protagonist, setting, and format
- reject unrelated seeded fallback stories, prompt-control leakage, or continuation scaffolding
- prefer a shorter matching response over a longer drifting response
- repair stale saved context when a stored assistant answer is corrected

This is the same product contract as route truth: the system should know which answer actually satisfied the user-visible request. See [58-story-finalization-and-scoped-auto-mtp.md](./58-story-finalization-and-scoped-auto-mtp.md).

## Release Gates

Public-safe gates for a governed Answer Engine include:

- answer records include route truth, fallback truth, privacy class, latency, stream finish, and continuation state
- preview modes are owner-only or advanced-only
- child and restricted users cannot select preview lanes
- Basic DOM and debug output contain no raw model or route internals
- unavailable preview endpoints prove fallback or fail-closed behavior
- long story and code streams complete or offer continuation
- finalization and fallback candidates preserve the requested task rather than drifting to unrelated content
- scoped Auto-MTP is enabled only for task classes with full app-route proof
- document, photo, Learning, image, and weather truth paths keep their existing specialized routes

The reusable lesson is not the private gate code. The reusable lesson is that a product-grade private AI assistant needs release gates for route privacy and stream truth, not only a benchmark score.

## Public Boundary

Keep public materials at the pattern level.

Do not publish:

- private route names
- exact routing heuristics or scoring
- private model filenames
- live model inventories
- internal hostnames, IPs, or hardware addresses
- connector schemas, tokens, or tunnels
- account-specific role behavior
- private screenshots, logs, cookies, or cluster inventory

The public architecture pattern is "friendly modes over governed internal routes": the product remains simple for families while the platform keeps enough evidence to operate safely.
