# Mobile checkout and a useful first task

Published November 12, 2026. This continues the [four-product direction](68-outcome-focused-ai-products.md):
help someone complete useful work, then offer capacity or continuity worth paying
for. The latest release removes friction between choosing a feature, setting up
an account, paying, and doing the first task. It is a commercial experiment;
conversion uplift, real customer revenue and profitability remain unproven.

| Product | What changed | First useful outcome |
| --- | --- | --- |
| [PRIORITYmicro](https://prioritymicro.com/) | A visible phone action, three-step workbench, clear CURRENT workspace handoff and truthful checkout return | Generate and download a draft before signup; preserve its versions when signing in. Existing paid capacity starts at $5/month. |
| [XpressContact](https://xpresscontact.com/) | Keep the chosen feature through email/MFA setup; guide free-card owners toward sharing or optional Pro capture; refresh verified paid state | Preserve the existing free card and link. Optional $4.95/month Pro collects a consented enquiry into a private inbox. The separate NFC card remains $15.95 with free standard U.S. shipping. |
| [AiZipZap](https://aizipzap.com/) | Keep the selected plan through email sign-in and cancellation; fix same-tab verification; put request-page setup next to active access | Receive a request, review an AI brief and reply draft, download the handoff, and save the next action. Starter remains $9/month. |
| [TryNessa](https://www.trynessa.com/) | Resume the chosen plan after sign-in, keep existing chat drafts ahead of purchase intent, and show a first-task path after verified activation | Continue everyday chat, Learning and private documents. Family remains $9.99/month, Pro $19.99 and Family Pro $24.99. |

## What was tested

Four subscription purchases and a separate physical-card purchase completed in
an isolated Stripe sandbox using published test cards. Correct signed provider
events activated access. Cancelled and declined checkout recovery, self-service
renewal cancellation, ended-plan access and retained work were exercised. New
public capture paused when access ended; existing cards and unexpired work stayed
available. No real charge or shipment was created. Tests are not customer sales.

Actual browser phone viewports at 360, 390 and 430 pixels, plus desktop, exercised
layout and navigation. The full sandbox purchase paths ran on the 390-pixel
viewport. Production browser checks and live price validators followed promotion
of the exact staged artifacts. There were 139 targeted automated tests and three
static contract checks. These are responsive browser tests; native phone contact
import, physical NFC taps, production email delivery, real payment settlement,
refunds, disputes and renewals over time were not tested in this release.

Webhook testing included transient non-success responses; later signed events
established the correct account states. We do not claim flawless delivery or
proof of every retry. AI drafts still require human review: payment and workflow
verification do not certify every answer's correctness or instruction adherence.

## What success means next

Measure genuine paid activation, a completed useful task, repeat use, retained
subscriptions and contribution margin after payment fees, inference, storage,
hosting and support. Exclude sandbox purchases and synthetic QA from customer and
revenue claims. Existing infrastructure can support these bounded workflows;
unused capacity does not establish demand or make operating costs zero.

Existing accounts and product boundaries remain independent. Choosing a plan
does not automatically submit payment. Limits do not trigger overage charges,
and replies are not sent automatically. Tests and empty states should explain
what really happened instead of manufacturing a successful customer outcome.
