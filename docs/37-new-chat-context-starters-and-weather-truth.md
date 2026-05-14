# New Chat Context Starters and Weather Truth

This note documents a public-safe pattern for making a private AI assistant feel personal at the start of a new chat without turning the first screen into a dashboard.

It intentionally does not publish product source, private routes, account state, screenshots, user data, model routing rules, weather locations, prompt chains, or proprietary implementation details.

## Product Principle

A new-chat screen should make asking easy first.

Useful context can help, but only when it is real, short, and immediately actionable. A clean start screen can include a greeting, one composer, and a few contextual starters. It should not show a grid of product areas, counters, operational status, or stale resume snippets before the user has asked for anything.

## Greeting Contract

The greeting should be simple and time-aware:

- morning, afternoon, or evening
- short second sentence
- light variation across refreshes
- no forced personality
- no repeated fixed menu language

Good examples are:

- "Good morning. What should we handle first?"
- "Good afternoon. Ready when you are."
- "Good evening. Send me anything."

The greeting should not use a handle as a first name unless the user explicitly set it as their preferred greeting name.

## Starter Contract

Starter chips can be useful when they are capped and grounded in current state.

Public-safe rules:

- show two to four starters at most
- keep labels short
- route only to valid destinations
- hide resume links for missing, private, archived, or expired sessions
- hide admin, device, billing, or owner-only actions from restricted users
- prefer verbs over product categories

Examples:

- "Upload worksheet"
- "Summarize document"
- "Redact a file"
- "Create image"
- "Plan evening"

Avoid generic repeated chips such as "Writing," "Learning," and "Documents" when they are just category labels.

## Weather Truth Contract

A weather chip is optional. It should be hidden unless the product has a verified current weather source for the user's authorized home or profile location.

When shown, the chip should be tiny and factual:

- current temperature or condition
- source timestamp available in details
- no severe, watch, warning, or alert language unless an official active alert gate proves it

If source data is stale, ambiguous, missing, or tied to an uncertain location, the chip should not render.

## Release Gate Pattern

Add a browser gate for the authenticated owner and mobile paths:

- greeting matches the current time bucket and rotates without becoming noisy
- composer is visible on load
- starter chips count stays between two and four
- each starter is short and has a valid route or prompt action
- no starter points to a missing Learning session
- restricted users do not see admin, device, model, or owner-only starters
- weather chip renders only with verified current weather data
- severe/weather-alert language is absent unless official active-alert proof is present
- no dashboard cards or first-use nudge cards reappear under the composer

## Public Boundary

The reusable pattern is product discipline plus release gates. Keep private:

- account-specific context
- exact dashboard payload schemas
- session IDs and document IDs
- weather locations
- screenshots from private accounts
- routing, entitlement, and model-selection internals

The public lesson is that personalization does not require clutter. The assistant can feel current and useful with one calm greeting, one composer, a few truthful starters, and verified weather only when the data earns its place.
