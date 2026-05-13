# Dual Sideband Transfer Pattern

This note documents a public-safe pattern for using two direct workstation-to-AI-node links without making the product depend on those cables for normal traffic.

The key lesson is simple: treat multiple direct links as optional sideband paths for large eligible payloads, not as one imaginary wider pipe and not as the default route for the application.

## What To Measure

Measure each link independently before changing routing or user-visible claims.

Recommended minimum proof:

- TCP single-stream throughput in both directions
- TCP parallel-stream throughput in both directions
- concurrent throughput while both links are active
- non-SSH file transfer using a data plane that is not crypto-limited by `scp`
- checksum verification after transfer
- fallback behavior when one path is degraded or unavailable

SSH is fine as a control plane for starting a remote receiver. It should not be used as the raw link-throughput measurement.

## Payload Classifier

Use the sideband only for payloads that can benefit from it:

- model sync
- artifact sync
- replay payloads
- large context staging
- OCR or document scratch payloads

Keep normal Kubernetes service traffic, ordinary chat, auth, billing, and web requests on the normal supported network path.

Recommended classifier:

- small payload: fastest healthy single path
- large payload: measured-weight parallel shards
- device busy: defer or fallback
- path degraded: fallback to the normal network with a logged reason

## User-Visible Truth

Do not show "dual sideband active" just because two cables are plugged in.

Show that state only when:

- both paths have current health
- both paths can move data
- the selected workload is sideband-eligible
- the last job has proof: size, transport, observed throughput, and checksum or equivalent integrity check

Good owner dashboard fields:

- active paths
- last job
- transfer size
- transport used
- observed throughput
- fallback reason

Bad claims:

- treating two links as one 60/80/120 Gb/s pipe without measured proof
- saying normal chat is accelerated by cables when the bottleneck is model compute or GPU memory
- persisting node-level routes without a reboot/drain rollback plan

## Sanitized Example Result

A representative run used two direct links between an Apple Silicon workstation and a Linux AI node. The links did not behave according to nominal labels: the second path was the faster measured data path.

Sanitized proof:

- path A iperf forward: about 2.1 Gb/s
- path B iperf forward: about 17.2 Gb/s
- concurrent iperf aggregate: about 19.3 Gb/s
- 512 MiB raw TCP single-path transfer on fastest path: about 11.2 Gb/s
- 512 MiB measured-weight striped transfer across both paths: about 12.6 Gb/s
- improvement over fastest single path: about 11%
- remote checksum matched

The correct product conclusion was not "we made one huge pipe." The correct conclusion was "large eligible payload staging can improve modestly with measured striping, while normal app traffic remains travel-safe and cable-independent."

## Operational Guardrail

Run this kind of test inside an explicit maintenance window when it might affect user-visible linked-device behavior. The start message should state the reason, affected surfaces, expected impact, owner/run id, and closeout condition. The clear message should include duration, proof, current version, health, and whether users were impacted.

Do not use maintenance mode for ordinary UI/copy runs where user-visible latency or outage is not expected.
