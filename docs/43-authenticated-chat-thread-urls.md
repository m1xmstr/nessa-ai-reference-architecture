# Authenticated Chat Thread URLs

## Purpose

Private AI assistants need durable links for support, QA, history, and browser continuity. A per-thread URL lets a user reopen the exact conversation, lets an operator attach proof to the right chat, and makes debugging less dependent on screenshots or hidden database records.

This note captures a public-safe pattern only. It intentionally omits private routes, database schema details, account data, chat contents, screenshots, production hostnames, tokens, internal identifiers, infrastructure names, and exact implementation code.

## Product Pattern

Keep the empty chat home clean. Do not create a durable thread just because a user opened the app.

When the first message is persisted, assign the thread a high-entropy public-facing identifier and update the browser URL without a full reload. The URL should be stable enough for authenticated history, reload, and support workflows, but possession of the link must never grant access.

Useful behavior:

- empty new chat stays on the clean app home
- first persisted message updates the URL to a thread path
- direct reload of a thread path restores the exact authorized conversation
- browser back returns to clean new chat without resending a prompt
- browser forward restores the thread without duplicate messages
- history items link to the stable thread path
- delete returns the user to clean new chat
- temporary or incognito chats keep their non-durable policy

## Identifier Rules

The public-facing thread id should be opaque and non-sequential.

Do not expose:

- raw database ids
- account ids or email-derived ids
- timestamps alone
- predictable counters
- internal route ids
- model or hardware identifiers

A public-facing id is only a locator for an authenticated lookup. Authorization must still happen server-side before any chat content, attachment metadata, or diagnostic context is returned.

## Privacy and Failure States

Missing, deleted, stale, and unauthorized thread URLs should use the same safe user-facing outcome. Do not confirm that a thread exists for another account.

A safe message can say the chat could not be opened and return the viewer to a new chat state. It should not reveal owner identity, account membership, title, attachment names, model route, or whether the thread was deleted.

Logs should prefer a public thread id or hash plus a request id. Avoid raw private prompt text unless a separate sampled-debug policy explicitly allows it.

## Debuggability Without Leaks

Owner or administrator diagnostics can show a friendly chat link and sanitized answer trace.

Useful owner-safe fields:

- public thread id
- request id
- sanitized answer record pointer
- created and updated timestamp
- friendly route label
- continuation state

Basic users should see normal history only. They should not see route ids, model filenames, hardware names, private paths, endpoint names, or owner diagnostics.

## Context Recall

Durable thread URLs are only useful if reload preserves the real working context. A private assistant should prove that a receipt, photo, document, or other compact thread fact survives a reload and can answer a follow-up in the same conversation.

That does not require publishing raw files or private extracted text. The public lesson is to persist and authorize compact context pointers carefully, then gate reload and follow-up recall as a product behavior.

## Release Gates

Public-safe gates for per-thread URLs include:

- new chat remains non-durable until the first persisted message
- first send changes the URL without breaking streaming
- direct reload restores the authorized thread
- history links use the public-facing thread id
- copied links use the canonical thread path
- unauthorized, missing, stale, and deleted URLs do not disclose existence
- no raw database ids appear in browser-facing APIs or Basic DOM
- streaming emits thread identity once and does not duplicate threads
- browser back and forward do not resend prompts
- context recall survives reload for attachment-aware threads
- temporary or incognito chats keep their current privacy policy

## Public Boundary

This pattern is not a public sharing feature. It is authenticated continuity and support debugability.

Do not publish private source code, schema details, production links, screenshots, chat history, account data, internal identifiers, tokens, routes, model filenames, hardware addresses, or infrastructure inventory.
