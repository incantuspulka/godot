# Review Policies

## Core reflection, Variant, and extension ABI surfaces
- **Paths**: `core/object/**`, `core/variant/**`, `core/string/**`, `core/extension/**`
- **Severity**: critical
- **Reason**: Changes can silently break scripting/language bindings and binary compatibility in ways CI may not fully detect.

## Threaded loading and resource lifecycle internals
- **Paths**: `core/io/**`, `scene/resources/**`, `utils/async_loaded_resource.gd`, `utils/async_loaded_group.gd`
- **Severity**: critical
- **Reason**: Threaded loading and teardown edits can introduce nondeterministic hangs, deadlocks, or shutdown races that are hard to validate automatically.

## Archive extraction path and permission handling
- **Paths**: `core/io/**`, `modules/zip/**`
- **Severity**: critical
- **Reason**: Archive parsing/extraction changes can reintroduce path traversal or unsafe permission restoration vulnerabilities.

## Rendering, shader, and backend driver paths
- **Paths**: `servers/rendering/**`, `drivers/gles3/**`, `drivers/vulkan/**`, `scene/3d/**`
- **Severity**: critical
- **Reason**: Small changes can cause race conditions, GPU/vendor-specific regressions, or subtle visual defects not reliably caught by CI.

## XR/OpenXR runtime integration
- **Paths**: `modules/openxr/**`, `scene/3d/xr/**`, `modules/mobile_vr/**`
- **Severity**: high
- **Reason**: Runtime/device-specific extension negotiation and session behavior require hardware validation beyond automated tests.

## Apple embedded export packaging
- **Paths**: `platform/ios/**`, `platform/visionos/**`, `drivers/apple_embedded/**`, `misc/dist/apple_embedded_xcode/**`
- **Severity**: critical
- **Reason**: Packaging/linking mistakes may compile but fail on device or distribution workflows.

## Windows message loop and DPI/titlebar behavior
- **Paths**: `platform/windows/**`, `editor/project_manager/**`, `editor/editor_node.cpp`, `scene/gui/caption_button_overlay.cpp`
- **Severity**: high
- **Reason**: Changes can introduce freezes, shutdown issues, or OS integration regressions that require manual runtime verification.

## Editor export and packaging flow
- **Paths**: `editor/export/**`, `editor/run/**`, `platform/*/export/**`
- **Severity**: high
- **Reason**: Export lifecycle/notifier mistakes can silently break multi-platform packaging behavior not fully covered by tests.

## Android interop and runtime hot paths
- **Paths**: `platform/android/**`
- **Severity**: high
- **Reason**: JNI/proxy contract errors and hot-path allocation changes can cause runtime crashes or performance regressions not seen in static checks.

## Editor UI layout, previews, and interaction flows
- **Paths**: `editor/inspector/**`, `editor/docks/**`, `editor/scene/**`, `scene/gui/**`, `editor/script/**`, `scene/debugger/**`
- **Severity**: high
- **Reason**: Interaction semantics and preview lifecycle behavior often require manual interactive testing to catch regressions.

## Class reference XML changes
- **Paths**: `doc/classes/*.xml`, `modules/*/doc_classes/**`
- **Severity**: medium
- **Reason**: Wording/signature edits can break cross-references, translation workflows, or user-facing API accuracy despite passing CI.

## CI workflow orchestration and coverage gates
- **Paths**: `.github/workflows/runner.yml`, `.github/workflows/*_builds.yml`
- **Severity**: high
- **Reason**: Workflow condition/matrix edits can silently reduce platform/test coverage while pipelines remain green.

## Build graph and SCons/codegen pipeline
- **Paths**: `SConstruct`, `SCsub`, `site_scons/**`, `platform/web/emscripten_helpers.py`
- **Severity**: high
- **Reason**: Build system changes can impact reproducibility and cross-platform toolchains in ways single-path CI cannot prove safe.

## Third-party vendored dependency updates
- **Paths**: `thirdparty/**`
- **Severity**: high
- **Reason**: Vendored library changes carry upstream divergence, security, and licensing risks requiring human review.

## Audio driver threading and reconnect behavior
- **Paths**: `drivers/pulseaudio/**`, `servers/audio/**`
- **Severity**: high
- **Reason**: Thread-state/reconnect edits can introduce deadlocks, glitches, or assertion failures that are timing-dependent.

## Instructions
- If a change removes or weakens validation checks, fallback paths, null/lifecycle guards, or early-fail behavior, a human must confirm the invariants hold in real scenarios.
- If public API shapes, names/order, defaults, casting behavior, or extension interfaces change, a human must assess migration cost and compatibility risk.
- If editor interaction semantics (selection, drag/drop, previews, shortcuts, sizing, discoverability) change, a human should validate the UX tradeoff with manual testing.
- If a PR adds caching or other optimizations that increase complexity, a human should verify the measured benefit justifies maintenance and memory costs.
- If threaded vs non-threaded behavior, synchronization, deferral, or lifecycle ordering changes, a human must validate correctness across platforms/callers.
- If a change switches between hard failure, warning, retry, degrade, or default fallback behavior, a human must judge the strictness/availability tradeoff.
- If warning or error emission frequency/severity changes, a human must balance diagnosability against noise and operational impact.
- If many existing docs/tooltips/messages are rewritten without behavior change, a human should confirm the clarity gain justifies translation invalidation.
- If new script-visible properties, methods, enums, or tuning knobs are introduced, a human must decide whether long-term API maintenance cost is justified.
- If workflow matrix entries, guards, or timeouts change, a human must confirm runtime/cost savings do not remove essential validation coverage.
