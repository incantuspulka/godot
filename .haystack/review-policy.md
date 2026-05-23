# Review Policies

## Core object and extension ABI risk
- **Paths**: `core/object/**`, `core/extension/**`, `core/variant/**`, `core/string/string_name.h`, `misc/extension_api_validation/**`, `**/*.compat.inc`
- **Severity**: critical
- **Reason**: Small changes can silently break reflection, scripting, GDExtension ABI/API compatibility, and downstream integrations that CI may not fully cover.

## Threading and resource-loader lifecycle
- **Paths**: `core/io/resource_loader*`, `core/io/resource*`, `core/io/**`, `editor/file_system/**`, `editor/**/resource*`, `servers/**`
- **Severity**: critical
- **Reason**: Timing-dependent loader/cache/teardown and locking changes can introduce deadlocks, hangs, stale resources, or lifetime bugs that are hard to reproduce in CI.

## Rendering backends and GPU driver behavior
- **Paths**: `servers/rendering/**`, `drivers/vulkan/**`, `drivers/gles3/**`, `drivers/d3d12/**`
- **Severity**: critical
- **Reason**: Backend synchronization, present/reprojection, and driver-workaround edits can cause platform-specific visual regressions, stutter, hangs, or crashes not reliably caught by automated tests.

## Export pipeline and platform plugin flows
- **Paths**: `editor/export/**`, `platform/*/export/**`
- **Severity**: high
- **Reason**: State-machine and lifecycle changes can pass tests yet break end-to-end export behavior, preset integrity, or completion signaling across platforms.

## Platform runtime/windowing/input integration
- **Paths**: `platform/android/**`, `platform/android/java/**`, `platform/windows/**`, `platform/linuxbsd/**`, `platform/macos/**`, `platform/web/**`, `scene/main/window.cpp`, `scene/gui/caption_button_overlay.cpp`
- **Severity**: high
- **Reason**: OS/device-specific lifecycle, windowing, DPI/RTL, display, and input behavior needs real-environment validation because regressions often evade CI.

## XR runtime and session negotiation
- **Paths**: `modules/openxr/**`, `scene/3d/xr/**`, `modules/mobile_vr/**`
- **Severity**: critical
- **Reason**: Extension negotiation, session/view configuration, and swapchain lifecycle behavior varies by runtime/device and can fail only on hardware matrices.

## Audio realtime threading paths
- **Paths**: `drivers/pulseaudio/**`, `servers/audio/**`, `audio/**`
- **Severity**: high
- **Reason**: Realtime and reconnection path changes can introduce deadlocks, glitches, and stalls that static checks cannot reliably detect.

## Public API documentation contracts
- **Paths**: `doc/classes/**`, `modules/*/doc_classes/**`
- **Severity**: medium
- **Reason**: Docs are user-facing API contracts; inaccurate or broad rewrites can mislead users and trigger large translation churn despite passing code checks.

## CI workflows and build tooling
- **Paths**: `.github/workflows/**`, `.github/actions/**`, `SConstruct`, `site_scons/**`, `platform/**/detect.py`, `misc/scripts/**`
- **Severity**: high
- **Reason**: Workflow/build-graph changes can weaken validation or break platform builds and packaging in ways not fully exercised by one PR run.

## Archive extraction and path-security code
- **Paths**: `core/io/**zip**`, `core/io/**archive**`
- **Severity**: critical
- **Reason**: Archive parsing/extraction edits can reintroduce traversal and permission vulnerabilities that require careful human security review.

## Instructions
- If a change alters public API behavior, signatures, enums, or property names, a human must decide whether compatibility impact and migration cost are acceptable for the target release.
- If a PR changes locking, signal dispatch, loader teardown, or threaded behavior, a human must verify lock ordering, wait behavior, and object lifetime safety under contention.
- If a change introduces hot-path optimizations, allocations, or extra branching/helpers, a human must decide whether measured gains justify complexity and maintenance cost.
- If a PR changes warning versus error severity or strict failure versus fallback behavior, a human must confirm the new reliability, UX, and diagnostics tradeoff is appropriate.
- If a PR changes export validation/state flow or editor interaction defaults/visibility/focus semantics, a human must verify the user workflow impact is intentional and acceptable.
- If documentation edits are mostly wording or alter user mental models, a human must decide whether clarity gains justify translation churn and accurately match real behavior.
- If a PR changes CI matrices, compatibility checks, triggers, or continue-on-error behavior, a human must decide whether coverage/security reductions are acceptable.
- If a change modifies rendering backend fallback/workaround behavior, a human must assess compatibility gains versus visual quality and performance regressions.
