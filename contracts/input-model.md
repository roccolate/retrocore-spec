# Input Model Contract

Status: placeholder

This contract will map raw platform input into logical events defined in `events.md`.

Examples:

- keyboard key -> `key`
- mouse click -> `pointer_down` / `pointer_up`
- Wii PAD/WPAD button -> `key` or project-specific launcher action
- HID/virtio-input event -> GUI logical event

Keep raw scan codes and platform-specific button constants out of shared fixtures unless explicitly needed.
