# WiiOS Adapter Notes

WiiOS consumes retrocore contracts as manifest/event vocabulary, not as a runtime dependency.

## App Manifest

WiiOS `sdroot_template/wiios/apps/*/manifest.ini` maps to `contracts/app-manifest.md`.

Canonical fields:

```ini
[app]
id=hello
title=Hello App
kind=demo_app
entry=app.elf
icon=icon.bin
version=0.1.0
requires=input,storage,window
```

Legacy WiiOS v0.1 aliases:

- `name` -> `title`
- `type` -> `kind`

The WiiOS parser currently accepts both during migration.

## Boundaries

Keep these Wii-native:

- libogc boot/input/render/storage glue
- Homebrew Channel package layout
- PAD/WPAD constants
- SD/USB path resolution
- `.dol` build and package flow
