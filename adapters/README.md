# Adapters

This folder is intentionally light.

Adapters map each project to the shared contracts and fixtures. They are documentation first: they explain how a project consumes `retrocore-spec` vocabulary without turning this repository into a shared runtime or framework.

## Current Adapter Notes

| File | Project | Status | Purpose |
|---|---|---|---|
| `retrotui.md` | RetroTUI | planned/reference | Maps Python/curses plugins, events, windows, and theme tokens to the shared contracts. |
| `retrodesk.md` | RetroDesk | active | Documents the current event fixture mapping used by RetroDesk tests and native app/window behavior. |
| `wiios.md` | WiiOS | active | Documents the current WiiOS `manifest.ini` mapping and migration aliases. |
| `armonios.md` | ArmoniOS | planned/reference | Maps bootfs/app metadata, GUI events, and bare-metal boundaries to the shared contracts. |

## Adapter Rules

1. Keep platform glue local to the project.
2. Use these notes to document mapping decisions, not to define a framework.
3. Add fixtures only when behavior is useful to at least three projects.
4. If a project intentionally diverges from a contract, document the exception here.
5. Update `contracts/compliance.md` whenever adapter status changes.