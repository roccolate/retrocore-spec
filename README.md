# retrocore-spec

Shared contracts and fixtures for the retro systems family:

- RetroTUI — Python/curses desktop reference
- RetroDesk — portable C terminal desktop
- WiiOS — Nintendo Wii homebrew shell/launcher
- ArmoniOS — bare-metal AArch64 desktop OS

This repository intentionally starts with **contracts before code**.

The goal is to avoid duplicated decisions across projects without forcing all systems into one shared runtime.

## Rules

1. Share contracts, fixtures, and vocabulary first.
2. Do not share runtime code until at least three projects prove the same shape is needed.
3. Keep platform glue local.
4. A contract may be partially implemented by a project.
5. If a contract makes a project less honest to its platform, the project wins.

## Structure

```text
contracts/  human-readable specs
fixtures/   sample manifests/events/themes used by tests
adapters/   notes for project-specific adapters; no framework yet
decisions/  architecture decision records
```

## Adoption Order

1. App manifest
2. Logical events
3. Window model
4. Theme tokens
5. Optional tiny tools/adapters later

See `~/RETRO_SYSTEMS_MAP.md` for the working map.
