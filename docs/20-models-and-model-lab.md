# Models and Model Lab

This page is the public-safe Nessa AI model page.

It explains representative model families, current validated model names, why those models are useful, and what was tested before they were discussed publicly. It intentionally does not publish private Auto routing rules, Pro entitlement behavior, prompt chains, Secure Connector internals, private eval suites, account-specific model selection, secrets, hostnames, IP addresses, device IDs, or complete live inventory.

## Current Public Answer

As of the 2026-05-09 CDT public-doc sync, the public answer to "which Gemma 4 model is Nessa using?" is:

- the cluster-side Gemma 4 label is currently `gemma4:26b`
- the Apple Silicon / MLX model lab also tracks `gemma4:e4b-mlx`
- these are model-lab artifacts and product lanes, not a promise that every request or every account uses the same model

The live OpenShift inference lane was verified with `gemma4:26b`, `qwen2.5:7b`, and embeddings in active use across the recent release window. The live M5 Max Linked Device record was also current, healthy, and reporting Apple M5 Max hardware, 128 GB unified memory, Metal runtime support, and MLX availability.

The 2026-05-09 validation pass also tightened a public-safe model-lab rule: device inventories should advertise only models that are actually installed and ready. For private vision/document work, the current public-safe Mac vision note is that `qwen2.5vl:7b` is the installed preferred lane. Stale unavailable model labels should be filtered from readiness displays instead of retried silently.

A later 2026-05-09 infrastructure pass added another public-safe rule: hot cluster-side model stores should live on the dedicated local NVMe cache tier when available, with any previous network-backed store kept as seed/rollback rather than the active hot path. That pass also reinforced that heavier models can be present in the local cache without being safe automatic warmup targets.

## How To Read This Page

Nessa uses a portfolio of local and open-weight models. A model can be useful even when it is not the default for a broad product path.

Public docs can say:

- what model families are being evaluated or used in validated lanes
- why a family is valuable
- whether a candidate proved fast, slow, stable, or risky in product-style testing
- what hardware class and runtime made sense

Public docs do not say:

- exactly how Auto chooses a model
- which private account gets which model
- premium selection rules
- fallback thresholds
- hidden prompts or private eval prompts
- raw connector payloads or device inventory rows

## Representative Model Families

| Model or family | Public role | Why Nessa uses or tracks it | Public validation notes |
|---|---|---|---|
| `gemma4:26b` | Current cluster-side Gemma 4 label | Stronger local response quality than the small fast baseline while still fitting the accelerated OpenShift lane | Verified warm on the live GPU-backed OpenShift lane during the 2026-05-07 sync; prior product proof showed free Gemma 4 generations resolving to this model |
| `gemma4:e4b-mlx` | Apple Silicon / MLX Gemma 4 lab artifact | Useful for comparing Gemma behavior on the M5 Max MLX/Metal path | M5 Max benchmark sample passed on 2026-05-07 with a 14.611s secure-connector probe |
| `qwen2.5:7b` | Fast local text baseline | Best practical balance of responsiveness, memory fit, and general local quality in the current OpenShift path | Strix Halo warm decode has repeatedly landed around 45-51 tokens/sec in production-style tests; it replaced slower 14B-class fast-lane behavior after testing |
| `qwen2.5:32b` / `qwen2.5:32b-instruct` | Heavier local quality comparison | Useful when depth matters more than immediate token cadence | Strix Halo tests showed roughly 10-13 tokens/sec warm and about 82s cold-load cost for the 32B class; M5 Max `qwen2.5:32b-instruct` probe passed in 21.765s |
| Qwen VL family, including `qwen2.5vl:3b`, `qwen2.5vl:7b`, and Qwen3-VL candidates | OCR, document, photo, and AI Vision exploration | Vision-language models are central to worksheets, document understanding, and family photo workflows | Live cluster artifact checks included Qwen 2.5 VL 3B and 7B; an M5 Max `qwen2.5vl:7b` benchmark sample passed in 95.355s |
| Qwen Coder family, including `qwen2.5-coder:3b-mlx`, `qwen2.5-coder:14b`, and Qwen3 Coder candidates | Coding and structured technical-output experiments | Code-specialized models are useful for shell, Python, YAML, Ansible, and fenced-artifact workflows | M5 Max `qwen2.5-coder:3b-mlx` benchmark sample passed in 8.094s; larger coder candidates stay in the lab until real-path quality and latency justify exposure |
| `gpt-oss:120b` | High-memory Apple Silicon large-model experiment | Validates that the M5 Max lane can run very large open-weight local models with truthful fail-closed behavior | M5 Max benchmark sample passed in 306.082s with a long response; this is a serious large-model lane, not a low-latency default |
| DeepSeek R1 distilled models | Reasoning-style comparison | Useful for comparing explicit reasoning behavior against latency, memory fit, and answer quality | Kept as a reasoning-family benchmark class; promotion requires real product-path proof, not only direct CLI success |
| `mistral-small:24b`, `mistral-nemo:12b-instruct`, `gemma2:27b-instruct`, `llama3.2:3b`, `phi3:mini-4k-instruct` | Comparison, fallback, and compatibility candidates | Older or smaller families help define baselines, compatibility, and regression behavior | Some candidates remain useful for comparison even when they are not the headline lane |
| `nomic-embed-text` and other embedding models | Retrieval and document search support | Small embedding models can matter as much as chat models for document workflows | Verified as part of the broader local model-serving inventory; embeddings are treated as product infrastructure, not demo extras |

## Why These Choices

The model lab optimizes for product behavior, not leaderboard headlines.

