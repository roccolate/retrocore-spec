# 0001 — Contracts Before Code

Date: 2026-07-01

## Decision

The retro systems family will share contracts, fixtures, and terminology before sharing runtime code.

## Context

RetroTUI, RetroDesk, WiiOS, and ArmoniOS solve related problems, but their platforms are very different:

- Python/curses terminal desktop
- C terminal desktop
- Nintendo Wii homebrew launcher
- Bare-metal AArch64 OS

A shared runtime would create artificial constraints and likely duplicate platform glue in worse forms.

## Consequences

- Shared specs define behavior and vocabulary.
- Each project keeps its own platform implementation.
- Fixtures become the first practical integration point.
- Shared code may appear later only after repeated patterns are proven.

## Rule

- 1 project: keep local.
- 2 projects: document as contract.
- 3 projects: add fixture/test vector.
- 4 projects: consider tiny shared tooling.
