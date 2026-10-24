# Blueprint implementation and reconstruction

Use for execution graphs, generated nodes, Blueprint state, input, and reconstruction.
The API-specific observations below originate in UE 5.4/5.6 work; confirm the
installed version before applying a workaround.

## Trace the actual caller

Start from the real entry event and follow execution and data to the final state
change. Identify shared functions, automatic callers, latent actions, ownership,
and reset paths before changing a gate.

An input-only toggle belongs at the identified input boundary when automatic
systems share the underlying action. For example, disabling a manual ragdoll
button must preserve collision and unsafe-landing callers of the shared ragdoll
function. Check enabled, disabled, re-enabled, and automatic cases independently.

Enhanced Input tests must model action order and held state. In observed UE 5.4
chord tests, event dispatch preceded the updated bound action values; starting
both actions in one injected pass produced a false rejection. Establish the
enabled baseline and inspect input processing before diagnosing a policy gate.

## Inspect reflected nodes and pins

- Resolve the owning class, exact function name, parameter type, and available
  pins from installed headers/reflection. Math function names and numeric pin
  types can differ across versions.
- Determine purity from the allocated node. A remembered `BlueprintCallable`
  annotation is insufficient to decide whether execution pins must be connected.
- Follow by-reference outputs through their actual storage. Bytecode reconstruction
  must preserve local-output versus ordinary-local expression semantics.
- Preserve default values, latent execution, validity checks, delegate bindings,
  and data ownership. A graph that compiles can still contain pruned nodes or
  silently zero-valued adapter arguments.
- Verify generated defaults on the saved asset and, where relevant, on the spawned
  instance. Tool adapters may accept different property names for node arguments.

## Readable graph delivery

Keep execution left to right and group operations by intent. Name intermediate
values when their units or role are otherwise unclear. Use reroutes for long
wires and comment frames for meaningful regions. A small operation does not need
several extra functions merely to satisfy an abstraction pattern.

Compile, save, reopen, and inspect generated graphs. Check warnings, disconnected
execution paths, misplaced defaults, and the actual layout. Headless compilation
does not verify graph presentation.

## Reconstruct behavior from evidence

Keep source dumps and reference projects unchanged. Classify recovered assets,
serialized defaults, Blueprint bytecode, animation node data, native code, and
visual observations separately. Missing native implementations remain a gap even
when a reconstructed Blueprint loads.

For animation graphs or bytecode, validate index/field-order interpretation on a
small connected example before rebuilding the whole graph. Recover each getter,
blend condition, transition, and state owner rather than substituting guessed
behavior. Keep uncertain decoding rules scoped to the tested engine format.

## Verify the route, not only the helper

Exercise startup, repeated invocation, invalid references, reset, and travel when
affected. A direct movement or function-call test is useful unit coverage, while
the possessed-player input path requires a separate run. Likewise, directly
opening a modal does not prove that selecting its entry button and pressing
Accept reaches it.

When an editor commandlet calls actor script functions directly, confirm the
script actually executed with a non-default state change. In UE 5.4 native test
code, an appropriate `FEditorScriptExecutionGuard` can be needed at that boundary;
check the installed engine implementation before adding it.
