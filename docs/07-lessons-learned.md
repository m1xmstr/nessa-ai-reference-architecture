# Lessons Learned

## 1. Platform discipline matters before model size
The most valuable early improvements were not glamorous.

They were things like:
- cleaner rollouts
- fewer noisy logs
- healthier PDBs
- better readiness checks
- cleaner routing fallbacks
- correct database state

## 2. CPU-first is a legitimate first stage
A compact cluster can host a real private AI application if you are realistic about:
- prompt sizes
- concurrency
- model sizes
- latency expectations

## 3. One stronger worker is often better than many weak experiments
A single dedicated AI worker improved the platform more than a pile of loosely managed side systems would have.

## 4. Storage boundaries matter
If you run ODF or local storage on a compact cluster, keep storage eligibility explicit.
Do not let AI experiment nodes silently drift into your storage topology.

## 5. Upgrades expose architectural truth
Cluster upgrades are one of the best reality checks you can get.

They reveal:
- weak PDBs
- bad node assumptions
- brittle networking
- noisy but harmless alerts
- real operator problems

## 6. Alert hygiene matters
Not every alert deserves a code or infrastructure change.
But every repeated alert deserves a clear explanation.

The right outcomes are:
- fix the root cause if it is real
- document and silence it if it is historical or an accepted environmental constraint
- do not leave the UI in a permanently noisy state

## 7. Demo readiness is a real engineering requirement
A notebook, a KServe endpoint, and a repeatable operator workflow are not vanity extras. They make the platform easier to explain, validate, and support.
