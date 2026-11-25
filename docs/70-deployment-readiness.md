# How to score deployment readiness

Use this worksheet to decide whether you have enough evidence to repeat a release, support more traffic or recover a service. Score those decisions in their own context. A delivery team can pass a release rehearsal and still need to test recovery on a clean platform.

**Every weight, result and forecast in the worked example below is invented for a fictional library application.** The example uses no TryNessa deployment measurements.

## Define the decision before scoring

1. Name the target environment, release artifact, expected workload and decision. Choose four observable checks per area. Give each an owner, an acceptance condition and a place to keep dated evidence.
2. Choose weights that add to 100. A digital archive might favor data integrity, while a classroom assistant might put more weight on answer quality and learner privacy. Agree on any blocking conditions independently of the weights.
3. Give a check **P = 1** when direct evidence covers its stated scope, **L = ½** when the evidence is partial, older or narrower, and **U = 0** when the check is unmet or evidence is missing. Retain unknowns in the denominator. A check that genuinely does not apply should be replaced with a relevant one before scoring.
4. Multiply the sum of four check credits by 25 to get an area score out of 100. Multiply each area score by its weight divided by 100, add the contributions, then round once to one decimal. Record the date and rescore affected checks when a dependency or artifact changes.

## Worked example for a fictional library application

The library runs catalogue search, a study assistant and optional paid events. Each letter corresponds to the check in the same position, from left to right. An operator using the worksheet would attach evidence privately to each check.

| Area | Example checks, in order | Weight | Invented results | Score /100 |
| --- | --- | ---: | --- | ---: |
| Platform and network | Serve a public request, replace a pod, recover a node, renew a certificate | 10% | P P L P | 87.5 |
| Compute | Return a search result, serve a cold request within budget, sustain peak traffic, lose a worker | 8% | P L U U | 37.5 |
| User journeys | Search catalogue, reserve a book, revisit a saved study session, cancel a reservation | 12% | P P L P | 87.5 |
| AI quality | Follow a requested format, decline an unsupported answer, cite an updated source, pass a representative evaluation set | 5% | L L P U | 50.0 |
| Data and files | Reload a saved record, restore the catalogue database, retrieve a stored cover, reconcile metadata | 12% | P P P L | 87.5 |
| Security and privacy | Scope access by patron, revoke a test session, enforce a role boundary, verify retention end to end | 12% | P P P L | 87.5 |
| Backups and recovery | Complete a backup, restore a search index, recover selected records off-site, rebuild the service within target time | 12% | P L P U | 62.5 |
| Delivery and rollback | Pin an image, exercise a canary, rehearse rollback, verify the released user journey | 8% | P P L P | 87.5 |
| Automation | Run a scheduled job, recover an interrupted job, reject a duplicate event, deliver an audit record | 6% | P L P P | 87.5 |
| Monitoring | Measure search success, deliver a test alert, meet a 30-day objective, reproduce an incident timeline | 6% | P P U L | 62.5 |
| Billing | Retain a selected event, confirm test checkout, verify cancellation, reconcile settlement | 4% | P P L U | 62.5 |
| Documentation | Identify a release, locate its rollback, complete an independent handoff, find the data owner | 5% | P P L P | 87.5 |

The fictional baseline is **76.1/100**. The recovery row contributes 7.5 points to the total. If the library completes a timed rebuild, its fourth recovery check changes from U to P. That adds 3.0 weighted points, bringing the estimate to **79.1/100**. This forecast assumes all other evidence stays the same. It gives a value to collecting evidence without assigning a probability to release success.

## Decide what to fix and what to publish

Prioritize a failed condition that blocks the decision, even when another check would add more points. For the fictional library, an approved rebuild time matters before a hosting move. Before a busy registration day, the peak-traffic and worker-loss checks matter more. For each intervention, record the proposed owner, the test, the expected result and the evidence that would change the rating.

Share the rubric, an invented example and general validation patterns publicly. Keep real scores, dates attached to control gaps, incident details, recovery dependencies, test receipts and remediation priorities in an access-controlled operator record. A public example can teach the calculation without allowing readers to track changes in a live service's defenses. [Public scope and redactions](00-public-scope-and-redactions.md) · [Release validation discipline](10-validation-and-release-discipline.md) · [Upstream platform options](71-upstream-platform-options.md)
