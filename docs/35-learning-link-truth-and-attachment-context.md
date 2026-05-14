# Learning Link Truth and Attachment Context

This note documents a public-safe pattern for two trust failures that can happen in private family AI products:

- a dashboard continuation link points at a Learning session that no longer loads
- a photo or attachment follow-up accidentally binds to stale or empty attachment context

It intentionally does not publish private routes, user accounts, emails, session IDs, screenshots, database rows, prompts, OCR internals, model routing rules, or proprietary Learning / Homework Buddy implementation details.

## Product Principle

A visible continuation or attachment affordance is a promise.

If a product says "continue this lesson," the destination must be loadable for the current user. If a chat says it is using an attached photo, the selected attachment must be the user's current usable file, not an old generated asset, missing file, or empty metadata row.

## Continuation Link Truth

Learning dashboards should validate targets before rendering continuation cards.

Public-safe checks:

- session id exists
- current user is allowed to open it
- lesson state is resumable, completed, archived, or recoverable with honest copy
- required worksheet or material artifact still exists
- tutor payload can be loaded
- snippet text reflects the current session, not an old hint or stale problem

If those checks fail, the dashboard should not show a stale "continue" action. It should either point at a valid replacement session or offer a clean "Start Learning" action.

## Recovery Instead Of Half-UI

Direct old links still matter because users bookmark, share, and revisit them.

A missing or private Learning link should render a dedicated recovery state:

- what happened in plain language
- Back to Learning
- Start a new session
- Open recent sessions
- Start from the same worksheet when that artifact still exists
- remove stale dashboard state when the product can safely do so

Do not leave the old tutor controls, blank worksheet panel, or message composer visible under a generic error. That makes the page look partially broken and makes users wonder whether their work disappeared.

## Attachment Context Truth

Attachment fallbacks should prefer explicit current-turn document IDs. When no explicit ID is present, recency or thread affinity can help, but only after validating that a row has usable chat payload.

Public-safe payload checks:

- readable extracted text, or
- a real storage path, or
- nonzero file size, or
- a current uploaded image row that the app can still open

Rows that represent generated images with no stored file and zero bytes should not be treated as usable photo context. They can remain in history as records, but they should not become the answer target for a new "describe this photo" request.

## No-Response Prevention

When an attachment cannot be read, the user should still receive a persisted, clear response.

Good behavior:

- return a non-500 API response
- explain that the attachment could not be analyzed in user-safe language
- preserve the user turn and assistant reply in the thread when the user is signed in and not in incognito mode
- avoid misleading "success" copy before the fallback has been saved
- log enough route/context truth for operators without leaking file contents or secrets

The practical rule is that "I could not read that file" is still a product response. Silent failure is not acceptable.

## Release Gate Pattern

Add release gates that seed or reuse representative stale states:

- a Learning continuation card whose original target is missing or unrecoverable
- a direct missing Learning URL
- a valid replacement session
- an attachment history row with metadata but no usable payload
- a current uploaded image that should still be selected

Passing gates should prove:

- dashboard cards only point to valid or recoverable destinations
- missing sessions show recovery UI, not blank tutor chrome
- stale snippets are suppressed
- stale empty generated assets are skipped for photo follow-ups
- unreadable attachment fallback replies are persisted

## Public Boundary

The public pattern is about truth and recovery discipline. Keep private:

- exact session schemas
- worksheet parsers and tutor prompt logic
- account and family identifiers
- production database rows
- private screenshots and uploaded files
- internal routing heuristics

The reusable lesson is simple: private AI products need target validation before rendering continuation actions, and attachment-aware chat must prove the selected file is both current and usable before it claims to describe it.
