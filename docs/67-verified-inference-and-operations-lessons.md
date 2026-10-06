# Verified inference and operations lessons

Published October 6, 2026. This summarizes selected work from September 15 through
October 5. It records engineering lessons, not a promise that every experiment is
available to every account or that historical checks prove current service health.

## Apple Silicon workers and MTP

The M5 Max remains a useful high-memory Apple Silicon reference. Independent
workers can serve different qualified tasks without pretending their memory is
one shared GPU. Retiring a worker means removing its routing and authorization,
then proving that the remaining lanes still complete real requests. A personal
laptop needs an honest unavailable state when it travels or is signed out.

Multi-token prediction (MTP) must be proven by accepted draft-token counters and
completed requests. A model alias or enabled setting alone is insufficient.
Recent qualification checked prose, strict structured output, concurrent streams,
and application routing after resolving a runtime-option compatibility issue.
Unknown per-request hardware or draft counts should remain unknown in diagnostics.
A direct provider result does not establish chat persistence; reload and stored
answers need their own tests. Short synthetic measurements are not general speed
or capacity guarantees. Earlier preview notes remain historical qualification
patterns, not a complete inventory of current serving lanes.

## Knowledge freshness and retrieved sources

Decoding acceleration does not update a model's training knowledge. A product
using several models should not invent one universal knowledge cutoff or imply
continuous retraining. Explicit lookup requests need to reach the retrieval tool,
including ordinary polite phrasing. Retrieved snippets should stay attributed to
their sources rather than being presented as a verified answer. Recovery from
uncertainty should preserve private conversation and uploaded-document boundaries.

## Safety and predictable scheduled messages

Harmless portrait requests still require independent output checks: benign intent
does not guarantee a safe image. Fix generation conditioning while retaining the
output safeguard, and validate the reported request through the actual product.
For scheduled owner messages that need predictable language, approved templates
can be a better fit than unconstrained generated text. Test dispatch and stored
state with isolated data and a fake sender before relying on the schedule.

## Recovery needs readback

Recent maintenance reinforced several reusable practices:

- Verify stored bytes across clients and after restore; healthy pod status alone
  does not prove data integrity.
- Separate a recovered service from a diagnosed and permanently repaired hardware
  fault. Preserve evidence and state the remaining uncertainty.
- Check certificate rotation from the real client path while retaining TLS
  verification. A new certificate on disk does not prove every process reloaded it.
- Repair alert delivery and clock ordering at their cause; keep alert thresholds
  and safeguards truthful.
- Measure archive memory use and read back archived content with hashes. Archive
  integrity does not by itself prove guest boot or a full disaster recovery.

## Closing the publication gap

A deployment and a local commit are separate from a published GitHub update.
Reconcile the deployed artifact with its source, verify the remote default branch
contains the source commits, and explicitly track reviewed validation evidence.
Ignored local evidence can otherwise disappear from the published record.

Private operational records and public architectural lessons have different
boundaries. At each material closeout, record either a sanitized public update with
its commit or a reason to skip it. Never mirror private source, credentials,
customer data, endpoint details or raw production evidence into the public repo.

See [release documentation checks](28-release-runbook-truth-sync.md),
[Linked Devices](04-linked-devices-public-pattern.md), and the
[current product boundary](CURRENT_PRODUCT_BOUNDARY.md).
