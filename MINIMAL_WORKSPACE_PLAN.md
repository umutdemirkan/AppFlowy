# Minimal AppFlowy Desktop Plan

This branch tracks the work to make this fork a small, local-first Windows workspace for personal use.

## Product goal

Keep the actual AppFlowy Flutter/Rust desktop application and its document/database foundations. Do not replace it with a browser-based imitation or a new application.

The app should feel like a reduced AppFlowy: quiet UI, short navigation, editable tables, and no features that are not needed for a single-person workflow.

## Required everyday areas

1. **Action Points** — a database/table with ID, topic, status, priority (0–5), ETA/follow-up, next action, and latest updates.
2. **Lists** — simple checklists.
3. **Tables** — editable grids with custom columns and rows.
4. **Notes** — ordinary editable documents.

Prefer AppFlowy's existing database and document features over creating duplicate implementations.

## Principles

- Windows desktop first.
- Local/offline use should be the default; no account or cloud setup should be required for the core workflow if the codebase permits it.
- Do not remove or migrate data destructively without a verified backup and a migration plan.
- Keep upstream AppFlowy history and licensing/attribution. The upstream project is licensed under AGPL-3.0.
- Make changes on this branch, not directly on `main`.
- Avoid broad dependency removal until the affected dependency graph is understood.

## Implementation phases

### Phase 0 — Establish a reproducible baseline

- Record the upstream commit this branch starts from.
- Follow the official source-build instructions for Windows.
- Build and launch the unmodified app before changing application code.
- Record build steps, startup behaviour, package size, and idle memory as a baseline.
- Confirm the local workspace/data location and test that a sample document and database survive app restart.

### Phase 1 — Remove unnecessary product surface

- Trace startup/onboarding, navigation, workspace home, settings, and feature registration.
- Remove AI entry points and AI-related UI first, if not already completely disabled.
- Hide collaboration, sharing, team, and cloud-only flows that are not needed for a local single-user app.
- Preserve local documents, database tables/views, search, and reliable local persistence.
- Prefer feature flags and navigation changes over deleting deeply coupled modules at first.

### Phase 2 — Configure a minimal default workspace

- Start in a single personal workspace.
- Keep only the four core areas: Action Points, Lists, Tables, Notes.
- Provide an Action Points table with the fields listed above.
- Use native AppFlowy documents and database views wherever possible.
- Keep navigation and settings minimal without breaking file import/export or recovery.

### Phase 3 — Verify and slim safely

- Build and test a Windows release.
- Test create/edit/rename/delete, restart persistence, table editing, search, backup/export, and offline operation.
- Compare package size, startup time, and idle memory against Phase 0.
- Remove code/dependencies only when tests and dependency tracing show they are no longer needed.

## Current status

- This repository is a fork of `AppFlowy-IO/AppFlowy`; upstream `main` is not modified by this work.
- Added a compile-time `MINIMAL_WORKSPACE` switch in `frontend/appflowy_flutter/lib/shared/feature_flags.dart`. In a minimal build it hides selected collaboration, member settings, document/database sync, plan/billing, space design, inline sub-page mention, and shared-section surfaces.
- Hid the command-palette AI search entry, AI writing commands in the slash menu, AI writing actions in the editor selection toolbar, and the floating support bubble.
- Hid AI settings and cloud settings entries from the settings menu in a minimal build.
- These are UI/feature-flag changes only. AI/cloud/authentication modules remain in the source tree. This does not yet prove the app can run fully offline or that all outbound network activity is disabled.
- No Windows build, static analysis, or runtime tests have been run from this environment. All changes require compilation and testing before they should be considered stable.

## Minimal build switch

From `frontend/appflowy_flutter`, after installing the repository's documented Flutter/Rust prerequisites, the intended Windows build command is:

```powershell
flutter build windows --dart-define=MINIMAL_WORKSPACE=true
```

This switch is a starting point for reducing the product surface, not a claim that all AI, account, or network code has been removed. Confirm the official source-build prerequisites in the AppFlowy contributor documentation before building.

## Minimal build switch

From `frontend/appflowy_flutter`, once the repository's documented Flutter/Rust prerequisites are installed, the intended Windows build command is:

```powershell
flutter build windows --dart-define=MINIMAL_WORKSPACE=true
```

This flag is a starting point for product-surface reduction, not a claim that all AI, account, or network code has been removed. Confirm the official source-build prerequisites in the AppFlowy contributor documentation before building.
