# Window Model Contract

Status: draft

Defines shared logical behavior for desktop/window systems.

## Goals

- Same language for focus, z-order, close, move, modal, and minimize/maximize.
- Test behavior without requiring identical rendering.

## Concepts

- **Window**: rectangular interactive surface owned by an app.
- **Focused window**: receives keyboard input.
- **Z-order**: front-to-back stacking order.
- **Modal window**: blocks focus changes outside itself until closed or resolved.
- **Minimized window**: not visible in desktop area but still present in task/app list.
- **Maximized window**: occupies available desktop area.

## Required Behaviors

1. Launching an app that opens a window focuses that window.
2. Clicking/pointer-selecting a window focuses it and brings it forward unless modal rules block it.
3. `focus_next` cycles through focusable windows in z-order or project-defined task order.
4. Closing the focused window focuses the next eligible window.
5. Dragging a title bar moves the window if the project supports moving windows.

## Partial Implementation Is Allowed

- WiiOS may implement panels/cards instead of overlapping windows.
- ArmoniOS may enforce kernel-owned compositor rules.
- RetroTUI and RetroDesk should implement most of this directly.

## Suggested Test Outcomes

- `window_exists(app)`
- `window_focused(app)`
- `window_frontmost(app)`
- `window_count(n)`
- `modal_active(true|false)`
