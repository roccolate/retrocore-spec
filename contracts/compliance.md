# Contract Adoption Matrix

Status: living document
Last reviewed: 2026-07-09

This matrix tracks how each retro systems project currently relates to the shared contracts and fixtures.

Legend:

- `active` — the project currently uses the contract or fixture shape.
- `partial` — the project maps only part of the contract, or adapts it to a different UI/runtime model.
- `planned` — documented target mapping, not yet proven by implementation.
- `reference` — useful as a prototype/reference, but not a requirement for other projects.
- `n/a` — not applicable yet.

## Project Matrix

| Project | App manifest | Logical events | Window model | Theme tokens | Adapter notes |
|---|---|---|---|---|---|
| RetroTUI | planned | planned | planned/reference | planned | `adapters/retrotui.md` |
| RetroDesk | partial | active | partial/active | planned | `adapters/retrodesk.md` |
| WiiOS | active/partial | planned/partial | adapted/partial | planned | `adapters/wiios.md` |
| ArmoniOS | planned | planned | planned | planned | `adapters/armonios.md` |

## Current Fixture Adoption

| Fixture | RetroTUI | RetroDesk | WiiOS | ArmoniOS | Notes |
|---|---|---|---|---|---|
| `fixtures/events/open-files-and-focus.json` | planned | active | planned/adapted | planned | RetroDesk maps logical app `files` to local app id `filemanager`; WiiOS may map it to `FILES`/File Manager launcher behavior. |
| `fixtures/events/window-drag-basic.json` | planned | planned | n/a/adapted | planned | WiiOS may adapt this as a panel/card focus test instead of overlapping-window drag. |
| `fixtures/themes/win31.json` | planned | planned | planned | planned | Renderer-specific color quantization is allowed. |
| `fixtures/themes/hacker.json` | planned | planned | planned | planned | Renderer-specific color quantization is allowed. |

## Contract Notes

### App manifest

WiiOS has the most concrete mapping today through `sdroot_template/wiios/apps/*/manifest.ini`. The canonical retrocore fields are `id`, `title`, `kind`, `entry`, `icon`, `version`, and `requires`. WiiOS currently accepts legacy aliases during migration: `name` -> `title`, and `type` -> `kind`.

WiiOS `v0.2.0` is expected to use this contract as the basis for its dynamic app registry, while keeping launch/execution mapping Wii-local. For example, a manifest may identify `id=hello`, but WiiOS may still map that to a compiled-in `hello_app_api()` until true external app loading exists.

RetroDesk can map its C app registry entries to this contract, but the current active integration is focused on event fixtures.

RetroTUI and ArmoniOS have documented target mappings, but should not be treated as fully implemented until their project repositories add tests or adapter references.

### Logical events

RetroDesk currently has the strongest event-fixture integration through `fixtures/events/open-files-and-focus.json`.

WiiOS should treat logical events as test/documentation vocabulary, not as replacement for raw Wii input. PAD/WPAD constants remain local to WiiOS. Candidate mappings include `A` -> activate/launch, `B` -> back/close, and `LEFT`/`RIGHT` -> focus previous/next.

All projects should keep raw platform input local:

- curses/xterm/terminal details stay in RetroTUI or RetroDesk.
- PAD/WPAD constants stay in WiiOS.
- HID, virtio-input, interrupt, and driver details stay in ArmoniOS.

### Window model

RetroTUI and RetroDesk can implement most desktop/window behavior directly.

WiiOS may adapt the model to panels, cards, launch screens, or launcher focus rather than overlapping desktop windows. `focus_next`, `close_window`, and `launch_app` are useful logical names, but dragging/minimize/maximize behavior should not be required until it makes sense on Wii.

ArmoniOS may enforce window behavior through a kernel-owned or compositor-owned model.

### Theme tokens

Theme tokens are semantic names, not rendering requirements. Each project may quantize or reinterpret the actual color values based on terminal colors, Wii rendering constraints, framebuffer palettes, or GUI style tables.

For WiiOS, theme-token adoption remains planned and should follow the app registry and desktop/panel behavior, not precede them.

## Maintenance Checklist

When documentation or implementation changes:

1. Update the relevant contract in `contracts/`.
2. Update the project mapping in `adapters/`.
3. Update this matrix.
4. Add or update a fixture only when the behavior is useful across multiple projects.
5. Keep implementation-specific details in the project repository, not in `retrocore-spec`.
