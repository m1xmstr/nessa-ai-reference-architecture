# Public Demo Scripts

These scripts are public-safe. They use synthetic examples and avoid private accounts, private screenshots, internal routes, hostnames, IP addresses, tokens, logs, and cluster console views.

## 60-Second Founder Demo

Goal: show the product promise quickly.

Script:

1. "Nessa is private AI for real family life."
2. "The first thing you see is simple: ask Nessa. No operations dashboard, no model inventory."
3. "For homework, Nessa checks the student's thinking and anchors to the right problem instead of dumping a generic answer."
4. "For documents, Nessa can summarize and redact sensitive details while keeping the artifact in the user's workspace."
5. "For family use, roles matter. Owners manage controls; learners get a safer experience."
6. "For private compute, Nessa can use approved Linked Devices when they are healthy, but the product does not depend on a cable or a single workstation."
7. "The platform behind this is OpenShift with release gates: staging proof first, production promotion second."

Close:

> Nessa is useful because it combines family workflows, private compute, and release discipline into one product.

## 5-Minute Technical Demo

Goal: explain the architecture without leaking the implementation.

Flow:

1. Landing page: show the hero and five proof cards.
2. Architecture: show the sanitized diagram in [38-public-demo-and-marketing-pack.md](./38-public-demo-and-marketing-pack.md).
3. OpenShift story: explain web/API, durable storage, inference lanes, and release gates at a pattern level.
4. Linked Devices: explain user-approved private compute and honest fallback.
5. Learning quality gate: use the synthetic algebra case:
   - problem: `v^2 - 4 = 12`
   - student reasoning: "I need v squared to equal 16. Is the answer 4?"
   - expected Nessa behavior: "Correct step. The complete answer is v = 4 or v = -4."
6. Documents/redaction: show the synthetic redaction sample from [40-public-demo-gallery-and-launch-checklist.md](./40-public-demo-gallery-and-launch-checklist.md).
7. Release discipline: explain that staging-to-production deploys must pass browser, legal, family, document, learning, linked-device, mobile, and release-truth checks.

Do not show:

- private cluster console
- owner account data
- production logs
- invite tokens
- private device identifiers
- internal prompts or prompt chains
- exact model-routing rules

## 10-Minute Family Workflow Demo

Goal: show a real family day without fake data.

Flow:

1. Start on the public landing page.
2. Create or open a clean demo/free account with only synthetic data.
3. Ask Nessa a normal family prompt: "Help me make a quick plan for homework, dinner, and one document I need to review."
4. Upload or reference the synthetic worksheet.
5. Ask: "For problem 2, I think v squared equals 16, so the answer is 4. Am I right?"
6. Show Nessa validating the reasoning and correcting the missing negative solution.
7. Upload or reference the synthetic family document.
8. Ask for a summary and a redacted copy.
9. Show that sensitive fields are masked in the redacted output.
10. Open family-safe explanation at a high level: owner controls exist, learners do not see owner/admin surfaces.
11. Mention Linked Devices as private compute that can help when approved, not as a required dependency.
12. End by showing the launch checklist and explaining what is safe to show publicly.

Success criteria:

- no private account or screenshot appears
- no stale tutor session or dead route appears
- no model telemetry dominates Basic user view
- no severe weather, platform alert, or maintenance claim appears without source proof
- the demo can continue if private compute is unavailable

## Presenter Notes

Use this language:

- "private by design"
- "user-approved private compute"
- "staging proof before production"
- "family roles shape the experience"
- "document behavior matches retention and deletion policy"

Avoid this language:

- "fully autonomous"
- "always faster"
- "guaranteed private under every possible workflow"
- "unlimited local compute"
- "production cluster details"
- "secret model routing"
