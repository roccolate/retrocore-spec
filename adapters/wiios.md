# WiiOS Adapter Notes

WiiOS consumes retrocore contracts as manifest/event/window vocabulary, not as a
runtime dependency.

## Adoption rule

WiiOS may adopt:

- contract field names,
- fixture ideas,
- logical event names,
- app/window/failure vocabulary.

WiiOS must not adopt:

- a shared runtime,
- a shared renderer,
- a platform abstraction layer,
- POSIX or terminal assumptions,
- keyboard/mouse assumptions as requirements.

The Wii implementation remains native to `devkitPPC`, `libogc`, Homebrew Channel,
Broadway/IOS, PAD/WPAD, VI/XFB, and SD/USB/FAT-style storage.

## App Manifest

WiiOS `sdroot_template/wiios/apps/*/manifest.ini` maps to
`contracts/app-manifest.md`.

Canonical fields:

```ini
[app]
id=hello
title=Hello App
kind=demo_app
entry=app.elf
icon=icon.bin
version=0.1.0
requires=input,storage,window
```

Legacy WiiOS v0.1 aliases:

- `name` -> `title`
- `type` -> `kind`

The WiiOS parser currently accepts both during migration.

## v0.2 App Registry Mapping

WiiOS `v0.2.0` should scan:

```text
<root>/apps/*/manifest.ini
```

The registry should keep Wii-native execution mapping. For example:

```text
id=hello -> hello_app_api()
```

until true external app loading exists.

Registry entries may map retrocore launch/failure vocabulary to WiiOS-local
status values:

| Retrocore failure | WiiOS use |
|---|---|
| `not_found` | Manifest/app id missing. |
| `invalid_manifest` | Manifest parse or validation failure. |
| `missing_capability` | Required runtime capability unavailable. |
| `launch_failed` | App is valid but cannot start. |
| `permission_denied` | Runtime policy blocks the app. |
| `unsupported_platform` | App kind/entry is valid but unsupported by WiiOS. |

## Logical Events

Raw PAD/WPAD input stays local. WiiOS may map high-level actions to logical event
names for docs, fixtures, or future tests:

| WiiOS input/action | Logical event meaning |
|---|---|
| `A` | activate / launch selected app |
| `B` | back / close focused panel |
| `LEFT` / `RIGHT` | focus previous / focus next |
| `HOME` / `START` | system exit |

Potential logical event names:

- `launch_app`
- `focus_next`
- `focus_prev`
- `close_window`
- `menu_action`

## Window Model

WiiOS may partially adopt `contracts/window-model.md`.

Allowed adaptations:

- panels/cards instead of overlapping desktop windows,
- launcher focus instead of pointer focus,
- no window drag requirement,
- no minimize/maximize requirement until useful on Wii.

Useful shared vocabulary:

- focused window/panel,
- z-order or task order,
- modal panel,
- close/back behavior,
- launch focuses/open app.

## Theme Tokens

Theme-token adoption is planned, not required yet. WiiOS may quantize colors for
its VI/XFB renderer and does not need to preserve exact RGB values from fixtures.

## Boundaries

Keep these Wii-native:

- libogc boot/input/render/storage glue
- Homebrew Channel package layout
- PAD/WPAD constants
- SD/USB/FAT path resolution
- `.dol` build and package flow
- VI/XFB rendering constraints
