---
name: offline-android-studio
description: Build, repair, and package privacy-first Android applications that remain useful without an internet connection. Use for offline-first Android apps, free or open-source Android tooling, local data storage, dependency-minimal builds, on-device processing, and APK release preparation.
---

# Offline Android Studio

Build Android apps for low-connectivity environments with **local-first behavior**, predictable builds, and no unnecessary paid services. Treat network access as optional, not as a hidden dependency.

## Start with a constraint brief

Translate the request into a short constraint brief before writing code:

| Area | Decide explicitly |
| --- | --- |
| Primary job | The one user problem the app must solve |
| Offline boundary | Which screens and actions must work with airplane mode enabled |
| Data | What is stored locally, for how long, and in which format |
| Privacy | Whether data may leave the device; default to no outbound data |
| Device target | Android version range, screen sizes, storage, and available memory |
| Distribution | Debug APK, signed APK, AAB, or source project |
| Cost | Use free/open-source components and avoid paid hosted services |

If requirements are incomplete, choose conservative defaults: local-only storage, a small dependency surface, accessible UI, and a reversible data export.

## Choose the smallest viable architecture

Prefer native Android with Kotlin and Jetpack Compose when a local Android project is available. Use classic Views only when the existing project already depends on them or when a stable legacy widget is essential. Choose a WebView or hybrid shell only when the user explicitly needs a web UI and the complete asset bundle can be shipped locally.

Keep the app in a single process unless a background worker is genuinely required. Separate presentation, domain, and storage layers so offline behavior can be tested without a device network. Define a repository interface such as `TaskRepository` and provide a local implementation first; a future sync implementation must be an optional adapter, never a prerequisite.

## Implement offline-first behavior

1. Define the app state that must be available at launch.
2. Seed only safe demo or onboarding data; never fabricate user data.
3. Store user data locally using the project’s existing persistence layer. If none exists, select the simplest durable option that supports the data shape and migration needs.
4. Make every core action succeed without network permission or connectivity.
5. Represent unavailable remote features honestly with a disabled state and a short explanation.
6. Add explicit export and import paths for user-owned data where practical.
7. Avoid analytics, remote fonts, remote images, remote configuration, and runtime CDN dependencies unless the user explicitly opts in.
8. Use stable local assets and content descriptions so the UI remains complete offline.

## Protect data by default

Collect the minimum data needed for the feature. Do not add account creation, telemetry, advertising identifiers, or background upload merely because a library makes them convenient. Keep secrets out of source control and do not store sensitive values in plain text when the platform offers a safer local mechanism. Add a visible “Delete local data” action when the app stores personal content.

When handling health, finance, identity, or location data, state the limits of the prototype and avoid claiming security or compliance that has not been verified. For a sensitive app, include a threat-model note covering device loss, screenshots, backups, logs, and exported files.

## Design the user experience for interruption

Use a clear first-run path, large touch targets, readable contrast, and predictable navigation. Preserve draft state after process death where feasible. Every save, delete, import, and export action should provide immediate local feedback. Avoid blocking spinners for work that can happen synchronously or in a short local transaction. If a long local task is necessary, show progress and allow cancellation.

## Build without the internet

Before building, inspect the project’s declared Gradle and plugin versions and check whether the required artifacts are already cached. Do not silently change versions or introduce new dependencies when offline access is unavailable. If a dependency is missing, first replace it with platform or already-present functionality; only then report the exact missing artifact and the environment needed to obtain it later.

Use the project’s existing wrapper and scripts when present. Keep generated outputs out of source control unless the user explicitly wants the APK committed. A reproducible build note should record the build variant, application ID, version name, version code, signing mode, and assumptions about the local SDK.

## Test the real offline boundary

Run a test matrix rather than checking only that the app launches:

| Scenario | Expected result |
| --- | --- |
| Fresh install in airplane mode | App opens to a usable local state |
| Create, edit, and delete core data | Changes persist after restart |
| Process killed during a draft | No avoidable data loss |
| Missing optional remote feature | Honest empty or unavailable state |
| Export then import | User data round-trips without corruption |
| Small display and large text | No clipped or inaccessible controls |
| Rotation or recreation | State remains coherent, or rotation is intentionally constrained |
| No permission granted | App still explains and works within its core scope |

Use unit tests for state transitions and serialization, UI tests for the critical flow, and a manual airplane-mode check when a device or emulator is available. Do not describe an app as “fully offline” until the critical paths have passed this matrix.

## Package and report the result

Separate development build output from release packaging. For a release artifact, verify the package name, version, signing configuration, and that debug-only logging or sample data is absent. Never ask the user to share a signing key. If signing cannot be completed, deliver an unsigned or debug artifact only when clearly labeled.

Report what works without internet, what is intentionally unavailable offline, where local data is stored, how it can be exported, the build and test commands actually run, any uncached dependency or missing SDK component, and the exact artifact path with its signing status.

## Quality gate

Before delivery, confirm that the app has a clear purpose, no hidden network dependency in its core path, no invented completion claims, no committed secrets, an accessible empty state, a tested persistence path, and a concise offline limitation note. Prefer a smaller working app over a larger app that fails when connectivity disappears.

## Useful references

- Android Developers, “Build an offline-first app”: https://developer.android.com/topic/architecture/data-layer/offline-first
- Android Developers, “App architecture”: https://developer.android.com/topic/architecture
- Android Developers, “Data and file storage overview”: https://developer.android.com/training/data-storage
- Android Developers, “Accessibility”: https://developer.android.com/guide/topics/ui/accessibility

## Practical example

Apply this skill to the included **Offline Notes app** example in `examples/offline-notes-app.md`. Start a new task by copying `templates/offline-app-brief.md` and use `assets/demo.gif` as a quick visual summary of the workflow.
