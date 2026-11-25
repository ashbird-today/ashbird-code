# Changelog

All notable changes to Ashbird Code are listed here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

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
