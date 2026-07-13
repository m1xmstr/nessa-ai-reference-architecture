# Location-aware tool latency and mobile action truth

Location-aware answers combine browser permission, live-data providers, response rendering, persistence, and mobile layout. A product can receive valid coordinates and still tell the user that location failed if its live lookup spends the full deadline on one source or discards useful unpriced nearby results. It can also show technically correct server timing while hiding the wait a person actually experienced.

This note captures public-safe patterns for making those flows reliable and honest.

## Treat location permission outcomes differently

Browser geolocation errors are not interchangeable:

- a permission denial is an explicit user choice and may be remembered for the session;
- a timeout can be caused by weak GPS, browser scheduling, or a slow device and should be retryable;
- a temporarily unavailable position should also remain retryable;
- a granted position should travel with the request and be reflected in the answer as a GPS-based result.

Caching every geolocation error as a denial turns one transient timeout into a broken session. Use a bounded but realistic timeout, cache only the browser's explicit denial code, and let later attempts recover without requiring the user to clear site data.

## Degrade to useful nearby truth

Live price and place providers vary in coverage and speed. A bounded lookup should start independent sources concurrently and preserve the best useful result available before the deadline.

For nearby fuel, a sound fallback order is:

1. current priced stations with distance;
2. nearby stations with distance and an honest `price unavailable` state;
3. a clearly labeled external tracker only when no local station result survives.

The second state matters. Valid phone coordinates plus nearby station cards are evidence that location worked, even when a price feed is temporarily incomplete. Do not collapse that into a generic location-failure message.

Place discovery should also avoid expensive enrichment by default. A normal `near me` request needs useful nearby options quickly. Additional review or rating lookups belong to explicit quality requests such as `best` or `top rated`, where the extra latency serves the user's stated intent.

## Report the wait the user experienced

One duration cannot explain a modern AI response. Keep these measures separate:

- **wait**: client wall-clock time from queued send through completed response;
- **first visible**: client wall-clock time until useful response content first appears;
- **server**: server-side request duration;
- **live lookup**: deterministic tool or provider work;
- **generation**: model inference time, shown only when model generation actually occurred.

Do not label deterministic place or price work as generation. Do not present database message timestamps as latency. If a streaming transport recovers through a retry, record that recovery so diagnostics explain the full user-visible wait.

## Make retries idempotent

A streaming client may retry through a blocking endpoint after a network or first-token failure. The same client request identifier must resolve to the already completed response instead of running the expensive route again.

The replay contract should:

1. compute a stable request fingerprint before creating a new thread or side effect;
2. check the replay cache at stream ingress;
3. cache the normalized final response before sending the terminal stream event;
4. let a fallback transport return that cached answer;
5. reject identifier reuse when the fingerprint does not match.

This protects both latency and write-path integrity.

## Keep post-response work off the visible path

Indexing a completed exchange for later retrieval is valuable, but it should not hold the composer in a working state after the final answer is already visible. Queue bounded background indexing only after message persistence succeeds, expose queue pressure in logs, and fail softly if indexing cannot be scheduled.

Ephemeral live-tool results deserve special treatment. Prices and nearby availability become stale quickly, so avoid treating them as durable personal-memory context unless the product has an explicit freshness model.

## Use a mobile action grid

Four equal response actions rarely fit safely in one phone row. Flex shrink can split words in the middle even when the controls remain technically inside the viewport.

At narrow widths, a two-column grid gives each action a stable tap target. Preserve normal word breaking, disable hyphenation, allow multi-word labels to wrap only between words, and keep a minimum touch height of 44 CSS pixels. Verify both a common phone viewport and a narrower fallback viewport with the actual labels, not placeholder buttons.

## Proof discipline

The acceptance path should include:

1. a signed-in mobile browser with granted geolocation;
2. a real location-aware price request and a real nearby-place request;
3. a creative response to compare tool and generation timing labels;
4. browser-measured first-visible and completion times;
5. action geometry and screenshots at two narrow widths;
6. an idempotent stream-retry regression test;
7. staging quality and release-truth gates run without competing manual load;
8. exact-digest production promotion followed by the same targeted browser proof.

Manual performance probes and automated latency gates should not share the same constrained staging account or runtime at the same time. If they overlap, record the contamination, rerun the unchanged candidate in isolation, and never lower a threshold to make the noisy run pass.

The broader lesson is that location truth, latency truth, transport truth, and mobile polish are one product contract. A response is not successful merely because some backend eventually produced text.
