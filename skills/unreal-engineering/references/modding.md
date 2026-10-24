# Host-game integration and modding

Use for game mods, native menu integration, compile-time stubs, loader ownership,
and cooked-content reconstruction. Confirm contracts from the exact host build
and loader implementation; similarly named loaders can behave differently.

## Identify the host contract

Determine the game's build, engine/toolchain requirements, native class/package
paths, input/UI systems, container format, and supported mod-loading route from
available files and primary tool/game references. Inspect installed versions
before selecting an extractor, mappings file, codec, cook, or converter.

Classify the change: asset replacement, defaults/data edit, new Blueprint behavior,
native integration, or map addition. These have different entry requirements.
Some games support data-driven or asset-based behavior without a ModActor; some
require a specific Blueprint loader or native plugin. Ask only for missing facts
that change the route.

Use mappings that match the target's unversioned property layout where required.
Inspect import/export errors and representative content rather than assuming a
successful mount means the extraction is complete. Cooked content does not
automatically contain editable native code or a reconstructable original graph.

## Compile-time stubs and runtime identity

When the workflow needs a hollow reference asset, reproduce the required package
path, parent, reflected fields, and exact types from the host. Preserve numeric
precision as declared; UE5 does not turn every floating-point field into a double.
Recursively resolve referenced types as necessary.

Keep compile-only stubs out of the distributed payload unless intentionally
replacing that exact asset is the task. A stub accidentally shipped over a native
asset can replace working behavior with an empty class. Verify container inventory
and actual runtime cast/load resolution against the original host asset.

## Loader initialization and duplicate prevention

Read the supported entry package, actor/level naming, filename rules, install
directory, hook timing, and map-travel behavior from the actual loader. The
`/Game/Mods/<Name>/ModActor` pattern is common in some Blueprint loaders, not a
universal entrypoint. A hook name alone does not establish player readiness.

When normal BeginPlay and loader hooks can both initialize the mod, route them
through an idempotent owner guard scoped to the intended world/lifetime. Reject
duplicates without destroying the established owner's UI, subscriptions, bridge,
or state. Keep rejected instances from running background work during cleanup.

Test actual hook order, repeated calls, forced duplicate spawns, teardown, and
completed map travel. Count live world actors rather than all retained UObject
instances. Test compatibility with each loader configuration explicitly requested.

## Input and native UI integration

Inspect the game's binding route before choosing a hotkey implementation. Use
available input actions, loader callbacks, or a suitably scoped polling fallback
according to the target. If polling is required, handle press/release edges,
focus, pause, and teardown, and bound the search/update cost.

For menu integration, inspect the actual host widget, control style, dispatcher,
input router, and owning stack. Use a lifecycle event or bounded discovery when
possible. A recurring search must have a justified lifetime and cleanup. Menus
may be recreated on each open; injection must be idempotent per instance.

Preserve native controls, spacing, sounds, focus timing, and camera behavior when
the request calls for a native appearance. Test the actual selection-to-Accept and
Back/return routes, not only a mouse delegate or directly opened test widget.

## Additive maps and content reconstruction

Preserve host-game class paths and use the smallest authoring/reference layer
needed. Avoid copying an entire dependency closure when the host already supplies
it. Preserve the native GameMode/pawn contract; an editor fly-preview override can
otherwise serialize a camera-only player into the shipped map.

Check startup components, timelines, navigation, collisions, materials, and dynamic
loads separately when investigating a pre-play map freeze or inert actors. Saved
fixtures and cold package checks do not prove the map loads and plays in the host.
For AI, observe ordinary behavior-tree/timer updates without injected interaction
state before claiming autonomous behavior. Keep cleanup/reservation injection
coverage distinct from naturally occurring interactions.

## Deliver and diagnose from evidence

When a mod does not load, inspect the installed container set and mount paths,
loader discovery logs, entry contract, runtime dependencies, initialization, and
host build compatibility. Follow observed failure evidence rather than changing
suffixes or loader conventions blindly.

Deliver only task-owned content and instructions appropriate to the target.
Report what was checked in the editor, container inspection, runtime loader,
native gameplay, visuals, physical controller, and multiplayer. Preserve an
accepted baseline and supply one independently testable revision when the user
has requested step-by-step in-game testing.
