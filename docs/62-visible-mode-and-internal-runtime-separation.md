# Visible Modes and Internal Runtime Separation

Private AI products should not make users choose an internal inference route. A response-mode control is a product promise; a model server, accelerator, or speculative decoding path is an implementation detail.

This distinction becomes important when an experimental mode is retired from the interface but the same runtime remains useful behind governed modes such as Auto or Thinking.

## The contract

Use two independent policy switches:

- runtime eligibility decides whether the internal inference path may be used by governed routing;
- explicit-mode eligibility decides whether a user may request that path as a standalone mode.

Disabling the explicit mode must not silently disable a healthy internal runtime. Likewise, enabling a runtime must not automatically publish a new selector option.

## Handling stale explicit requests

Old tabs, saved clients, and direct API callers may continue sending a retired mode value after the UI removes it. Treat that input as an explicit policy event:

1. recognize the retired value;
2. reject it for unauthorized accounts;
3. for an authorized account, resolve it to a visible supported mode such as Auto;
4. provide a short user-facing fallback notice;
5. do not activate an explicit route lock;
6. allow the governed mode to choose any eligible internal runtime normally.

The important proof is the user-visible mode and route-lock state. Seeing the internal router choose the same accelerator afterward does not mean the retired explicit mode was honored.

## Release-gate design

A useful release gate checks both layers:

- the retired option is absent from guest, member, child, and privileged selectors;
- unauthorized raw requests are denied before routing;
- an authorized stale request resolves to a supported visible mode;
- the explicit route lock remains inactive;
- the response contains truthful fallback copy and a non-empty final answer;
- ordinary Auto and Thinking requests may still use the internal runtime when policy allows;
- no internal route, model, hardware, or endpoint identifiers leak into basic product surfaces.

Run these checks on the exact staged image, then repeat the policy-critical subset in a production canary before digest promotion.

## Common gate mistake

Do not define success as “the final internal route was different.” That assertion conflates two separate decisions and will fail whenever Auto legitimately selects the internal runtime.

Instead, assert:

- retired explicit request was recognized;
- visible mode resolved safely;
- explicit lock is inactive;
- governed routing remained healthy.

## Public-safety boundary

Public evidence can describe the two-switch policy, fallback semantics, route-lock checks, and exact-artifact release discipline. It should not publish private model filenames, hostnames, account allowlists, hardware identifiers, endpoint addresses, routing heuristics, or production diagnostics.

## Practical takeaway

Keep the product vocabulary stable and human-readable. Experimental runtimes can mature, move behind Auto, or be retired without leaving a fake selector, breaking old clients, or giving up a useful private inference lane.
