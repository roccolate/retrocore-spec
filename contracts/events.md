# Logical Events Contract

Status: draft

Defines platform-independent user/system events for replay tests and shared behavior descriptions.

## Goals

- Same vocabulary for keyboard, pointer, launch, focus, and window actions.
- Fixtures that can be consumed by tests in different projects.
- Keep raw platform events local.

## Event Envelope

```json
{"type":"key","key":"TAB"}
```

Every event has a required `type`. Extra fields depend on type.

## Core Event Types

### `key`

```json
{"type":"key","key":"ENTER","mods":["CTRL"]}
```

- `key`: symbolic key name or printable character.
- `mods`: optional list: `CTRL`, `ALT`, `SHIFT`, `META`.

### `pointer_down`, `pointer_move`, `pointer_up`

```json
{"type":"pointer_down","x":10,"y":2,"button":"primary"}
```

- `x`, `y`: logical screen coordinates.
- `button`: `primary`, `secondary`, `middle`, or project-defined.

### `launch_app`

```json
{"type":"launch_app","app":"files"}
```

### `close_window`

```json
{"type":"close_window","window":"focused"}
```

### `focus_next`

```json
{"type":"focus_next"}
```

### `menu_action`

```json
{"type":"menu_action","action":"file.open"}
```

## Expected Outcomes

Fixtures may define expected logical outcomes, not pixels.

Example:

```json
{
  "windows": [
    {"app":"files","focused":true}
  ]
}
```

## Platform Mapping

- RetroTUI: curses/xterm/gpm/Windows events -> logical events.
- RetroDesk: curses/ANSI/TTY events -> logical events.
- WiiOS: PAD/WPAD buttons -> logical events or launcher actions.
- ArmoniOS: HID/virtio-input -> GUI events -> logical events for host tests.
