# Chat-First Home Simplicity Gate

This note documents a public-safe pattern for private AI products that start as a chat assistant but drift into a busy dashboard.

It intentionally does not publish product source, private routes, account state, screenshots, user data, analytics, prompts, model routing rules, or proprietary implementation details.

## Product Principle

The default chat home should answer one question:

What can I ask right now?

Daily context can be useful, but it should not compete with the primary chat action. If a home surface has cards, panels, counters, operational status, and starter prompts all competing above the fold, the user has to decode the product before asking for help.

## When To Remove Instead Of Polish

Dashboard content should be removed from the default first fold when it causes any of these failures:

- the chat composer overlaps content
- multiple panels compete as equal primary actions
- status details feel like operations telemetry
- stale learning/document snippets appear before the user asks for them
- mobile users land in the middle of cards instead of at the composer
- the first screen needs explanation before it can be used

The fix is not always "make prettier cards." Sometimes the correct product move is to remove the cards.

## Clean Home Contract

A clean AI home can be as small as:

- product header
- simple varied greeting
- one composer
- optional sign-in or profile affordance
- no dashboard cards
- no starter-chip clutter unless explicitly useful and capped
- no raw device, model, admin, or control-plane copy in Basic mode

The user should be able to type without scrolling, interpreting, or dismissing anything.

## Release Gate Pattern

Add a first-fold browser gate for the authenticated owner and mobile guest paths.

Useful assertions:

- composer is visible on load
- no card or panel overlaps the composer
- no daily-brief card grid is visible by default
- no more than a small capped set of attention items appears above the fold
- mobile has no horizontal scroll
- sign-up or provider banners do not push the chat action out of view
- hidden menu text is not visually present on the first fold
- Basic mode does not expose raw device/model/admin/control-plane strings

When the product decision is "chat-only home," the gate should fail if cards, starter chips, secondary navigation rows, or daily brief panels reappear.

## Recovery Path For Context

Removing dashboard cards does not mean removing workflows.

Learning, Documents, Family, Linked Devices, and support surfaces can remain available through the menu, profile, or explicit user prompts. Context can still be fetched after the user asks for it. The important distinction is that default home state should not make every subsystem look equally urgent.

## Public Boundary

The reusable pattern is UX discipline plus a browser gate. Keep private:

- actual account state and household data
- exact dashboard payload schemas
- session IDs and attachment IDs
- screenshots from private accounts
- model/device inventory
- routing and entitlement internals

The public lesson is straightforward: for a private family AI assistant, the fastest path to premium can be less UI, not more UI. Make chat obvious first, then reveal context only when it helps.
