# Review Policies

## Core object/variant and GDExtension ABI-sensitive changes
- **Paths**: `core/object/**`, `core/variant/**`, `core/string/**`, `core/extension/**`, `core/extension/gdextension_interface.json`, `**/*.compat.inc`
- **Severity**: critical
- **Reason**: Small changes can silently break reflection, scripting bindings, ABI compatibility, and downstream extensions in ways CI may miss.

## Serialization and resource loading lifecycle
- **Paths**: `core/io/**`, `core/io/resource_loader.*`, `core/io/resource_format_binary.cpp`, `scene/resources/resource_format_text.cpp`, `core/variant/variant_parser.cpp`, `scene/resources/**`, `utils/async_loaded_resource.gd`, `utils/async_loaded_group.gd`
- **Severity**: critical
- **Reason**: Parser, serialization, and threaded load lifecycle edits can introduce corruption, deadlocks, or shutdown hangs that are timing-dependent and hard to catch automatically.

## Rendering drivers, shaders, and temporal pipeline
- **Paths**: `servers/rendering/**`, `servers/rendering/renderer_rd/**`, `servers/rendering/renderer_*/*taa*`, `servers/rendering/renderer_*/*ssr*`, `servers/rendering/renderer_*/*ssil*`, `drivers/vulkan/**`, `drivers/gles3/**`, `drivers/**`
- **Severity**: critical
- **Reason**: Backend and temporal rendering changes can cause vendor-specific crashes, races, or visual regressions that require real hardware validation.

## Platform runtime and build/toolchain scripts
- **Paths**: `platform/**`, `platform/**/display_server_*.cpp`, `drivers/pulseaudio/**`, `servers/audio/**`, `SConstruct`, `site_scons/**`, `misc/scripts/install_*.py`, `misc/scripts/install_*.sh`, `misc/scripts/validate_extension_api.sh`, `modules/mono/build_scripts/**`, `platform/web/emscripten_helpers.py`
- **Severity**: high
- **Reason**: Platform and toolchain changes can introduce OS/device-specific hangs, packaging failures, or build breakage not covered by default CI.

## Editor export/import and notifier lifecycle
- **Paths**: `editor/export/**`, `editor/import/**`, `platform/*/export/**`
- **Severity**: high
- **Reason**: Export pipeline and notifier lifetime changes can silently produce broken or misleadingly successful exports across platforms.

## Editor UI lifecycle and live-edit/debug flows
- **Paths**: `editor/docks/**`, `editor/scene/**`, `editor/script/**`, `editor/gui/**`, `editor/inspector/**`, `scene/debugger/**`, `scene/gui/**`
- **Severity**: high
- **Reason**: Complex editor state, threading, and UX interactions can regress workflows in ways static checks cannot validate.

## OpenXR runtime lifecycle and extension negotiation
- **Paths**: `modules/openxr/**`, `scene/3d/xr/**`, `modules/mobile_vr/**`
- **Severity**: high
- **Reason**: XR runtime behavior varies by vendor/device, so fallback/session/extension changes need hardware-based human validation.

## Apple embedded linking and bundling
- **Paths**: `platform/ios/**`, `platform/macos/**`, `platform/apple/**`, `drivers/apple_embedded/**`, `misc/dist/apple_embedded_xcode/**`
- **Severity**: high
- **Reason**: Apple platform link/bundle rules are device-specific; incorrect changes can pass CI but fail launch/export on target devices.

## CI workflows and shared actions
- **Paths**: `.github/workflows/**`, `.github/actions/**`
- **Severity**: high
- **Reason**: Workflow condition or matrix edits can silently reduce required validation coverage while still passing syntax checks.

## Third-party vendor updates
- **Paths**: `thirdparty/**`
- **Severity**: high
- **Reason**: Vendored library changes can introduce compatibility, maintenance, and security risks requiring human review.

## Archive extraction and path validation security
- **Paths**: `modules/zip/**`, `core/io/**`
- **Severity**: critical
- **Reason**: Archive handling edits can reintroduce path traversal or unsafe permission restoration vulnerabilities.

## Class reference mass-edit risk
- **Paths**: `doc/classes/**`
- **Severity**: medium
- **Reason**: Large documentation sweeps can introduce semantic drift and translation churn that automated checks do not evaluate for quality.

## Instructions
- If a PR changes editor interactions, visibility/defaults, or close/restore behavior, a human must decide whether usability, discoverability, and safety tradeoffs are acceptable.
- If a change alters fail-fast behavior, warning/error severity, or fallback strategy, a human must confirm the user-impact tradeoff is correct.
- If a PR expands script-facing APIs or changes public API semantics, a human must assess long-term compatibility and stability impact.
- If documentation rewrites change implied guarantees or cause broad translation invalidation, a human must decide whether clarity gains justify the cost.
- If a PR changes hot-path allocation/container/lookup behavior, a human must verify performance impact using profiling or representative measurements.
- If work moves between background and main/deferred execution or shared-state assumptions change, a human must validate correctness tradeoffs.
- If platform/device heuristics or hardcoded thresholds are introduced, a human must verify they generalize across real hardware and environments.
- If workflow triggers, trusted-event guards, sanitizer/toolchain matrix entries, or critical checks are narrowed, a human must confirm coverage remains sufficient.
- If a change renames public properties, removes parallel interfaces, or cleans up legacy behavior, a human must decide whether compatibility impact is acceptable.
