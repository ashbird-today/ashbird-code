# Changelog

All notable changes to Ashbird Code are listed here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [0.1.56] - 2026-11-27

### Changed

- The README's privacy section now describes the onboarding counts and usage telemetry separately and accurately.

## [0.1.55] - 2026-11-27

### Fixed

- On a fresh VS Code start the sidebar showed an empty chat panel until the extension loaded. It now shows "Starting Ashbird…" until the chat is ready.

## [0.1.54] - 2026-11-26

### Changed

- The extension mentions the 7-day Reader trial where plans are offered.

## [0.1.53] - 2026-11-26

### Changed

- Using your own API key is cheaper, and the first 500k BYOK tokens each day are free.

## [0.1.52] - 2026-11-26

### Added

- First-run welcome: the chat opens and offers sign-in on first install.
- Anonymous onboarding counts (event name and version only). They follow VS Code's telemetry setting; set `ashbird.telemetry` to `false` to turn them off.

## [0.1.50] - 2026-11-26

### Changed

- Scope is now three fixed levels: Budget (32k), Smart (128k) and Long (256k). Lite is removed.

## [0.1.49] - 2026-11-26

### Changed

- The model picker shows Ashbird Pro as **Kimi K3.1 Pro**.
- Using your own API key without a subscription now shows a clear notice with a link to start a trial or pick a plan, instead of a raw error.

## [0.1.48] - 2026-11-25

### Fixed

- Commands such as `git log` no longer hang waiting at a pager prompt until the timeout.
- On Windows the agent is told the shell is PowerShell, so it stops using bash-only syntax like `||`.

## [0.1.47] - 2026-11-25

### Changed

- The model picker now shows which model each option runs on, for example **Ashbird Fast · GLM 5.3 Flash**, and the Ashbird Pro cost prompt names its model. The names come from the server, so they stay correct when a model changes.

## [0.1.46] - 2026-11-25

### Changed

- Models are now named **Ashbird Fast**, **Ashbird Pro** and **Ashbird Included** in the picker, status bar and prompts.
- The own-key (BYOK) provider for the Ashbird EU route is now called `ashbird`. Keys you saved before keep working.

## [0.1.45] - 2026-11-24

First public Marketplace release. Summary of 0.1.28 to 0.1.45:

### Added

- **Preview** button next to Send, and screenshots and renders shown to the model by default.
- **Scope picker** in the sidebar: Lite (cheapest), Budget, Smart and Long, set with `ashbird.scope`.
- **Page check**: the agent renders HTML and standalone SVG files and flags broken layouts, duplicate ids and drawings that disappeared since the last check.
- **Multiple own keys (BYOK)**, chosen per model role (head and worker).
- **Live files and working set**: the agent tracks the files it is working on and keeps their current contents in view, so it edits against fresh code instead of stale reads.
- Auto plan, brief and compression switches for long runs.
- Safe post-tool hooks.

### Changed

- Long conversations cost less: older context is stored server-side, encrypted with a key only you hold, and the model sees a compact working set.
- Clearer, plain-language messages in the sidebar.
- `ashbird.history` was renamed to `ashbird.scope`.

### Fixed

- Line edits: a valid line range now wins over a text search.
- Guards against writing over a file the agent has not read, and a timeout for page loads.
- Clearer errors when your own provider key is rejected.

## Earlier versions

Versions before 0.1.45 were a private preview.
