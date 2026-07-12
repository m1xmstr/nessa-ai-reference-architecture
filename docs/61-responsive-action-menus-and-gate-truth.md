# Responsive action menus and release-gate truth

Small interaction defects can invalidate an otherwise strong private-AI workflow. A menu may exist in the DOM, have working event handlers, and pass desktop tests while remaining unusable on a phone because an ancestor clips it. A release gate may also reject a correct product response when its assertion is narrower than the public contract it is meant to prove.

This note captures two public-safe patterns for avoiding those failures.

## Menus must escape containment traps

Compact learning and family workflows often place primary actions in a horizontally scrollable row. That layout works for buttons, but it becomes a containment trap when one item opens a dropdown:

- `overflow-x: auto` commonly establishes clipping behavior for the other axis too;
- an absolutely positioned submenu can be present and semantically visible while still being cut off by the row;
- keyboard and accessibility snapshots may list the submenu even though a person cannot see or tap it;
- narrow-screen rules that assign every child `width: 100%` can make the disclosure control look unrelated to its neighboring actions.

For a compact action row that includes a disclosure menu, prefer a wrapping row with visible overflow at the relevant breakpoint. Keep the disclosure trigger sized like the other actions and confirm the opened choices remain inside the viewport.

The proof should combine:

1. a source-level responsive contract;
2. a real mobile viewport on the actual workflow route;
3. the disclosure opened, not merely present;
4. every choice reported as visible and available to interaction;
5. a broader mobile route matrix to catch adjacent overflow regressions.

## Preserve user-authored identity in revisions

Template finalizers should distinguish between a missing placeholder and user-authored content. When revising an existing email, a sign-off already present in the draft is part of the user's document unless a new sender name explicitly replaces it.

A safe precedence rule is:

1. explicit sender name from the current request;
2. valid sign-off recovered from the existing draft;
3. a neutral placeholder.

Recovered names still need an internal-name guard. Service accounts, environment labels, admin constants, and assistant identities must never become the visible sender simply because they resemble a name.

## Gates must test the product contract

Release assertions should normalize presentation details that the product contract treats as equivalent. Branding checks are a useful example: a case-sensitive search for one legacy capitalization can reject a correctly branded page even when the page contains the current product name, tagline, recovery action, and safe error copy.

Good gate behavior:

- normalizes case when capitalization is not part of the contract;
- checks several meaningful signals instead of one brittle literal;
- records the exact candidate digest the gate evaluated;
- separates a product failure from a gate-harness failure;
- requires a rebuilt and reverified candidate when packaged release tooling changes.

Bad gate behavior:

- treats one capitalization or old marketing phrase as permanent truth;
- accepts a route because it returned an HTTP status without checking the visible recovery experience;
- relaxes a threshold after a noisy result instead of rerunning the unchanged warm candidate;
- promotes an image after changing files that are packaged into that image.

## Practical release sequence

For a focused responsive and template-continuity repair:

1. reproduce the visible mobile failure on the real workflow;
2. make the smallest containment or finalization repair;
3. add focused regression coverage;
4. run the recent-change suite to distinguish real regressions from stale expectations;
5. build staging images and run model-quality plus release-truth gates;
6. open the exact control at a narrow viewport and verify each revealed action;
7. promote the exact verified image digests;
8. repeat the targeted browser proof in production;
9. verify versions, workloads, logs, and inference warmth;
10. document both product fixes and gate-harness corrections truthfully.

The broad lesson is that private AI quality is not only model quality. It includes whether a learner can reach every help action on a phone, whether a revised document still belongs to its author, and whether the release system recognizes the product's actual public contract.
