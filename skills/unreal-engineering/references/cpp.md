# Gameplay C++ and editor modules

Use for reflected classes, modules, object lifetime, gameplay ownership, and
native editor tooling. Resolve signatures in the project's exact engine source.

## Module and header boundaries

Read existing `.Build.cs`, `.Target.cs`, and plugin descriptors before adding a
dependency. A public header exposing another module's types can require a public
dependency; implementation-only dependencies usually belong in the private list.
Examples include `UMG`, `Slate`, `SlateCore`, `EnhancedInput`, `AIModule`, and
`NavigationSystem`; link the modules actually used.

Keep `.generated.h` as the final include in a reflected header. Forward-declare
types where the full definition is unnecessary and include the defining header
where needed. Use the module export macro on symbols that must cross a module
boundary; every class does not automatically need export.

Follow the engine's reflected type conventions: `A` for actor classes, `U` for
UObject classes, `F` for ordinary structs, and `E` for enums. Unreal interfaces
have both a reflected `U` interface and an `I` implementation interface. Match
project conventions for Slate types, booleans, and assets.

## Object ownership and lifetime

Choose pointer semantics deliberately. For a persistent strong UObject member
owned by a reflected UObject, a `UPROPERTY()`-marked `TObjectPtr<T>` is the usual
UE5 form. A `TObjectPtr` alone does not make every storage location visible to GC.
Use weak references when lifetime is owned elsewhere; soft references can defer
asset loading. Non-UObject owners and asynchronous work require the appropriate
engine lifetime mechanism rather than assuming a raw pointer keeps an object alive.

Create default subobjects in constructors. For runtime components, establish the
outer/ownership, attachment, registration, and teardown appropriate to the actor.
Keep editor transactions and asset mutations out of runtime construction paths.

Guard references at lifecycle boundaries: world, pawn, controller, owner, casts,
widget members, and objects pending destruction. Do not paper over a broken
required invariant with a silent early return; log or assert it at a useful
development boundary.

## Reflection and editable settings

Use property exposure to express intent: `EditDefaultsOnly` for class defaults,
`EditInstanceOnly` for placed-instance tuning, `VisibleAnywhere` for component
references that should not be reassigned, and Blueprint access only where needed.
Group exposed settings and document units and valid ranges. Metadata such as
`ClampMin` improves editing; validate runtime input as well.

Check `BlueprintCallable`, `BlueprintPure`, native/implementable events, and
delegate signatures against UHT and actual allocated nodes. A dynamic delegate
handler requires the matching reflected function. `BlueprintNativeEvent` native
defaults use the generated `_Implementation` route for that event.

## Startup, tick, and cleanup

Make Tick eligibility explicit and select its frequency from the behavior.
Use delegates or timers for state changes and slower periodic work when appropriate.
Declare tick prerequisites when components depend on one another's current-frame
state. Preserve thread-safe animation/async boundaries.

Make initialization idempotent if callers can repeat. Pair subscriptions, timers,
async callbacks, UI, and transient registrations with teardown at the owner’s
lifetime boundary. Use engine-aware callback bindings or weak references where
appropriate, and account for captured objects as well as `this`.

For multiplayer, distinguish authority, replicated state, RPC ownership, and local
presentation. Test the actual server/client role affected; a standalone run does
not establish replication.

## Editor utilities and plugins

Inspect existing tools before introducing a plugin. Keep editor dependencies in
an editor or appropriately uncooked module. Support transactions/undo for user
asset mutations, preserve unrelated dirty assets, and verify real editor
registration, invocation, and cleanup. Scope utilities to the current task.

## Build and diagnostic evidence

Use module log categories for maintained diagnostics and include enough identity
to distinguish worlds, actors, and phases. Investigate compiler/UHT/linker messages
at their source: missing dependencies, exports, includes, or reflected signatures.

Build affected native code and compile affected Blueprint children. For reflection,
class-layout, constructor/default-subobject, or module changes, use the project's
closed-editor rebuild and fresh-launch procedure. Live Coding can give useful
iteration feedback but does not establish a clean cold build or persisted defaults.
