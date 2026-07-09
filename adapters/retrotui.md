# RetroTUI Adapter Notes

Status: planned/reference

RetroTUI should consume retrocore contracts as vocabulary and fixtures, not as a runtime dependency.

RetroTUI is the Python/curses reference desktop in the retro systems family. It may be the fastest place to prototype shared behavior, but its Python/curses implementation details must not become requirements for RetroDesk, WiiOS, or ArmoniOS.

## App Manifest

RetroTUI plugins or built-in apps may map to `contracts/app-manifest.md` like this:

| retrocore field | RetroTUI source |
|---|---|
| `id` | plugin id or built-in app id |
| `title` | plugin/app display name |
| `kind` | `system_app`, `bundled_app`, or `demo_app` |
| `entry` | Python module, callable registry id, or built-in app key |
| `icon` | symbolic icon name or local glyph id |
| `requires` | logical capabilities such as `input`, `storage`, `window`, `shell` |

RetroTUI may keep extra Python-specific metadata locally. Do not add Python-only fields to the shared contract unless another project proves the same shape is useful.

## Logical Events

RetroTUI should map curses/xterm/host input into `contracts/events.md` logical events.

Suggested mappings:

| Raw source | Logical event |
|---|---|
| keyboard key | `key` |
| printable character | `key` with printable `key` value |
| mouse press | `pointer_down` |
| mouse movement/drag | `pointer_move` |
| mouse release | `pointer_up` |
| app launcher command | `launch_app` |
| focus hotkey | `focus_next` |

The first fixture target should be:

```text
fixtures/events/open-files-and-focus.json
```

The second fixture target should be:

```text
fixtures/events/window-drag-basic.json
```

## Window Model

RetroTUI may implement most of `contracts/window-model.md` directly because it is a desktop-like environment.

Expected native concepts:

- focused window
- z-order
- close focused window
- focus cycling
- movable windows when mouse support is enabled
- modal windows if implemented locally

## Theme Tokens

RetroTUI may map `contracts/theme-tokens.md` into curses color pairs, xterm colors, or a reduced host terminal palette.

Theme-token names should stay stable even if the terminal backend quantizes the actual color values.

## Boundaries

Keep these RetroTUI-native:

- Python plugin lifecycle
- curses initialization and teardown
- host terminal quirks
- host filesystem paths
- mouse backend support
- rendering/layout implementation
- any Python-only extension fields