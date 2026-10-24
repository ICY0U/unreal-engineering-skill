# Cooking, packaging, and installed-content verification

Use for standalone builds and mod containers. Determine the engine, platform,
build target, host game build, and packaging format before choosing commands.

## Establish the real route

Inspect project settings, packaging presets, installed files, and the target's
documented pipeline. `.utoc`/`.ucas` indicate IoStore containers; a companion `.pak`
may also be present. A `.pak` inventory alone does not establish the contents of
an IoStore package store. Keep required companion files together.

Cooking transforms editor assets into platform data. Packing uncooked source
assets does not make them loadable in a cooked game. Use the matching supported
editor/toolchain and required customizations. Executable file properties are one
hint about a game's version, not proof of every serialization or custom-version
requirement.

## Control cook inclusion and dependencies

Inspect Asset Manager settings, maps, hard/soft references, primary asset labels,
and explicit package lists. Dynamic string-only loads may require explicit cook
inclusion. Choose chunk IDs and labels from the target's actual allocation; there
is no universal mod ID range or one chunk policy for every project.

Keep the mod's intended payload separate from compile-time stubs and base-game
dependencies. Inspect the final cook manifest and containers for both missing
content and accidental inclusion of reference/dummy assets.

In observed UE 5.4 single-package cooks, the mode skipped soft-reference/CookDir
discovery. A successful CookDir-only invocation could produce no intended mod
actor. Inspect the current command's discovery rules and use explicit long package
names when needed; assert the changed actor and defaults are in fresh output.

## Build, stage, and identify fresh output

Use the project's existing supported BuildCookRun or editor packaging preset as
the baseline. Confirm project, target, configuration, platform, maps, build/cook
flags, and output path from its resolved command and logs. Blueprint-only projects
may need different build flags/receipts from a native project.

Separate stale output from the new run with a dedicated staging directory or
scoped cleanup of generated artifacts. Preserve unrelated builds and source data.
Check timestamps/hashes or another reliable identity before accepting output.

Filename suffixes such as `_P`, mount priority, alphabetical ordering, folder
names, and ModActor naming are host/loader contracts to verify. They are not a
single rule for all Unreal games and container types.

## Container and install acceptance

1. Inspect fresh manifests and package inventories using tools compatible with the
   target container version. A legacy pak can be listed with the matching
   UnrealPak; IoStore content needs a compatible package-store inspection path.
2. Check expected long package/mount paths, changed defaults/content, dependency
   imports, and absence of unintended stubs. A container integrity pass does not
   prove that its imports resolve or that it contains the requested change.
3. For independently distributed IoStore mods, inspect internal container identity
   where relevant. Renaming files does not necessarily change a container ID;
   chunk names alone do not prove separate package stores.
4. Back up files being replaced and install the required container set together.
   Verify installed hashes/identity against the staged result. Preserve a clear
   rollback route.
5. Launch the actual packaged target or game when authorized and available. Check
   startup logs, real asset loads, map travel, and changed behavior. Keep gameplay,
   visuals, physical controller, and multiplayer acceptance explicit.

## IoStore conversion is a separate check

Some conversion tools need matching script-object metadata from the host's global
containers and matching retail containers to resolve native and game-package
imports. Use an isolated local input set matched to the target build and filter
the intended mod output. Base-game containers are references, not mod payload.

Inspect imports for unresolved/unknown packages after conversion and exercise
actual packaged soft loads. Require the output files expected by that tool and
package format; not every valid package has an identical sidecar layout. A round
trip can provide dependency evidence without proving the real mod loader starts
the content or multiplayer state replicates correctly.
