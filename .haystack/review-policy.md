# Review Policies

## Core object system and extension ABI changes
- **Paths**: `core/object/**`, `core/extension/**`, `core/extension/gdextension_interface.json`, `core/config/engine.*`, `**/*.compat.inc`
- **Severity**: critical
- **Reason**: Small semantic or contract changes here can break scripting, reflection, deprecated build modes, and third-party extensions without obvious CI failures.

## Rendering backend and driver internals
- **Paths**: `servers/rendering/**`, `drivers/vulkan/**`, `drivers/gles3/**`, `drivers/d3d12/**`, `servers/rendering/renderer_rd/**`, `servers/rendering/renderer_scene_render_rd/**`
- **Severity**: critical
- **Reason**: Minor logic, synchronization, or workaround changes can cause GPU-specific crashes, corruption, or severe regressions that automated checks rarely cover.

## Platform integration and XR runtime behavior
- **Paths**: `platform/**`, `modules/openxr/**`, `modules/mobile_vr/**`, `modules/visionos_xr/**`, `scene/3d/xr/**`
- **Severity**: high
- **Reason**: Behavior depends on OS/device/runtime specifics that cannot be fully validated in CI, so subtle lifecycle/fallback regressions may slip through.

## Export pipeline and platform packaging
- **Paths**: `editor/export/**`, `platform/*/export/**`
- **Severity**: high
- **Reason**: Small export-flow or notifier changes can silently break signing, packaging, or completion semantics across platforms.

## Build system and CI pipeline definitions
- **Paths**: `SConstruct`, `SConscript`, `site_scons/**`, `methods.py`, `platform_methods.py`, `platform/**/detect.py`, `platform/**/SCsub`, `.github/workflows/**`, `.github/actions/**`
- **Severity**: high
- **Reason**: Toolchain detection and workflow edits can weaken validation or break cross-platform builds in ways not obvious from a single CI run.

## Resource loading, scanning, and cache lifecycle
- **Paths**: `core/io/**`, `scene/resources/**`, `editor/**/resource*`, `editor/file_system/**`, `modules/gdscript/**`
- **Severity**: high
- **Reason**: Recognition/caching/lifecycle changes can cause uncached runtime failures, hangs, or severe scan performance regressions that are hard to detect automatically.

## Editor UI/docking/layout interaction flows
- **Paths**: `editor/docks/**`, `editor/inspector/**`, `editor/scene/**`, `scene/gui/**`, `editor/project_manager/**`
- **Severity**: high
- **Reason**: Layout/input/focus/persistence regressions often require manual interactive testing and are not fully covered by automated tests.

## Audio threading and device reconnect paths
- **Paths**: `drivers/pulseaudio/**`, `servers/audio/**`
- **Severity**: high
- **Reason**: Threading and reconnect changes can introduce dropouts, deadlocks, or device-specific runtime failures not caught by static checks.

## Class docs and generated artifact files
- **Paths**: `doc/classes/**`, `misc/extension_api_validation/**`, `editor/translations/**/*.po`
- **Severity**: medium
- **Reason**: Large or direct edits to generated/reference artifacts can degrade link correctness, translation stability, or contract consistency despite passing CI.

## Third-party vendored code
- **Paths**: `thirdparty/**`
- **Severity**: high
- **Reason**: Changes to vendored code can diverge from upstream and create long-term security/update maintenance risks requiring human validation.

## Instructions
- Require human judgment when a change removes or weakens null checks, safety guards, fallbacks, teardown protections, or early-exit paths.
- Require human judgment when behavior changes between hard-fail and fallback/permissive handling in runtime, export, or SDK/tooling flows.
- Require human judgment when a PR changes issue severity (warning vs error) or suppresses repeated errors, to ensure debugging and UX tradeoffs are acceptable.
- Require human review when new public fields, flags, callbacks, methods, or tuning knobs are added to decide if long-term API cost is justified.
- Require human review when changes introduce new cross-layer dependencies or architectural coupling between major subsystems.
- Require human review with measured evidence when a PR changes hot-path allocations, lookups, data structures, caching, or threading for performance.
- Require human judgment when work scheduling/thread-affinity semantics change across main thread, worker threads, deferred callbacks, or extension call paths.
- Require human judgment for large wording-only documentation rewrites to confirm semantic accuracy and justify translation churn.
- Require human judgment when editor/UI interaction patterns, control visibility, icons, filters, or menu placement change.
- Require human judgment when renaming/reordering user-facing labels or changing compatibility behavior to balance cleanup against migration risk.
- Require human judgment when rendering features/backends are disabled or fallback behavior changes for specific devices.
- Require human judgment when CI matrix coverage, sanitizer scope, or job timeouts are changed.
