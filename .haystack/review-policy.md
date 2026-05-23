# Review Policies

## Core object/extension ABI surface
- **Paths**: `core/object/**`, `core/extension/**`, `core/variant/**`, `core/string/**`
- **Severity**: critical
- **Reason**: Changes can silently break object lifecycle, bindings, and extension ABI/API compatibility despite passing CI.

## Rendering backend and driver internals
- **Paths**: `servers/rendering/**`, `drivers/vulkan/**`, `drivers/gles3/**`, `drivers/d3d12/**`
- **Severity**: critical
- **Reason**: Renderer and driver changes can cause hardware/vendor-specific crashes, correctness issues, or performance regressions not covered by CI.

## Android lifecycle and runtime paths
- **Paths**: `platform/android/**`
- **Severity**: critical
- **Reason**: Initialization/lifecycle/input/plugin ordering changes can break startup or runtime behavior on real devices in ways tests miss.

## Editor interaction and workflow paths
- **Paths**: `editor/scene/**`, `editor/docks/**`, `editor/inspector/**`, `editor/gui/**`, `editor/script/**`
- **Severity**: high
- **Reason**: Interactive editor behavior (undo/redo, selection, docking, text interactions) is regression-prone and requires manual UX validation.

## Export, build, and packaging flows
- **Paths**: `editor/export/**`, `platform/*/export/**`, `SConstruct`, `site_scons/**`, `misc/scripts/**`
- **Severity**: high
- **Reason**: Small logic changes can break export/build artifacts or platform packaging despite green CI on limited matrices.

## Resource loading and teardown concurrency
- **Paths**: `core/io/**`, `core/resource/**`, `core/io/resource_loader.cpp`
- **Severity**: critical
- **Reason**: Timing-sensitive loading/shutdown changes can introduce deadlocks or hangs that are hard to reproduce automatically.

## Audio threading and driver behavior
- **Paths**: `servers/audio/**`, `drivers/pulseaudio/**`, `scene/resources/audio_stream_*`
- **Severity**: high
- **Reason**: Audio thread and reconnect changes can trigger deadlocks, dropouts, or instability only under real runtime conditions.

## OpenXR runtime integration
- **Paths**: `modules/openxr/**`, `scene/3d/xr/**`
- **Severity**: high
- **Reason**: Extension negotiation and session behavior vary by runtime/device, so correctness requires device-level validation.

## Platform windowing integration
- **Paths**: `platform/windows/**`, `platform/macos/**`, `platform/linuxbsd/**`, `servers/display_server.cpp`, `scene/main/window.cpp`
- **Severity**: high
- **Reason**: OS-specific windowing, DPI, and event-loop behavior can regress in ways not captured by generic tests.

## Vendored third-party modifications
- **Paths**: `thirdparty/**`
- **Severity**: high
- **Reason**: Direct vendored edits can create upstream divergence and hidden security/maintenance risks that need human judgment.

## Class reference documentation changes
- **Paths**: `doc/classes/**`
- **Severity**: medium
- **Reason**: Doc edits can alter API meaning or create large translation churn even when syntax checks pass.

## CI workflows and actions
- **Paths**: `.github/workflows/**`, `.github/actions/**`
- **Severity**: high
- **Reason**: Workflow changes can alter test coverage or secret exposure in ways not evident from a single passing run.

## Instructions
- Require human judgment when a change alters whether failures hard-fail, warn, retry, or silently fall back, because compatibility, debuggability, and safety tradeoffs are contextual.
- Require human judgment when public API surface or extension behavior changes to decide if compatibility impact is acceptable for the target release.
- Require human judgment when optimizations add architectural/state complexity to verify measured gains justify maintenance cost.
- Require human judgment for editor interaction semantic changes (selection, focus, layout, visibility, panning, warnings) to confirm real workflow usability.
- If a change crosses subsystem boundaries or removes abstraction layers, a human must decide whether the exception is architecturally acceptable.
- Require human judgment for substantial documentation rewording to ensure behavioral meaning remains accurate and translation churn is justified.
- Require human judgment when platform-specific branches or feature gating logic changes to validate behavior on non-primary targets.
- Require human judgment when work moves between threads/deferred/main paths because correctness and responsiveness tradeoffs are context-dependent.
- Require human judgment for changes to warning/error surfacing to balance diagnostics value against user-facing noise or duplication.
- Require human judgment when parsers/loaders or inputs become stricter or more tolerant to balance interoperability, security, and compatibility.
- Require human judgment when disabling features/backends for compatibility to assess stutter, visual quality, and device impact tradeoffs.
- Require human judgment when CI branch/event gating changes to ensure sufficient coverage and safe secret handling.
