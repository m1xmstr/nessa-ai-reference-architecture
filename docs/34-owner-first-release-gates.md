# Owner-First Release Gates

This note documents a public-safe pattern for catching premium-home, route-truth, recovery, and weather-copy failures before a private family AI release reaches production.

It intentionally does not publish private account data, screenshots, production routes, session IDs, gate source code, cluster hostnames, cookies, prompts, or internal implementation details.

## Product Principle

A family AI home surface should answer one calm question:

> What does this family need from the assistant today?

Owner accounts often see the most capable product state, so they also expose the most clutter risk. A release gate should prove the owner experience remains premium, understandable, and chat-first instead of drifting into an operator dashboard.

## First-Fold Calm Home

The first fold should not make every product area compete for attention.

Useful gate assertions:

- the authenticated owner home loads without the chat composer covering useful content
- no more than a small number of primary attention items are visible above the fold
- major areas such as Learning, Documents, Family, Private Compute, and Ask are not all presented as equal hero panels at once
- operational or diagnostic language is absent from the Basic home view
- the default state feels like a helpful assistant, not a cluster or task dashboard

The point is not to hide capability. The point is to reveal it with priority. Secondary cards can live behind progressive disclosure when they are not the most useful next step.

## Continuation Route Truth

Continuation actions are trust surfaces. If a home card says "Continue Learning" or "Resume", it must open the right workflow.

Public-safe checks:

- click the top continuation action from the signed-in home surface
- pass only if a valid active session, recovered lesson, or clear recovery screen opens
- fail if the destination shows a missing-session error inside otherwise blank UI
- fail if the destination opens a generic tools catalog or unrelated page
- fail if stale problem text or a dead route appears

This catches a common class of single-page-app bugs: the URL looks correct, but the router renders the wrong workspace.

## Not-Found Recovery

Private family products need graceful recovery for expired, deleted, private, or stale links.

A missing learning session page should explain what happened and offer clear actions:

- return to Learning
- start a new session from the same worksheet when recovery is possible
- remove the stale dashboard link when recovery is not possible

Do not show a generic error banner while leaving the original tutor, worksheet, or chat chrome blank underneath. That makes users wonder whether their data is missing, loading, or broken.

## Greeting And Weather Truth

Greeting and weather are small surfaces, but they strongly affect trust.

Public-safe gate assertions:

- greeting copy is simple, varied, and not hard-coded to one product slogan
- handles or internal usernames are not treated as first names
- weather chips use only verified current weather truth for current temperature or conditions
- severe weather, warning, watch, or alert language is forbidden unless an official active alert has been validated by the weather truth gate
- forecast risk uses softer language and never impersonates an official alert

Weather copy should separate "current condition", "forecast risk", and "official active alert" so users cannot confuse casual forecast chatter with a life-safety notice.

## Evidence Pattern

Failed owner-first gates should produce fixable evidence:

- route opened
- viewport size
- screenshot or DOM excerpt with private values masked
- exact failed assertion
- release version and digest class
- whether the failure happened in staging or read-only production canary

Passing evidence should prove the exact staged digest that will be promoted, not a later rebuild.

## Public Boundary

This repo shares the release discipline, not the private implementation.

Keep out:

- private session URLs
- account names and emails
- worksheet images or private family data
- production screenshots
- exact gate code
- credentials, cookies, or route tokens

The reusable pattern is straightforward: owner-visible surfaces need their own release gates because owner accounts combine family, learning, documents, compute, and admin-adjacent features. That makes them the best early warning system for clutter, stale links, route drift, and copy that overclaims.
