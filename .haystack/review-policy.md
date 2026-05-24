# Review Policies

## Core object/variant/extension ABI safety
- **Paths**: `core/object/**`, `core/variant/**`, `core/extension/**`, `**/*.compat.inc`, `misc/extension_api_validation/**`
- **Severity**: critical
- **Reason**: AI changes here can silently break ABI/API compatibility for extensions and language bindings even when CI passes.

## Rendering, driver, and shader behavior
- **Paths**: `servers/rendering/**`, `drivers/vulkan/**`, `drivers/gles3/**`, `drivers/d3d12/**`, `servers/rendering/renderer_rd/shaders/**`, `drivers/gles3/shaders/**`, `scene/3d/**`, `scene/resources/material.cpp`
- **Severity**: critical
- **Reason**: Rendering/driver/shader diffs can compile and test cleanly while causing hardware-specific crashes, visual regressions, or synchronization bugs.

## Export/platform/project workflow integrity
- **Paths**: `editor/export/**`, `platform/*/export/**`, `platform/linuxbsd/export/**`, `platform/windows/export/**`, `editor/project_manager/**`
- **Severity**: high
- **Reason**: Exporter lifecycle and project workflow changes can regress end-to-end packaging behavior and notifications in ways static checks miss.

## Android lifecycle and runtime interop
- **Paths**: `platform/android/**`
- **Severity**: critical
- **Reason**: Lifecycle/JNI/plugin/proxy changes often fail only on real devices and can cause startup/runtime crashes not covered by CI.

## OpenXR/XR runtime integration
- **Paths**: `modules/openxr/**`, `modules/mobile_vr/**`, `scene/3d/xr/**`, `modules/cross_runtime/**`
- **Severity**: high
- **Reason**: XR session/extension/fallback behavior is runtime- and device-specific, so AI cannot reliably validate correctness from static diffs.

## Resource I/O, serialization, and loader lifecycle
- **Paths**: `core/io/**`, `core/io/resource_format_binary.cpp`, `scene/resources/**`, `core/variant/variant_parser.cpp`, `modules/zip/**`
- **Severity**: critical
- **Reason**: Parsing/loading/archive changes risk data loss, hangs, stale-cache behavior, or filesystem security flaws that automated checks may miss.

## Threading and editor interaction paths
- **Paths**: `editor/**`, `editor/gui/**`, `editor/docks/**`, `editor/scene/**`, `editor/inspector/**`, `editor/**/editor_resource_preview*`, `scene/gui/**`
- **Severity**: high
- **Reason**: Interactive editor/threading/layout regressions are state-dependent and often require manual UX testing across scenarios.

## Build system and CI orchestration
- **Paths**: `SConstruct`, `methods.py`, `platform_methods.py`, `site_scons/**`, `platform/*/detect.py`, `.github/workflows/**`, `drivers/sdl/**`
- **Severity**: high
- **Reason**: Build/CI edits can silently reduce platform coverage or break toolchain/cross-compile paths while green checks still appear.

## Audio threading and reconnect behavior
- **Paths**: `drivers/pulseaudio/**`, `servers/audio/**`
- **Severity**: high
- **Reason**: Small audio-thread changes can introduce stalls/deadlocks or unrecoverable reconnect failures not reproducible in standard CI.

## Third-party vendored library updates
- **Paths**: `thirdparty/**`
- **Severity**: high
- **Reason**: AI edits to vendored code can diverge from upstream and introduce long-term maintenance/security risks requiring expert verification.

## Platform windowing and native UI behavior
- **Paths**: `platform/windows/**`, `platform/macos/**`, `platform/ios/**`, `drivers/apple_embedded/**`, `scene/main/window.cpp`, `scene/gui/caption_button_overlay.cpp`
- **Severity**: high
- **Reason**: Platform-specific windowing/chrome/event behavior can regress only on target OS/device combinations and needs manual validation.

## Instructions
- If a change modifies public API shape, semantics, naming, or deprecation routing, a human must decide whether compatibility and migration impact are acceptable.
- If a PR changes guard behavior, error/warning severity, retry/fallback strategy, or strictness versus resilience, a human must confirm product expectations are still met.
- If optimization adds caching, custom fast paths, or extra state, a human must verify the measured benefit justifies readability, lifecycle, and maintenance costs.
- If new cross-module/layer dependencies are introduced, a human must assess architectural impact and long-term maintainability.
- If behavior differs across platforms/runtimes, a human must validate parity expectations and ensure no unintended regressions on unaffected targets.
- If docs significantly rewrite normative behavior wording or broad terminology, a human must verify technical accuracy and that churn is justified.
- If UI interaction/layout/discoverability patterns change, a human must decide whether usability and consistency tradeoffs are acceptable.
- If CI matrix size, timeouts, gating, or failure handling changes, a human must confirm runtime savings do not hide important regression signals.
- If a hardware/runtime-specific workaround is introduced, a human must evaluate affected-device scope and safety/performance tradeoffs before merge.
- If a PR combines a high-risk fix with unrelated extras, a human should decide whether to split changes to reduce regression risk and improve reviewability.
