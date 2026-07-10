# RetroDesk Adapter Notes

RetroDesk consumes retrocore contracts as event fixtures and vocabulary, not as a
runtime dependency.

RetroDesk remains a native portable C terminal desktop. `retrocore-spec` is not a
library, framework, submodule, or runtime dependency for RetroDesk.

## Event Fixtures

RetroDesk currently consumes event fixtures from:

```text
fixtures/events/
```

### `open-files-and-focus.json`

RetroDesk maps `fixtures/events/open-files-and-focus.json` like this:

| retrocore event/app | RetroDesk action |
|---|---|
| `launch_app` | `app_launch()` |
| app `files` | local app id `filemanager` |
| expect `focused: true` | `desktop_active_window() == desktop_app_window_id(... )` |

Because File Manager is launched by default in RetroDesk, launching logical app
`files` focuses the existing `filemanager` instance instead of duplicating it.

### `window-drag-basic.json`

RetroDesk maps `fixtures/events/window-drag-basic.json` like this:

| retrocore event/app | RetroDesk action |
|---|---|
| `launch_app` | launch/focus local `filemanager` |
| `pointer_down` | translated to a RetroDesk mouse event on the File Manager title bar |
| `pointer_move` | translated to a RetroDesk mouse move event preserving fixture drag delta |
| `pointer_up` | translated to a RetroDesk mouse release event |
| expect `focused: true` | File Manager must still exist and remain focused |

The fixture uses logical coordinates. RetroDesk preserves the fixture drag delta,
but translates the start point to the local File Manager title bar so the same
contract can exercise RetroDesk-native WM drag behavior without requiring all
projects to share identical initial window geometry.

RetroDesk assertions for this fixture are split intentionally:

- desktop-level replay verifies File Manager exists, remains focused, and is not
  duplicated;
- WM-level replay uses `retro_window_get_geometry()` to assert final `x`/`y`
  coordinates after applying the fixture drag delta.

If a future fixture requires app-window geometry after a full `Desktop` replay,
RetroDesk should add a small public desktop/window geometry helper instead of
reaching into private `Desktop` internals.

## Supported Event Types

Currently consumed by RetroDesk's tiny fixture runner:

- `launch_app`
- `pointer_down`
- `pointer_move`
- `pointer_up`

Planned / not yet consumed:

- `focus_next`
- `close_window`
- `key`
- `menu_action`

Unsupported event types must not be ignored silently by RetroDesk tests.

## Test Integration

RetroDesk has an optional CMake test:

```text
tests/retrocore_event_fixture_test.c
```

It is enabled when `RETROCORE_SPEC_DIR` points at a checkout containing:

```text
fixtures/events/open-files-and-focus.json
```

Default expected sibling layout:

```text
~/retrodesk
~/retrocore-spec
```

Example configuration:

```bash
cmake -S . -B build -DRETROCORE_SPEC_DIR=../retrocore-spec
cmake --build build
ctest --test-dir build --output-on-failure
```

## Boundaries

Keep these RetroDesk-native:

- C app lifecycle
- curses/ANSI render backends
- TTY/mouse decoding
- window manager implementation
- terminal PTY behavior
- platform capability detection

Do not move runtime code from `retrocore-spec` into RetroDesk, and do not make
RetroDesk link against `retrocore-spec` as a library.
