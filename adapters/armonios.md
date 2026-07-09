# ArmoniOS Adapter Notes

Status: planned/reference

ArmoniOS should consume retrocore contracts as boot/app metadata vocabulary, GUI behavior vocabulary, and host-test fixture vocabulary. It must not consume this repository as a shared runtime dependency.

ArmoniOS is the bare-metal AArch64 desktop OS in the retro systems family. Its kernel, drivers, scheduler, memory model, boot flow, and compositor rules remain ArmoniOS-native.

## App Manifest

ArmoniOS may map bootfs or app-registry metadata to `contracts/app-manifest.md` like this:

| retrocore field | ArmoniOS source |
|---|---|
| `id` | stable bootfs/app registry id |
| `title` | user-facing app label |
| `kind` | `system_app`, `bundled_app`, `external_app`, or local extension |
| `entry` | bootfs image, executable path, service id, or app registry target |
| `icon` | symbolic icon id, embedded asset id, or bootfs asset reference |
| `requires` | logical capabilities such as `input`, `storage`, `window`, `timer`, or prefixed local capabilities |

ArmoniOS may define prefixed local capabilities such as:

- `arm:el0`
- `arm:mmu`
- `arm:fb`
- `arm:virtio`
- `arm:bootfs`

These should remain local unless another project needs the same vocabulary.

## Logical Events

ArmoniOS may map raw input into `contracts/events.md` for GUI behavior tests, emulator tests, or host-side replay tests.

Suggested mappings:

| Raw source | Logical event |
|---|---|
| USB HID keyboard | `key` |
| virtio-input keyboard | `key` |
| USB HID pointer | `pointer_down`, `pointer_move`, `pointer_up` |
| GUI app-launch command | `launch_app` |
| compositor focus command | `focus_next` |
| window close request | `close_window` |

Raw scan codes, interrupt details, device descriptors, and driver constants should stay outside shared fixtures unless explicitly needed for a hardware-specific test.

## Window Model

ArmoniOS may implement `contracts/window-model.md` through a kernel-owned or compositor-owned model.

Expected mapping:

| retrocore concept | ArmoniOS interpretation |
|---|---|
| Window | compositor-managed surface or app-owned drawable region |
| Focused window | surface receiving keyboard/input dispatch |
| Z-order | compositor stacking order |
| Modal window | compositor-enforced focus restriction |
| Minimized window | hidden surface still represented in app/task state |
| Maximized window | surface occupying available desktop area |

Partial implementation is allowed while the OS is early. Do not add kernel or compositor internals to the shared contract.

## Theme Tokens

ArmoniOS may map `contracts/theme-tokens.md` into its framebuffer/compositor palette or GUI style table.

Projects may quantize or reinterpret token values. The token names are the shared contract; the exact rendering is platform-local.

## Boundaries

Keep these ArmoniOS-native:

- bootloader and boot protocol
- exception levels and privilege transitions
- MMU/page table design
- scheduler and process/thread model
- syscall ABI
- executable format and loader
- bootfs and storage drivers
- framebuffer/GPU/compositor implementation
- HID/virtio/raw driver details
- panic/debug/logging infrastructure