# Example: Offline Notes app

Use this as a small, testable project brief rather than as a claim that a complete production app already exists.

## Request

Build a notes app that lets a user create, edit, search, export, and delete notes while the device is in airplane mode. No account, analytics, remote images, or cloud sync is required for version one.

## Constraint brief

| Area | Decision |
| --- | --- |
| Primary job | Capture and retrieve short personal notes |
| Offline boundary | All note actions, search, and export work without connectivity |
| Local data | Note ID, title, body, created time, updated time, and tags |
| Privacy | No outbound requests; no account required |
| Distribution | Debug APK first, then a clearly labeled signed release if keys are available |
| Cost | Use the project’s existing Android dependencies before adding anything |

## Suggested flow

1. Show a local notes list on launch.
2. Open a note editor with draft preservation.
3. Save through a local repository interface.
4. Search locally and show an honest empty state.
5. Export notes to a user-selected file.
6. Offer import with validation and a duplicate-handling rule.
7. Provide “Delete all local data” behind a confirmation step.

## Acceptance tests

| Test | Pass condition |
| --- | --- |
| Airplane-mode launch | The list opens without a network error |
| Create and restart | A saved note is still present after process restart |
| Draft interruption | A draft is restored or the limitation is documented |
| Search | Matching title/body text is found locally |
| Export/import | A test note round-trips without changed content |
| No permission | The core note workflow still works |
| Accessibility | Controls have readable labels and no clipped content at large text |

## Delivery note

Report the exact build command, variant, application ID, test results, export format, and any missing SDK or uncached dependency. Call the app **offline-first** only after the critical tests pass.
