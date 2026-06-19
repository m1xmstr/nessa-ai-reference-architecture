# Private AI Security Hardening Pattern

Public-safe date: 2026-06-19

This note documents a public-safe security pattern for a private family AI product. It intentionally omits private hostnames, routes, source code, prompts, account identifiers, credentials, digests, connector protocols, production proof artifacts, and operational runbooks.

## Why This Matters

Private AI security is not only "model safety." A useful family AI product has a conventional web app, account data, uploads, private documents, connected devices, optional BYO providers, agent-style workflows, and server-to-server automation. The security posture has to cover all of those at once.

The public pattern is:

1. Treat browser writes as untrusted unless the request proves it came from the same site.
2. Treat app-shell headers as hints, never as a replacement for explicit `Origin` or `Referer` checks.
3. Treat server-to-server automation as a separate channel with bearer-token authorization.
4. Do not put internal automation tokens in URLs, browser-visible state, screenshots, public prompts, uploaded files, or logs.
5. Treat uploaded documents, web pages, emails, and tool outputs as untrusted instructions until account policy allows the action.

## Threat Model

The main risk classes are:

- cross-site request attempts against browser-authenticated sessions
- prompt injection and tool injection through chat, uploads, webpages, documents, or connected channels
- excessive agency, where an AI workflow can take more action than the user intended
- token leakage through URLs, logs, screenshots, browser state, or public docs
- connector replay or forged linked-device traffic
- cross-account identifier guessing for chats, documents, devices, family records, or agent workspaces
- hidden fallback from a private route to a less-private route
- Smart Home discovery abuse against local networks
- supply-chain drift in dependencies, models, connectors, and automations

## Browser-Origin Guard Pattern

For mutating browser APIs:

- check explicit `Origin` and `Referer` first
- reject when either header is present and does not match the request host
- only consider app-shell request headers after explicit cross-origin headers have been handled
- keep API-key callers on a separate authorization path
- return a clear refresh/retry error instead of silently accepting suspicious writes

This avoids the common mistake of trusting a custom header while ignoring a hostile explicit origin.

## Internal Automation Pattern

For release gates, benchmarks, specialty jobs, and scheduled operational checks:

- keep internal automation endpoints narrow and enumerated
- require `Authorization: Bearer <internal automation token>`
- compare secrets with constant-time comparison
- disable query-string token compatibility by default
- log only minimized status metadata, not bearer tokens or request bodies
- make automation callers fail closed when a token is missing

Public docs should describe this pattern without publishing endpoint names, token names, hostnames, job payloads, or screenshots.

## Agent and Tool Boundary Pattern

Agent-style features need visible limits:

- tools are not automatically granted because a model asks for them
- connected accounts, devices, email, files, calendars, Smart Home controls, provider keys, and browser sessions require explicit product permission
- high-impact actions need approval gates or should be unavailable during beta
- uploaded files and webpages are data, not authority
- tool output must be validated before it changes app state, sends messages, writes files, or controls devices

The goal is to make "AI-jacking" a normal product-security concern rather than a prompt-only problem.

## Linked Devices Pattern

Linked Devices should be treated as private compute endpoints, not trusted LAN appliances:

- use device secrets and account-scoped authorization
- show device health truthfully
- fail closed when a user explicitly selects a private device that is unavailable
- avoid silent fallback to external or less-private routes when privacy is the point
- add replay resistance with nonce/timestamp or signed-request windows
- never publish connector internals, pairing flows, real device labels, private addresses, or token mechanics

## Smart Home Pattern

Smart Home features should protect the home network:

- require account ownership before discovery or control
- rate-limit discovery by account and IP
- keep discovery scoped to approved ranges and user intent
- distinguish monitoring from control
- prefer truthful unavailable states over fake tiles
- avoid exposing private camera/device hostnames in public docs

## Prompt and Tool Injection Regression Pack

A private AI release gate should include adversarial cases such as:

- hidden instructions in uploaded text
- "ignore previous instructions" requests
- requests to reveal system prompts, tokens, logs, hidden routes, or account data
- malicious follow-up actions such as "fix this script" when the script is harmful
- attempts to reuse another user's chat, document, device, or workspace identifier
- attempts to force private-route fallback to a public provider
- attempts to make a connected agent send email, run shell, write files, or control devices without approval

The gate should assert both the answer and the action state. A refusal is not enough if a hidden action still fires.

## Public Documentation Pattern

A useful public reference repo can explain:

- what risk class was addressed
- what control pattern was used
- what was validated
- what remains as hardening backlog

It should not publish:

- private routes, job schemas, hostnames, account IDs, thread IDs, hardware addresses, or digests
- real secrets, token names when avoidable, screenshots, logs, or private config
- exploit payloads that would make bypassing the live product easier
- proprietary prompt chains, routing heuristics, or Learning/Homework Buddy internals

## Go-To-Market Lesson

Security can be a product differentiator when it is visible as calm trust, not fear copy. For a family AI platform, the strongest market posture is:

- private by default
- family-role aware
- upload/document safe
- truthful about route and device state
- clear when an action is unavailable
- transparent about what is blocked and why
- backed by staging-to-production proof

That is more defensible than presenting another generic chatbot.

