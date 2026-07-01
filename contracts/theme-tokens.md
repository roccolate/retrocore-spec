# Theme Tokens Contract

Status: draft

Defines shared semantic color/style names. Projects render them however their platform allows.

## Goals

- Keep theme names stable across projects.
- Avoid copying renderer-specific color code.
- Support low-color terminals, Wii rendering, and bare-metal GUI palettes.

## Core Tokens

```text
desktop.bg
window.bg
window.fg
window.border
window.title.bg
window.title.fg
button.bg
button.fg
selection.bg
selection.fg
status.bg
status.fg
warning.fg
error.fg
```

## Optional Tokens

```text
menu.bg
menu.fg
menu.hotkey.fg
panel.bg
panel.fg
icon.fg
shadow.bg
disabled.fg
```

## JSON Fixture Shape

```json
{
  "name": "win31",
  "tokens": {
    "desktop.bg": "#008080",
    "window.bg": "#c0c0c0",
    "window.fg": "#000000"
  }
}
```

Projects may quantize colors to their palette.