The 7B Qwen baseline stayed important because it returned practical, interactive responses on the Strix Halo OpenShift lane. The measured difference between the prior Qwen 2.5 14B-class fast behavior at about 24.7 tokens/sec and the Qwen 2.5 7B warm range around 45-51 tokens/sec was large enough to matter to real users.

The 32B Qwen family stayed useful because larger models can give better depth, but the measured cold-load and warm-token costs make them a poor blanket replacement for fast local chat. That is the core lesson: a larger model can be better and still not be the right default everywhere.

The same rule applies to warmup. A 32B-class artifact can belong in the local cache and remain available for deliberate tests, while the automatic warm set should stay limited to models whose first-load and memory behavior has proven safe under live service budgets.

The Gemma 4 work is valuable because it gives Nessa another strong local model family for quality comparison. The current public cluster-side answer is `gemma4:26b`, and the Apple Silicon lab also tracks `gemma4:e4b-mlx` for MLX/Metal experiments.

The GPT-OSS 120B work is valuable for a different reason: it proves the M5 Max high-memory lane can participate in very-large-model local experiments. That does not make it a universal default. It makes the Linked Device architecture more credible for high-memory private AI work.

The Qwen VL and image-model work is important because Nessa is not only a chat product. Worksheets, family documents, photos, redaction, OCR, and private image workflows all need models that can handle visual and document-heavy inputs. Those models are validated under a stricter standard because an answer that looks confident but misses the image is worse than an explicit failure.

## Runtime Discipline

Nessa does not force every model through one runtime.

| Runtime | Where it fits | Public-safe lesson |
|---|---|---|
| Ollama | Practical OpenShift-hosted GGUF serving and warm local model iteration | Useful when it gives stable product-path behavior with simple operations |
| `llama.cpp` / `llama-server` | Lower-level GGUF control, linked-device serving, and model-cache discipline | Useful when direct control and portability matter |
| MLX / Metal | Apple Silicon text, vision, and image-generation experiments | Best fit for the M5 Max unified-memory lane when the artifact is MLX-friendly |
| OpenShift AI / KServe-style serving | Governed model-serving lifecycle and repeatable platform validation | Useful for turning experiments into platform practices |

The public lesson is not "runtime A beats runtime B." The lesson is to test the model on the actual runtime, hardware, context size, warmup path, and product request shape that users will experience.

## Testing Standard

Before Nessa talks about a model publicly, the model should have enough evidence to avoid guesswork:

1. License and model-card review.
2. Artifact and quantization fit for the target lane.
3. Load proof on the real hardware class.
4. Cold-load and warm-load timing.
5. Time-to-first-token and token-cadence measurements where relevant.
6. Product-style prompts, not only one-line CLI probes.
7. Failure-mode proof: timeouts, empty responses, unavailable devices, and fallback behavior.
8. Staging proof before production exposure when the model affects a product path.
9. Browser or real-user-path proof for visible product behavior.
10. Logs checked after promotion.
11. Readiness truth: visible model labels should reflect installed, routable, recently healthy models rather than stale inventory.

This is why a direct benchmark can be promising and still not become a product lane. A model must survive the real request path, not only a lab prompt.

## What Is Intentionally Omitted

This page omits:

- Auto routing rules
- Pro or premium model-selection behavior
- per-user model availability
- private prompt suites
- raw eval data
- Secure Connector protocol details
- live device IDs and account IDs
- private hostnames, IP addresses, and route names
- exact model-cache paths
- proprietary Learning / Homework Buddy logic

That boundary is deliberate. Public readers should understand the architecture and the reasoning without receiving a clone recipe for private Nessa internals.

## Current Lab Snapshot

Public-safe snapshot from the 2026-05-09 CDT sync:

- Strix Halo / Ryzen AI Max+ 395, 128 GB remains the OpenShift AI worker lane for cluster-side local inference and model-serving experiments.
- MacBook Pro M5 Max, 128 GB remains the Apple Silicon Linked Device lane for private MLX/Metal, vision, image, and high-memory local model tests.
- The cluster-side model store pattern now uses a dedicated local NVMe cache tier for hot model artifacts, with the older network-backed store treated as seed/rollback during migration.
- The recent live cluster warm set included `qwen2.5:7b`, `gemma4:26b`, and `nomic-embed-text`.
- The live cluster model list included `gemma4:26b`, `qwen2.5:7b`, `qwen2.5:32b`, Qwen VL 3B/7B artifacts, Qwen3 Coder 30B, and `nomic-embed-text`.
- `qwen2.5:32b` remains a useful heavier local comparison model, but it is not treated as a blanket automatic warmup target without controlled proof.
- The current public Gemma 4 answer is `gemma4:26b` on the cluster lane, with `gemma4:e4b-mlx` tracked on the Apple Silicon / MLX lane.
- The M5 Max record remained current, reported Metal and MLX availability, and had passing public-safe samples for `gemma4:e4b-mlx`, `qwen2.5-coder:3b-mlx`, `qwen2.5:32b-instruct`, `qwen2.5vl:7b`, and `gpt-oss:120b`.
- The Mac-to-Linux direct sideband pattern is useful for artifact and model-cache movement, but public docs should describe the measured link class and architecture pattern rather than private interface details.
- The product path also reinforced a practical truth rule: an image or media feature should either return a real artifact through a proven lane, show a clear limitation, or stay out of the UI. In the current public-safe architecture, private generated-image workflows remain on the Linked Device lane unless a separate OpenShift-hosted image service is actually deployed and validated.

This snapshot is intentionally public-safe. It is not a complete live inventory and it is not a routing map.
