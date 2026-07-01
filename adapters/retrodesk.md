# RetroDesk Adapter Notes

RetroDesk consumes retrocore contracts as event fixtures and vocabulary, not as a runtime dependency.

## Event Fixtures

RetroDesk maps `fixtures/events/open-files-and-focus.json` like this:

| retrocore event/app | RetroDesk action |
|---|---|
| `launch_app` | `app_launch()` |
| app `files` | local app id `filemanager` |
| expect `focused: true` | `desktop_active_window() == desktop_app_window_id(... )` |

Because File Manager is launched by default in RetroDesk, launching logical app `files` focuses the existing `filemanager` instance instead of duplicating it.

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

## Boundaries

Keep these RetroDesk-native:

- C app lifecycle
- curses/ANSI render backends
- TTY/mouse decoding
- window manager implementation
- terminal PTY behavior
