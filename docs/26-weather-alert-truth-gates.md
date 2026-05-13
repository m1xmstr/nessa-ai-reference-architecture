# Weather Alert Truth Gates

## Purpose

This document captures public-safe weather-alert notification patterns from Nessa AI. It does not publish private locations, home addresses, user records, exact alert history, source code, credentials, or private notification payloads.

The core lesson is that weather alerts must not sound official unless they are backed by an active official source. Forecast chatter and owner notifications are different products.

## Alert Language Contract

Do not use words such as "severe weather," "warning," "watch," or "alert" unless an active official alert exists or the message clearly says it is only a forecast risk.

Useful wording split:

| Condition | Allowed wording | Push behavior |
|---|---|---|
| Official active alert | "Weather alert" with source, event, severity, area, effective time, and expiration | Push allowed |
| Forecast risk only | "Weather heads-up" and "forecast risk" | Optional low-urgency digest |
| Ambiguous parser result | No urgent wording | Suppress and log |
| No alert | No alert wording | Suppress |

This prevents a casual forecast from becoming a false emergency.

## Confidence Levels

A useful weather gate uses explicit confidence levels:

- `official_active_alert`: an active alert from an official source matches the user's resolved location.
- `forecast_risk`: weather language indicates possible future risk, but there is no active official alert.
- `ambiguous`: the text looks concerning, but source, location, or active-alert proof is incomplete.
- `no_alert`: no actionable alert or forecast risk exists.

Only `official_active_alert` should page by default. `forecast_risk` can be digest-only if the user opted into low-urgency heads-up messages.

## Official Alert Push Requirements

When a real active alert is pushed, the copy should include:

- source
- alert id or event id
- event type
- severity
- affected county or area
- effective time
- expiration time
- user or home location matched

If any of those fields are missing, the system should either degrade the severity or suppress the push until the missing truth is resolved.

## Location Resolution

Location matching should be explicit and private.

Recommended public-safe pattern:

1. Resolve the user's selected home location, postal code, or configured weather location.
2. Query official active alerts for that resolved point or area.
3. Match the alert area to the user-facing location label.
4. Log only a redacted location hash or coarse region in general logs.
5. Keep precise location details out of public logs, screenshots, and reference docs.

Location mismatch should fail closed. A storm in a nearby region is useful forecast context, but it is not an official alert for the user's home unless the source says so.

## Preferences And Quiet Hours

Weather notifications should respect user preferences.

Recommended controls:

- enable or disable weather alerts
- choose whether forecast-risk heads-up messages are allowed
- configure quiet hours
- allow urgent official alerts to bypass quiet hours only when policy permits it

Suppressed events should still be recorded with a safe reason so the owner can debug the decision later.

## Why Did I Get This?

Owner/admin users need a safe debug path.

The debug view should show:

- confidence level
- official source
- event id
- matched location label
- preference decision
- quiet-hours decision
- final push decision

It should not expose another user's precise private location, raw notification destination, or secret channel identifiers.

## Replay Tests

Weather gates should include replay tests for false positives.

Useful fixtures:

- severe-sounding forecast text with no active official alert
- active official warning with complete source metadata
- active official watch with expiration
- expired alert
- mismatched location
- ambiguous parser output

The false-positive case should not send as a severe alert unless official active-alert proof exists.

## Public Boundary

This repo intentionally omits:

- private user locations and exact coordinates
- production notification text from real users
- source-code implementation details
- private scheduler names, routes, channel ids, and database rows
- screenshots from owner/admin weather pages

The reusable pattern is the discipline: official alerts require official proof, forecast risk uses softer language, ambiguous results fail closed, and every weather push can explain why it was sent.
