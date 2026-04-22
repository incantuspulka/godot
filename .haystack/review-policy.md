# Review Policies

## Core object/variant/extension ABI and lifecycle
- **Paths**: `core/object/**`, `core/variant/**`, `core/extension/**`, `modules/gdextension/**`, `misc/extension_api_validation/**`, `**/*.compat.inc`
- **Severity**: critical
- **Reason**: High risk of ABI/compatibility, refcount, lifecycle, and extension breakage that automated checks may miss.

## Rendering backends and shader pipeline
- **Paths**: `servers/rendering/**`, `drivers/**`, `*.glsl`, `scene/resources/material*`
- **Severity**: critical
- **Reason**: Renderer/shader changes can cause platform/GPU-specific crashes or visual regressions not fully covered by CI.

## Editor UI/scene/script interaction flows
- **Paths**: `editor/gui/**`, `editor/scene/**`, `editor/docks/**`, `editor/inspector/**`, `editor/script/**`, `editor/shader/**`, `scene/gui/**`
- **Severity**: high
- **Reason**: Interactive UX, focus/layout/state behaviors require manual end-to-end validation.

## GDScript parser/analyzer and language tooling
- **Paths**: `modules/gdscript/gdscript_analyzer.*`, `modules/gdscript/gdscript_parser.*`, `modules/gdscript/gdscript_tokenizer.*`, `modules/gdscript/editor/**`
- **Severity**: high
- **Reason**: Small logic errors can cause widespread diagnostics/type-inference regressions and editor instability.

## Debugger transport and serialization
- **Paths**: `editor/debugger/**`, `scene/debugger/**`, `editor/script/script_editor_debugger.cpp`
- **Severity**: critical
- **Reason**: Protocol and serialization changes carry security and cross-version interoperability risks.

## Platform, build system, and dependency acquisition
- **Paths**: `platform/**`, `SConstruct`, `SCsub`, `methods.py`, `platform/*/platform_methods.py`, `misc/build_deps/**`, `misc/scripts/install_*`, `*.build`, `*.py`
- **Severity**: high
- **Reason**: Cross-platform build behavior and trust boundaries can regress despite partial CI coverage.

## CI workflow and secret/auth-sensitive automation
- **Paths**: `.github/workflows/**`
- **Severity**: critical
- **Reason**: Workflow condition/auth/secret changes can weaken security or silently reduce validation coverage.

## XR/runtime integration
- **Paths**: `modules/openxr/**`, `modules/visionos_xr/**`, `modules/mobile_vr/**`, `scene/3d/xr/**`, `servers/xr/**`, `platform/visionos/**`
- **Severity**: high
- **Reason**: Runtime/device-specific extension negotiation and lifecycle issues require manual runtime testing.

## Resource loading, serialization, and duplication
- **Paths**: `core/io/**`, `scene/resources/**`, `scene/main/**`
- **Severity**: high
- **Reason**: Threaded loading and resource duplication/typing regressions can cause deadlocks, corruption, or hard-to-reproduce runtime failures.

## Mono/C# integration and startup registration
- **Paths**: `modules/mono/**`, `main/**`, `editor/file_system/**`
- **Severity**: high
- **Reason**: Initialization/order changes can break class discovery, plugin loading, and editor startup behavior.

## Documentation generation and class reference surfaces
- **Paths**: `doc/classes/*.xml`, `editor/editor_help.cpp`, `editor/doc_tools.cpp`, `doc/tools/**`
- **Severity**: high
- **Reason**: Doc/tooling changes can create semantic inconsistencies not reliably caught by automated checks.

## Instructions
- If checks, fallbacks, or compatibility paths are removed/weakened, a human reviewer must confirm invariants and failure-mode tradeoffs are acceptable.
- For changes to defaults, warnings, interaction models, or workflow semantics, a human reviewer must assess usability, discoverability, migration impact, and documentation clarity.
- When previously internal functionality is exposed to scripting/public API, a maintainer must judge long-term stability, naming, and maintenance cost.
- If a fix improves correctness but may break existing projects/extensions, a human must decide whether to preserve compatibility, gate behavior, or stage migration.
- When behavior/data-structure changes are justified by performance claims, reviewers should verify measurement quality and whether costs in complexity/coverage are justified.
- Changes to deferred execution, threading, notification timing, callback/signal ordering, or fallback priority should be manually validated for safety and semantic correctness.
- If workflow conditions, matrix, timeouts, or resource limits are changed, reviewers should assess whether any speed gains justify reduced defect-detection confidence.
- Changes that alter implicit network/dependency/download behavior or other trust-sensitive defaults should be reviewed for user consent and expected workflow impact.
- Large new features should be accepted only with clear evidence of demand and sustainable long-term maintenance/documentation cost.
- When docs change API ownership boundaries, depth/structure, or terminology with semantic implications, reviewers should confirm clarity and maintainability.
