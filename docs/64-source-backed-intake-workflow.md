# Source-backed intake: a small, inspectable AI workflow

September 2026. A public architecture note from PRIORITYmicro, the team building TryNessa and AiZipZap. This note was prepared with AI assistance and checked against the implementation.

## Start with the handoff

An agent becomes easier to evaluate when its job has a clear beginning and end. For customer intake, the input is a request that a person is authorized to process. The output is a reviewable handoff: source excerpts, missing questions, proposed next steps and a reply draft.

The current [AiZipZap Intake Agent](https://aizipzap.com/agent?utm_source=github&utm_medium=reference&utm_campaign=intake_launch) implements this bounded workflow. It does not connect an inbox, send messages, execute code or update business systems. Its original [rule-based redaction tool](https://aizipzap.com/upload) remains available separately. These are AiZipZap capabilities, not newly enabled TryNessa family features.

## Keep evidence under application control

Asking a model to repeat a source quotation is not sufficient. A model can paraphrase, choose the wrong passage, or return malformed output. A stronger pattern is to let the application retain the source segments and let the model select references. The application resolves those references back to the original text, validates the result shape, and rejects invalid selections.

This establishes a narrow claim: the displayed excerpt came from the submitted source. It does not establish that the source is true, the selected excerpt is relevant, the categorization is correct, or every requirement was found. Those remain review questions.

## Constrain work before adding autonomy

- Use one fixed task with bounded input and output, rather than a public arbitrary tool runner.
- Keep model inference separate from customer accounts, payment state and private product databases.
- Bound concurrency, timeouts and total attempts so failed requests cannot consume unlimited capacity.
- Treat operational logs as operational evidence. A successful generation is not a customer, a paid conversion or money saved.
- Present failure plainly, preserve the source for correction and avoid manufacturing an output when the model fails.
- Require renewed review after the user changes an editable draft. A download request is not proof that a customer received a message.

Prompt instructions alone are not a security boundary. Obvious instruction-in-source attempts can be rejected before inference; that does not prove resistance to every prompt-injection strategy. The more meaningful limit here is the absence of external tools and the application's control over displayed source evidence.

## Evaluate the outcome people might pay for

Use synthetic examples for release checks, then authorized, sanitized examples for a customer pilot. Record whether the first reply was usable, which requirements were missed, what needed correction, and how long a human spent reaching an acceptable handoff.

Compare similar work and include review time. Do not advertise a time-saving percentage until the measurement supports it. A short sequence of successful test generations does not establish production reliability or market demand.

The [customer intake checklist](https://aizipzap.com/customer-intake?utm_source=github&utm_medium=reference&utm_campaign=intake_launch) and [product launch note](https://aizipzap.com/intake-launch?utm_source=github&utm_medium=reference&utm_campaign=intake_launch) explain the current public experience. The [PRIORITYmicro pilot offers](https://prioritymicro.com/pilots/?utm_source=github&utm_medium=reference&utm_campaign=intake_launch) test different audiences through separately scoped proposals; enquiries are not revenue.

## TryNessa stays a separate product

TryNessa's standard product remains Chat, Learning, saved Chat History and Linked Devices. Sharing an operator's infrastructure does not imply shared user records, a shared subscription, or permission to transfer one product's customer data into another. See the [current product boundary](CURRENT_PRODUCT_BOUNDARY.md).
