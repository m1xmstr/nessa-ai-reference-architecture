# Family Command Center Pattern

This note documents a public-safe pattern for turning a private family AI product home screen into a daily command center instead of a blank chatbot prompt.

The product question is simple:

> What does my family need from AI today?

The answer should be grounded in real account state, not fake cards, marketing placeholders, or operator diagnostics.

## Design Principle

A family AI home surface should summarize the user's day in plain language and offer the next useful action.

Good cards:

- show real learning sessions, document state, household safety status, and private-compute readiness
- explain honest zero states when there is no activity yet
- hide cards when the backend cannot support a working action
- keep admin, model, raw device, and routing details out of Basic mode
- give child or restricted users a learner-safe version

Bad cards:

- invent "pending" work
- expose raw model names or hardware ids
- use control-plane or operator language
- put owner/admin controls in front of regular family members
- show buttons that do nothing

## Recommended Cards

Use a small number of durable cards that map to actual workflows:

- **Today with Nessa**: current learning, documents, family safety notices, generated artifacts, linked-device readiness, and only verified home/weather alerts
- **Learning**: continue latest lesson, upload worksheet, create study plan, and parent progress summary when authorized
- **Documents**: upload, summarize, redact, and recent documents
- **Family**: household status, kid-safe defaults, invites, and weekly learning summary
- **Private Compute**: simple ready/unavailable state without raw device details in Basic mode
- **Ask Nessa**: practical starter prompts customized to current state

Every action should resolve to a real route, upload path, prompt, or disabled state with clear next-step copy.

## Persona Rules

The same home surface should adapt by role:

- **Owner**: can see household status, family controls, private-compute readiness, and parent progress summaries
- **Adult family member**: sees family membership and allowed shared workflows, not controls meant for another role
- **Child/restricted member**: sees a learner-safe view, Basic mode, and no model/device/admin controls
- **Free user**: gets a clean starter view and honest plan gates
- **Guest**: gets a useful starter view with sign-in moments only where saving or account state is needed

The safest default is fail-closed: if role or entitlement state is missing, show the simpler non-admin version.

## Data Truth Contract

The command center should use real source data:

- learning state from saved lesson/session records
- document counts and recent documents from the document library
- generated artifacts from stored metadata, with short safe titles
- family state from the family account/roster service
- linked-device readiness from current heartbeat, permissions, and model availability
- weather or home alerts only when an official/verified alert path exists

No fake data is better than fake polish.

## Basic vs Advanced

Basic mode should read like a family product:

- "Private compute is ready"
- "Continue your last lesson"
- "Upload a worksheet"
- "Review family settings"

Advanced diagnostics can exist, but they should sit behind explicit owner actions. They should not be part of the default daily home.

## Validation Pattern

Before shipping, prove the surface with browser tests:

- owner desktop and mobile
- guest or free starter state
- child/restricted learner-safe state
- no horizontal overflow on narrow mobile
- no raw device ids or model inventory in Basic mode
- no owner/admin/control-plane text in regular-member DOM
- all visible card actions route or submit correctly
- staged proof before production promotion

The closeout artifact should record the version, exact digest, screenshots, role snapshots, and any hidden cards or limitations.

## Public-Safe Lesson

The important architecture pattern is not the exact implementation. The lesson is that a private AI product gets more useful when its home screen becomes a truthful daily state summary with safe actions, role-aware visibility, and proof-backed release gates.
