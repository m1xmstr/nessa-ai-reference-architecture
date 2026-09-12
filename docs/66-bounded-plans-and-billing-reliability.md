# Useful free products and bounded monthly plans

Verified September 12, 2026. These are public product offers, separate from the architecture examples in this repository. Prices are USD per month; the linked product pages define current allowances and terms.

| Product | Free first result | Monthly plans |
| --- | --- | --- |
| [TryNessa](https://www.trynessa.com/pricing) | Chat, Learning and supported creative/attachment flows with finite usage | Family $9.99; Pro $19.99; Family Pro $24.99 |
| [CURRENT](https://prioritymicro.com/current/pricing/) | Try an artifact without an account; preserve a small private project library after sign-in | Starter $5; Plus $12; Studio $29 |
| [AiZipZap](https://aizipzap.com/pricing) | Three successful intake drafts per network per UTC day, subject to capacity | Starter $9; Pro $29; Business $79 |
| [XpressContact](https://xpresscontact.com/pricing) | Create and share a standard digital business card | Pro $4.95; Studio $9.95; Business $29 |

Optional consulting is scoped separately. Trying a free tool or making an enquiry creates no subscription. CURRENT's existing $120 annual Plus offer and XpressContact's optional $15.95 physical card remain separate choices.

## What the products deliver

TryNessa combines ordinary Chat and Learning with user-controlled Linked Devices where supported. The monthly plans buy documented capacity. Generated-token and image allowances are shared by a linked household; storage is limited per account. Private work is not published by default. Configured external services may process requests under the product's privacy policy; private AI does not mean every request stays on the user's device.

CURRENT turns a goal and explicit context into a saved, versioned Plan, Decision, Lesson, Creative Draft or Script. Text exports are available on Free; paid plans add ZIP exports and larger project/history/share allowances. Generated scripts remain text for review, without automatic execution.

AiZipZap drafts a handoff with source quotes, missing-information questions and a review step. It does not read an inbox, send messages, execute code or update another business system. Paid plans increase bounded intake capacity for one account; Business does not imply multiple seats. The separate rule-based redaction utility retains its own free limits.

XpressContact paid plans add card appearance controls, an optional HTTPS button and aggregate activity reports. Reports count requests rather than people or confirmed address-book saves. Standard card links survive cancellation.

## Reliability patterns

- Confirm the real provider price and currency before opening checkout.
- Separate each product's identity, customer mapping and event purpose; a payment for another product cannot grant access.
- Reserve usage durably before work, fence concurrent jobs, and recover expired reservations.
- Re-fetch signed billing events from the provider so late events do not restore canceled access.
- Keep account deletion from orphaning an unfinished checkout or subscription.
- Keep upgrades, period-end downgrades and cancellation usable in the billing portal.
- Make quotas visible and offer a deliberate plan change; do not apply automatic overage charges.
- Test real outputs, storage and downloads alongside provider sandbox payments. A successful build, HTTP response or synthetic purchase is not a new customer.

This release did not replace the production models or establish a benchmark advantage over another AI service. Product differences are the working workflows, review and persistence controls, supported compute options and published limits. Internal source, account records, credentials and release access links are not included here.
