---
name: unreal-engineering
description: Build, debug, reconstruct, and review Unreal Engine Blueprints, gameplay C++, editor tools, UMG/Slate, animation, materials, cooked builds, and game mods. Use for Unreal implementation and verification, including tidy graphs, lifecycle, controller navigation, version compatibility, and packaging.
license: MIT
---

# Unreal Engineering

Deliver maintainable Unreal work in independently testable steps. Preserve the
user's scope, existing working behavior, source references, and project conventions.
Use the tools actually available in the host; this skill assumes no particular
model, operating system, editor bridge, mod loader, or companion skill.

## Establish the target and checkpoint

1. Read applicable project instructions and the latest relevant checkpoint.
   Inspect the actual `.uproject`, plugin descriptors, source modules, assets,
   editor identity, and build configuration. Verify the engine version; an
   `EngineAssociation` may be an installation identifier rather than a version.
2. Identify the requested behavior, the state owner, and the smallest useful
   change that can be checked independently. State the plan in proportion to the
   task and identify the evidence needed to accept that change.
3. Resolve material unknowns from the project and installed tools first. Ask a
   focused question if a missing engine version, game build, loader contract,
   platform, or Blueprint-only restriction would change the implementation.
   Continue useful inspection that does not depend on the answer.
4. Preserve originals, user edits, and accepted checkpoints. Work in a separate
   variant where repair or reconstruction would overwrite reference material.
   Report unavailable source or inferred behavior explicitly.

## Read the reference matching the work

Read only the relevant topic files; combine them when the task crosses boundaries.

| Work | Reference |
| --- | --- |
| Graph generation, Blueprint state and input paths, graph presentation | [Blueprints](references/blueprints.md) |
| Gameplay C++, reflection, object lifetime, modules, editor plugins | [C++](references/cpp.md) |
| UMG, Slate, CommonUI, native menus, focus and controller navigation | [UI](references/ui.md) |
| Skeletal animation, contact, modular meshes, cloth, material fidelity | [Animation and materials](references/animation-and-materials.md) |
| Python/reflection, commandlets, bridge readiness, saving and reload | [Editor automation](references/editor-automation.md) |
| Cooking, chunks, pak/IoStore, staging and installation | [Packaging](references/packaging.md) |
| Host-game contracts, dummy assets, loader initialization, reconstruction | [Modding](references/modding.md) |

## Verify APIs against the actual engine

Use version-matched [Epic documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine)
and local engine headers/source to settle uncertain APIs, node signatures,
specifier behavior, lifecycle, threading, and compatibility. Use the documentation
version selector or supported `application_version` query parameter; check that
the returned page actually describes the requested version.

Treat local source for the installed engine fork and reflected editor metadata as
the authority for available functions. C++ availability does not establish Python
or Blueprint exposure. Inspect allocated pins for purity, execution flow,
parameter types, and by-reference outputs. Inspect a tool's actual schema before
generating calls; different graph adapters can use different argument formats.

When documentation or engine access is unavailable, identify the assumption and
the check still needed. Do not turn remembered signatures into verified claims.

## Design ownership before implementation

- Trace initialization, possession, repeated calls, asynchronous completion,
  teardown, reset, and map travel. Define who owns state and who restores it.
- Check invalid references, boundary values, units, coordinate spaces, and startup
  denominators. Check authority, replication, and client/server ownership when
  multiplayer is involved.
- Make tick and animation dependencies explicit. Read game-thread UObject state
  through the appropriate engine synchronization path before worker evaluation.
- Use events, functions, interfaces, and components by responsibility. Choose Tick,
  polling, casts, or searches when justified by the required behavior and measured
  cost. Prefer an owned reference or event where it removes repeated work.
- Keep tuning editable, grouped, named, and documented with units. Preserve native
  names that serialization, skeletons, sockets, or external callers depend on.
- Respect a Blueprint-only deliverable. Add C++ or an editor utility only when it
  fits the project's toolchain and authorized scope.

## Keep code and graphs readable

Follow existing conventions. Otherwise use Unreal native type naming and
consistent asset prefixes such as `BP_`, `WBP_`, `ABP_`, `BS_`, `M_`, `MI_`, `IA_`,
and `IMC_`. Keep cohesive functions and clear module responsibilities; keep
editor-only dependencies out of shipping runtime modules.

Lay Blueprint execution left to right, with coherent groups, useful reroutes,
consistent spacing, and comment frames around meaningful regions. Extract repeated
responsibilities when the interface becomes clearer. Remove abandoned debug nodes.
Inspect the actual saved graph in the editor when graph presentation is part of
the deliverable; successful compilation does not establish readable layout.

Comment intent, ownership, units, coordinate space, ordering, threading, and
non-obvious workarounds. Keep comments accurate and avoid narrating every node.

## Implement and verify each change

1. Establish a baseline or reproduction for behavior that can regress. Test a
   positive case before accepting a blocked/disabled case. Keep the change narrow
   enough to attribute a failure to it.
2. Confirm the project/editor identity and stop PIE before editing assets. Follow
   the project's shutdown and rebuild procedure for native changes. Distinguish
   Live Coding feedback from a cold build and fresh editor launch.
3. Compile affected C++ and Blueprints, inspect relevant warnings, save owned
   assets, and reload from disk. Check external actor packages when applicable.
   Tool success and an exit code are evidence to inspect, not proof of behavior.
4. Exercise the actual route changed: possessed-player input, selection and
   Accept/Back dispatch, startup hooks, animation transitions, or map travel.
   Label direct function calls and injected test state as narrower coverage.
5. Inspect rendered results when appearance matters. Check affected poses, contact,
   layout, text, materials, and supported resolutions. Keep offscreen previews,
   normal viewport behavior, and human control/feel judgments distinct.
6. When packaging is requested, inspect fresh cooked/container contents and launch
   the actual packaged target where tools and authorization permit. Preserve a
   rollback checkpoint and verify the installed files, not only build output.

Review the whole affected implementation before reporting completion: lifecycle,
failure paths, duplicated logic, unnecessary complexity, naming, graph quality,
tuning, and evidenced performance costs. Fix findings and rerun affected checks.
Remove temporary instrumentation or make it an intentional maintained tool.

## Report what the evidence establishes

| Claim | Required evidence |
| --- | --- |
| Builds or compiles | The affected build/Blueprint compilation ran successfully |
| Persists | A saved asset was reloaded and the changed state checked |
| Input/navigation works | The intended input and dispatcher route ran |
| Looks correct | The relevant rendered result was actually viewed |
| Package contains the change | Container inventory, identity, and changed payload were checked |
| Mod starts correctly | Runtime evidence from the target loader and world |
| Plays or feels right | The relevant human play/controller review occurred |

Report changed behavior, runnable artifact/checkpoint, checks actually performed,
and material limitations. A fixture, compile, or package check does not satisfy an
unperformed runtime, visual, controller, or multiplayer check. When access is
missing, give the specific next check and leave its result open.

## Recover from failures and maintain useful lessons

Pause the affected operation after an error instead of repeating an unchanged
failed action. Identify the failed assumption, preserve the useful evidence,
correct the cause, and rerun the reproduction. Add a proportionate prevention
check where it improves the task. Keep independent useful work moving.

Record task-specific lessons in the project's authorized log. Change the shared
skill only when the user requests maintenance or the repository's contribution
process authorizes it. A public lesson should state its trigger, applicable
version, correction, and verification, with private incident details removed.
Merge duplicate lessons and retire outdated ones. Skill text guides future
sessions; it does not retrain models or guarantee identical behavior.
