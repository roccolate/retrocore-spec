# retrocore-spec

Shared contracts and fixtures for the retro systems family:

- RetroTUI — Python/curses desktop reference
- RetroDesk — portable C terminal desktop
- WiiOS — Nintendo Wii homebrew shell/launcher
- ArmoniOS — bare-metal AArch64 desktop OS

This repository intentionally starts with **contracts before code**.

The goal is to avoid duplicated decisions across projects without forcing all systems into one shared runtime.

## What This Repository Is

`retrocore-spec` is a specification and test-fixture repository. It provides shared vocabulary, human-readable contracts, and small fixtures that individual projects may consume in their own native way.

It is **not**:

- a shared runtime
- a framework
- a common renderer
- a kernel abstraction layer
- a portability layer that hides each platform

Each project keeps its own implementation, build system, platform glue, and runtime model.

## Rules

1. Share contracts, fixtures, and vocabulary first.
2. Do not share runtime code until at least three projects prove the same shape is needed.
3. Keep platform glue local.
4. A contract may be partially implemented by a project.
5. If a contract makes a project less honest to its platform, the project wins.

## Structure

```text
contracts/  human-readable specs and adoption status
fixtures/   sample manifests/events/themes used by tests
adapters/   project-specific mapping notes; no framework yet
decisions/  architecture decision records
```

## Current Contracts

| Contract | Status | Purpose |
|---|---|---|
| `contracts/app-manifest.md` | draft | App identity, metadata, launch target vocabulary, capabilities, and launch failure semantics. |
| `contracts/events.md` | draft | Platform-independent logical user/system events for replay tests and behavior descriptions. |
| `contracts/window-model.md` | draft | Shared vocabulary for focus, z-order, modal behavior, close behavior, and logical window outcomes. |
| `contracts/theme-tokens.md` | draft | Semantic UI color/style tokens that each renderer may quantize or map locally. |
| `contracts/app-lifecycle.md` | placeholder | Common lifecycle vocabulary only; each project owns its actual process/thread/plugin model. |
| `contracts/filesystem-layout.md` | placeholder | Shared names for user-facing storage roots without assuming POSIX. |
| `contracts/input-model.md` | placeholder | Notes for mapping raw platform input into logical events. |
| `contracts/compliance.md` | living document | Per-project adoption matrix for the current contracts and fixtures. |

## Current Fixtures

| Fixture area | Current examples | Purpose |
|---|---|---|
| `fixtures/events/` | `open-files-and-focus.json`, `window-drag-basic.json` | Replayable logical event sequences with expected outcomes. |
| `fixtures/themes/` | `win31.json`, `hacker.json` | Sample semantic theme-token palettes. |
| `fixtures/manifests/` | valid and intentionally broken app manifests | Parser and validation examples for `contracts/app-manifest.md`. |

## Current Adapter Notes

| Project | Adapter note | Current role |
|---|---|---|
| RetroTUI | `adapters/retrotui.md` | Planned/reference mapping for Python/curses plugins, events, windows, and themes. |
| RetroDesk | `adapters/retrodesk.md` | Active mapping for event fixtures and RetroDesk-native app/window behavior. |
| WiiOS | `adapters/wiios.md` | Active mapping for WiiOS `manifest.ini` fields and legacy aliases. |
| ArmoniOS | `adapters/armonios.md` | Planned mapping for bootfs/app metadata, GUI events, and bare-metal boundaries. |

## Adoption Order

1. App manifest
2. Logical events
3. Window model
4. Theme tokens
5. Optional tiny tools/adapters later

## Maintenance Rule

When a project starts using a contract or fixture, update the matching file in `adapters/` and the matrix in `contracts/compliance.md` in the same change. If implementation behavior differs from the contract, document the project-specific exception instead of forcing the project to pretend it is compatible.

Older working notes may still exist outside this repository, such as `~/RETRO_SYSTEMS_MAP.md`, but this repository is the source of truth for shared contracts, fixtures, adapter notes, and decisions.