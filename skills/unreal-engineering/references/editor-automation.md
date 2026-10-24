# Editor automation, persistence, and recovery

Use for Python, native editor helpers, commandlets, bridge calls, bulk imports,
and save/reload checks. Match API exposure and tool schemas to the installed build.

## Verify identity and operation-specific readiness

Before mutation, confirm the editor executable, engine version, absolute project
identity, world, and PIE state. A connected bridge or a `running` flag does not
mean the next operation's dependency exists. Wait with a bounded deadline for the
actual pawn, controller, asset, or subsystem needed next.

Stop PIE and verify it has stopped before asset mutations. Handle a busy import
or compiler as a state to inspect rather than repeatedly issuing the same request.
If an import stalls a user's editor, preserve saved data and diagnostics; recovery
that could discard unsaved edits requires the user's authorization. An isolated
project/commandlet path can provide a safer reproduction.

## Reflection and call boundaries

Inspect Python exposure and reflected functions rather than translating a C++
name mechanically. Native make/break functions, protected properties, pure nodes,
and struct-array copies can require different access paths. Confirm an immediate
state change after a call, then confirm the saved result independently.

Load required assets explicitly in a cold commandlet phase before looking up
their inner/template objects. A previous live-editor phase may have loaded them
implicitly. Test each intended independent phase in a fresh process.

Check complete logs, completion markers, and process exit. A final shutdown line
can follow an earlier exception; a zero exit status can accompany a failed
packaging helper. Validate the produced artifact and the intended state.

## Save the package that owns the change

Use editor transactions and mark the appropriate objects modified before changing
user assets. Save only the packages owned by the task. Preserve unrelated dirty
changes and restore temporary class-default edits without saving them.

World Partition actors can live in external actor packages. A successful map save
does not establish that their component settings or transforms persisted. Save
the affected external actors explicitly and validate them in a fresh process with
the required descriptors/actors loaded.

For imported transforms, compare cold-reloaded placement, bounds, and contact.
Euler serialization near pitch singularities can differ from a live quaternion
pose. Preserve coordinate conventions and named pitch/yaw/roll semantics, and
check meaningful mesh bounds/contact points rather than only arbitrary axes.

## Commandlet paths and argument handling

Pass arguments as arguments rather than composing untrusted values into shell or
Python source. Keep structured job data in a separate file when that avoids code
injection and quoting ambiguity.

In observed UE 5.4 Python commandlets, backslash-containing script paths could be
interpreted as escapes even after process quoting. Normalize path separators to
forward slashes for that commandlet interface and test a real path containing
spaces and an escape-like segment. Verify the path handling used by the current
launcher instead of prescribing one shell across all platforms.

## Validation fixtures and useful tools

Keep fixtures outside the player's test lane and preserve intended collisions.
Inspect complete phase traces, including controls, positions, and initialization
timing. A sample taken before the next input tick does not describe the full phase.
Hold a suitable strong reference to long-lived offscreen UObject fixtures.

Create an editor helper only when it materially reduces repeated work or errors
in scope. Verify build, registration, real editor invocation, undo/cleanup, and
fresh-process operation. Report exactly which behavior the helper measured;
headless asset/layout checks do not establish rendered or human-play acceptance.
