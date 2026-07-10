# App Manifest Contract

Status: draft

Defines how a retro-system app describes itself to a launcher, desktop, or OS app registry.

## Goals

- Stable app identity across projects.
- Minimal metadata useful to terminal desktops, Wii launchers, and bare-metal app registries.
- Human-editable format.
- Easy validation.

## Non-Goals

- No shared loader/runtime.
- No requirement that every project supports external apps.
- No POSIX assumptions.

## Canonical Fields

```ini
[app]
id=files
title=Files
kind=system_app
entry=files
icon=folder
version=0.1.0
requires=input,storage,window
```

| Field | Required | Meaning |
|---|---:|---|
| `id` | yes | Stable lowercase app id. Suggested charset: `a-z`, `0-9`, `_`, `-`. |
| `title` | yes | Human-facing label. |
| `kind` | yes | `system_app`, `bundled_app`, `external_app`, `demo_app`, or project-defined extension. |
| `entry` | yes | Project-local launch target: Python module, C registry id, Wii app path, bootfs image, etc. |
| `icon` | no | Symbolic icon id, not necessarily a file path. |
| `version` | no | App version string. |
| `requires` | no | Comma-separated capabilities. |

## Initial Logical App IDs

The current event fixtures use a small shared vocabulary of logical app ids.
Projects map these ids to their own local app registry entries.

| Logical app id | Meaning | Example project-local mapping |
|---|---|---|
| `files` | File manager / storage browser | RetroDesk `filemanager` |
| `notes` | Simple notes/text editor | RetroDesk `notepad` |

This vocabulary is intentionally small and draft-stage. Adding new shared app ids
should be driven by fixtures or cross-project behavior tests, not by one
project's internal app names.

## Capability Vocabulary

Initial shared capabilities:

- `input`
- `storage`
- `window`
- `network`
- `audio`
- `shell`
- `settings`
- `timer`

Projects may define local capabilities with a prefix, e.g. `wii:sd`, `arm:el0`, `terminal:pty`.

## Failure Semantics

If an app cannot launch, the host should return a structured failure with:

- app id
- failure code
- human message
- optional platform detail

Suggested codes:

- `not_found`
- `missing_capability`
- `invalid_manifest`
- `launch_failed`
- `permission_denied`
- `unsupported_platform`

## Compatibility Notes

- RetroTUI can map plugins/apps to manifests.
- RetroDesk can map C app registry entries to manifests.
- WiiOS can align existing `manifest.ini` files with this contract.
- ArmoniOS can map bootfs/user app metadata to this contract without changing its binary format.
