# Worksheet-First Tutor Session Layout

Last updated: 2026-05-28

This document captures a public-safe UX pattern for private AI tutor sessions that use worksheet or homework images.

It intentionally omits private product source, session IDs, route names, screenshots, account data, OCR internals, prompts, worksheet parser details, tutor-state schemas, endpoint addresses, deployment manifests, tokens, and production proof artifacts.

## Problem

Worksheet tutoring breaks down when the learner cannot see the worksheet.

Common failure modes:

- placing maps, recaps, study plans, or command panels above the worksheet
- locking the page height so the worksheet is off-screen and cannot be reached
- making fit-to-page controls look active while the document still overflows the viewer
- repeating hint, check, and step buttons for every problem card
- putting too many tutor action buttons near the composer
- using internal labels such as "mission control" when the learner needs plain next-step guidance

The learner should never have to hunt for the worksheet before asking for help.

## Layout Rule

The worksheet belongs in the primary visual pane.

On desktop:

- left side: active worksheet, compact worksheet heading, page controls, file controls
- right side: tutor chat, active problem picker, small help controls
- first viewport: worksheet visible without scrolling
- page body: scrollable if content exceeds the viewport

On mobile:

- preserve clear context/tutor switching
- keep the worksheet reachable without hidden overflow traps
- keep the composer usable without burying it below long command lists

## Problem Picker Rule

The problem picker is a tutor-side navigation aid, not the main event.

Public-safe rules:

- label it with plain copy such as "Worksheet problems" and "Pick one problem to work on"
- show real problem cards only when extracted problem state exists
- keep each problem card focused on selection, not on a stack of repeated action buttons
- place secondary tools such as parent recap behind a small tools affordance
- use an honest empty state when extraction is weak

The worksheet remains the source of truth. The picker helps the tutor stay focused.

## Action Clutter Rule

Tutor sessions need fewer visible buttons.

A practical visible set:

- Hint
- Check work
- More

The More menu can hold secondary actions such as explain mistake, similar example, and explain steps. Avoid exposing a "show answer" control as a first-class action in a learning session; it undermines teach-first behavior and adds clutter.

## Fit-To-Page Rule

Fit-to-page must be tested visually, not only by active-button state.

The proof should verify:

- the fit-page button is active by default for dedicated worksheet sessions
- the rendered image or page fits within the visible stage width
- the rendered image or page fits within the visible stage height
- switching away from fit-page and back to fit-page recomputes the layout
- zoom controls do not push the worksheet out of reach without a way back

If a page is too large to fit perfectly because of source aspect ratio, the UI should make the limitation obvious and offer a reliable reset.

## Browser Proof

Closeout proof should include the exact learner-facing path in a real browser.

Useful measurements:

- worksheet heading visible
- worksheet stage top within the first viewport
- worksheet image or document bounds inside the viewer
- problem picker located in the tutor pane
- visible direct tutor actions reduced to the intended set
- no stale internal labels or study-plan dumps in the main worksheet pane
- body overflow behavior does not trap content

Backend API proof is not enough for this class of repair. The failure is visual and workflow-based.

## Product Lesson

For AI tutoring, "smart" starts with orientation. The learner must see the worksheet, know which problem is active, and have one obvious way to ask for help. Extra commands, internal labels, and hidden overflow make the tutor feel less capable even when the model answer is improving.
