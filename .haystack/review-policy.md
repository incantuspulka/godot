# Review Policies

## Core object lifecycle and extension ABI
- **Paths**: `core/object/**`, `core/extension/**`, `core/object/class_db.*`, `core/string/string_name.h`, `**/*.compat.inc`, `misc/extension_api_validation/**`
- **Severity**: critical
- **Reason**: Small lifecycle, registration, or API-metadata changes can silently break ABI, bindings, or runtime stability despite passing CI.

## Resource loader threading and teardown
- **Paths**: `core/io/resource_loader*`, `core/io/**`, `main/main.cpp`, `utils/async_loaded_resource.gd`, `utils/async_loaded_group.gd`
- **Severity**: critical
- **Reason**: Timing-sensitive loader and shutdown changes can cause deadlocks, hangs, or duplicate-load races that automated checks miss.

## Rendering backend, shaders, and driver paths
- **Paths**: `servers/rendering/**`, `servers/rendering/renderer_rd/**`, `drivers/vulkan/**`, `drivers/gles3/**`, `drivers/d3d12/**`
- **Severity**: critical
- **Reason**: GPU/driver and shader pipeline edits can introduce hardware-specific crashes, visual regressions, or synchronization bugs not covered by CI.

## OpenXR runtime and extension behavior
- **Paths**: `modules/openxr/**`, `scene/3d/xr/**`, `modules/mobile_vr/**`
- **Severity**: critical
- **Reason**: Runtime-dependent XR extension negotiation and frame behavior require device/runtime validation beyond static review.

## Android lifecycle and plugin integration
- **Paths**: `platform/android/**`, `platform/android/java/lib/src/main/java/org/godotengine/godot/plugin/**`
- **Severity**: critical
- **Reason**: Initialization order and Java/Kotlin interop changes can cause startup and nullability failures that are environment-dependent.

## Editor docks, plugins, preview, and undo lifecycles
- **Paths**: `editor/docks/**`, `editor/inspector/**`, `editor/scene/**`, `editor/plugins/**`, `editor/debugger/**`, `editor/editor_node.cpp`, `editor/project_manager/**`, `editor/file_system/**`
- **Severity**: high
- **Reason**: Interaction-heavy editor lifecycle changes can regress focus/layout/undo/teardown behavior in ways not reliably covered by tests.

## GUI, 3D, and simulation behavior paths
- **Paths**: `scene/gui/**`, `scene/3d/**`, `modules/csg/**`
- **Severity**: high
- **Reason**: Layout/input/physics/rendering semantics may remain build-correct while regressing user-visible behavior and edge-case correctness.

## Audio playback and realtime paths
- **Paths**: `modules/mp3/**`, `modules/vorbis/**`, `scene/resources/audio_stream_*.cpp`, `scene/resources/audio_stream_*.h`
- **Severity**: high
- **Reason**: Seek/loop/state and realtime-thread changes can introduce subtle desync or stalling behavior requiring interactive runtime validation.

## Export platform coupling and notifier lifetime
- **Paths**: `editor/export/editor_export_platform*.cpp`, `platform/linuxbsd/export/export_plugin.cpp`, `platform/windows/export/export_plugin.cpp`, `platform/**/export/export_plugin.cpp`
- **Severity**: high
- **Reason**: Cross-platform export flow or notifier-lifetime drift can produce incorrect completion behavior on specific platforms.

## Apple embedded packaging and templates
- **Paths**: `platform/ios/**`, `drivers/apple_embedded/**`, `misc/dist/apple_embedded_xcode/**`
- **Severity**: high
- **Reason**: Packaging/template wiring mistakes can build successfully but fail at runtime due to missing bundled artifacts.

## CI workflows and build graph/toolchain files
- **Paths**: `.github/workflows/**`, `.github/actions/**`, `SConstruct`, `**/SConscript`, `site_scons/**`, `platform/**/emscripten_helpers.py`
- **Severity**: high
- **Reason**: Workflow/build-graph edits can silently weaken checks, break cross-platform builds, or affect release/security posture.

## Archive extraction and IO security paths
- **Paths**: `core/io/**`, `modules/zip/**`
- **Severity**: high
- **Reason**: Archive path/permission handling changes can reintroduce traversal or unsafe restoration vulnerabilities not caught by normal tests.

## Third-party vendored code
- **Paths**: `thirdparty/**`, `modules/**/thirdparty/**`
- **Severity**: high
- **Reason**: Vendored dependency edits may diverge from upstream and introduce subtle ABI/behavior/security issues hard to validate locally.

## Class reference documentation bulk edits
- **Paths**: `doc/classes/**`
- **Severity**: medium
- **Reason**: Large doc rewrites can degrade technical precision and create significant translation churn despite passing lint/checks.

## Instructions
- If a change alters public signatures, enums, properties, extension contracts, or semantic behavior, a human must confirm migration and backward-compatibility impact is acceptable.
- If behavior shifts between fail-fast, warning-only, and fallback paths, a human must decide whether reliability, debuggability, and correctness tradeoffs are acceptable.
- If a change adds overhead in hot/realtime paths or introduces/removes performance workarounds, a human must confirm profiling-backed impact is acceptable.
- If editor UI behavior changes affect discoverability, density, workflow, focus, or interaction semantics, a human must decide whether the UX outcome is preferable.
- If documentation rewrites are broad or mostly stylistic, a human must decide whether clarity gains justify translation churn and wording changes.
- If a PR mixes distinct issues/features or introduces user-visible behavior changes late in a release cycle, a human must decide whether to split or defer.
- If a change relaxes CI strictness, build matrix coverage, or test-failure handling, a human must approve the confidence tradeoff for releases.
- If reported user-facing outcomes conflict with claimed intentional behavior, a human must decide whether to preserve behavior or treat it as a regression.
