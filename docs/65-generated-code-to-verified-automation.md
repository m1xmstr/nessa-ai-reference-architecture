# From generated code to a verified automation

September 2026. A public engineering note from PRIORITYmicro. Prepared with AI assistance and checked against an actual TryNessa coding exercise and reviewed Ansible execution.

## Begin with a result someone can inspect

Our [Make One Useful Thing walkthrough](https://prioritymicro.com/proof/?utm_source=github&utm_medium=reference&utm_campaign=make_one_useful_thing) starts with two small tasks: summarize synthetic sales rows in Python, and check public websites with Ansible. The page includes actual Chat captures, an explicitly edited video, and the exact downloadable examples.

The Python example passed its three included tests. The Ansible draft needed error feedback and review before its four HTTPS checks passed in both local Ansible and Ansible Automation Platform, with zero changes. Earlier failures are part of the result. Nessa did not automatically execute or deploy the generated code.

## Use different checks for different claims

| Check | What it establishes | What it does not establish |
|---|---|---|
| Python or YAML parses | The text has valid syntax | Correct calculations, valid module arguments or useful tests |
| Tests are discovered | The runner can find the intended cases | That the assertions cover real failure modes |
| Included tests pass | Those cases passed in that environment | General correctness, safety or production readiness |
| Ansible checks return successfully | The reviewed task completed against its named targets | Working checkout, model quality or customer demand |
| AAP records a successful job | The approved source ran through Controller | That an arbitrary model draft is safe to schedule |

A small example is useful when its limits are visible. Three passing tests are not a complete accounting system. A responding homepage is not a functioning purchase flow. Inspect both the output and the evidence behind the status label.

## Put the reviewed artifact into operations

Keep drafting, review and execution as separate steps. Read the entire generated file. Confirm its destinations and permitted actions, validate module options, test representative failures and run it in an appropriate test environment. Save the reviewed source in version control, then point an AAP job template at that source with only the credentials it needs.

For routine observations, begin with read-only tasks: public-page availability and expected content, release-currency observations, and aggregate activation reports. Use bounded timeouts and fail when evidence is incomplete. Store the aggregate result in Controller history; avoid logging authenticated request objects, tokens, customer records or private visitor identifiers.

An EDA listener accepting a request is a separate checkpoint from a rule matching and a Controller job finishing. Verify the complete event path before calling an event-driven workflow operational. A listener health probe should not accidentally trigger business actions.

## Evaluate changes before scheduling their promotion

New model and runtime releases are candidates for evaluation. Compare the same task, hardware and conditions; inspect quality, latency and failure cases. A faster image model can still be worse at the subject or geometry that matters to the product.

Use OpenShift AI pipelines for repeatable quality checks, then exercise the actual application route. A passing model-level evaluation can miss a web routing shortcut, a broken artifact download or a confusing error state. Build an immutable candidate, validate it in staging and a controlled canary, and promote that artifact only after the required checks pass.

Release-currency automation should collect evidence and identify a candidate. It should not silently replace a production model or perform a major platform upgrade merely because a newer version exists.

## Measure useful work separately from traffic

A product screenshot can invite exploration; it cannot prove a customer outcome. Keep visits, completed first tasks, saved accounts, qualified enquiries and paid work as separate measures. Label synthetic examples and exclude operator checks where possible. Attribution is a clue about where interest came from, not proof that a campaign caused a sale.

The [public walkthrough and source examples](https://prioritymicro.com/proof/?utm_source=github&utm_medium=reference&utm_campaign=make_one_useful_thing) let a reader inspect one modest result before bringing a larger problem. TryNessa remains a separate product with the [current access boundary](CURRENT_PRODUCT_BOUNDARY.md); this workflow does not enable arbitrary public code execution or autonomous actions.
