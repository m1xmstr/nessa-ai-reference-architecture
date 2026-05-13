# Access, Support, Referrals, and Donation Boundaries

## Purpose

This document captures public-safe patterns for support, referrals, billing labels, and donation flows in a private family AI product. It does not publish product source code, private routes, Stripe keys, customer records, account identifiers, checkout session IDs, screenshots, or entitlement implementation details.

The core lesson is simple: support/donation flows must never look like a paid-plan entitlement path unless the backend really grants that entitlement.

## Access And Plan Labels

Plan labels should come from the same backend truth used by enforcement.

Good patterns:

- render the effective plan, not a stale default plan
- include owner/member role when the role changes available controls
- update visible state after family or entitlement data loads
- keep request-upgrade CTAs hidden or disabled for users who already have active access
- preserve honest zero states instead of placeholder upgrade copy

For a family product, this matters because a family owner, adult member, child/restricted member, free user, and guest can all have different access boundaries.

## Donations Are Not Entitlements

Optional support can help sustain the project, but it should not imply plan access.

Public-safe wording should state:

- donations are optional
- donations are separate from account access
- donations do not change plan, quota, feature gates, or support priority unless the product explicitly implements that rule
- donations are not represented as tax-deductible unless the organization has the legal basis to say so

Backend responses should carry the same truth. A useful invariant is:

```text
donation.grants_entitlement == false
```

When a product does have paid plans, paid-plan checkout and optional support checkout should be separate flows with separate copy.

## Stripe Boundary Pattern

Staging should use Stripe test mode only. Production should use live mode only for real user-facing flows, not automated QA.

Recommended guardrails:

- staging refuses live Stripe keys
- E2E suites refuse production hosts
- frontend payloads never expose secret keys
- frontend payloads do not need publishable keys when checkout sessions are created server-side
- server responses include explicit success and cancel URLs
- failed or disabled checkout returns non-500 safe copy
- cancel/back flows make it clear no payment was made
- logs and screenshots mask checkout identifiers, customer identifiers, tokens, invite emails, and keys

If staging lacks test-mode credentials, the correct result is a safe disabled state, not a live checkout probe.

## Referrals

Referral surfaces need the same honesty:

- create a real code or link, or hide the control
- show whether referral rewards are active, pending, or unavailable
- do not imply paid entitlement unless the reward is actually granted by backend policy
- protect invite/referral tokens in logs and screenshots
- clean up seeded referral state in staging tests

## E2E Checklist

Useful staging-only checks:

- Access & Support shows the effective plan and role
- support copy matches Terms and privacy policy
- donation API says it does not grant entitlement
- checkout disabled/failure state returns a non-500 safe response
- hosted test checkout opens only with test-mode credentials
- cancel/back returns to a clear no-payment page
- receipt/success page does not overclaim access
- referral code/link creation works or the UI is hidden
- no live keys or secret tokens appear in frontend payloads, logs, screenshots, or videos
- cleanup removes seeded users, referrals, and test checkout records where possible

## Public Boundary

This repo intentionally omits:

- Stripe keys, checkout session IDs, webhook secrets, customer IDs, or price IDs
- private entitlement schemas and admin grant tooling
- private route names, account records, and test-user identities
- screenshots or videos containing user data
- source-code implementation of billing or referral handlers

The reusable pattern is the boundary discipline: support can exist without creating false entitlement, staging can prove checkout safely without touching production live money paths, and plan labels must match backend truth.
