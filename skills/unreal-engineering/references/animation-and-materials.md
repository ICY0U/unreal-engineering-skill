# Animation, modular meshes, cloth, and materials

Use for skeletal ownership, attachment/contact, cloth persistence, imported
materials, and visual fidelity. Practical observations originate in UE 5.4/5.6;
inspect the actual skeleton, component class, and engine implementation.

## Animation ownership and sampling

Identify which layer owns translation, lean, aim, and pose adjustments. Applying
the same adjustment in a parent pivot and an animation graph can double it.
Make units, normalization ranges, and zero-safe startup/reset behavior explicit.

Pose leaders must update before dependent followers and attachments. Check the
final runtime socket/bone relationship rather than only editor component transforms.
Sample comparisons after the relevant animation and component updates. An editor
bridge call made between component ticks can show one current pose and one stale
pose; a common offset across several limbs can be a sampling/order fault.

Compare local/component-space motion as well as world-space motion. Actor
translation alone can make an unmoving ankle appear animated. Measure a target's
distance from the chain root against the chain's reachable length before changing
an IK solver. Distinguish unreachable targets from in-range solve/order failures.
Use current evaluated pose data for corrections that must avoid a frame of lag,
with the proper animation thread data path.

Check animation budgets and update-rate settings on components other meshes follow.
A dependency on a leader updating every frame must be reconciled with throttling;
inspect lifecycle registration and settings before assuming the budget system is
enabled or disabled.

## Modular character and material ownership

Inspect the component names, classes, bone maps, material slots, and active
instances on the actual runtime character. Template names alone can differ from
runtime names or lookup semantics.

Preserve underlying body meshes and accepted outfit variants. Use an outfit-specific
mask or section policy for covered geometry and inspect coverage from several
angles and relevant poses. A pose-sharing check does not establish absence of
body penetration.

For scoped dynamic-material changes, identify the actual equipped item and the
native writer of each parameter. Apply at the correct update point, keep the
unmodified source value separately, and avoid compounding an already scaled value.
Restore an override only while it is still owned by the system making the change;
respect newer overrides from other systems.

## Cloth binding and cold persistence

An authored cloth object, a successful binding call, and a saved mesh are separate
facts from active render-LOD cloth data and nonzero simulated particles. In
observed UE 5.4 authoring, section mapping needed to be persisted through the
skeletal mesh post-edit/rebuild lifecycle. Inspect Persona's implementation and
verify the saved binding in a fresh process before tuning simulation.

Use the actual target leader/component class in acceptance fixtures. Check cloth
asset GUID/LOD/section correspondence, effective masks, collision bind basis, and
the simulated particle count. Painted roots do not by themselves prove effective
solver pins. Preserve closure geometry and seam attachment deliberately when
splitting simulated and skinned sections.

Test startup, motion, sharp transitions, teleport/reset, and relevant frame rates.
Inspect pin drift, collision, stretch, and visible penetration. Numerical stability
and a headless component simulation leave normal gameplay appearance open.

## Imported materials and texture fidelity

Audit the full affected material family and all slots. Preserve texture source,
UVs, channel packing, color space, compression, sampler types, blend mode, two-sided
behavior, and skeletal/cloth usage flags. A compiled material can still have the
wrong roughness, opacity, normals, or surface scale.

When reconstructing cross-version data, preserve factory-owned texture source
bytes and metadata together; importing partial serialized settings can corrupt
that relationship. Explicitly restore connection/output defaults when a partial
import would otherwise retain an earlier disconnected state. Render and cook
inherited variants as well as the base asset.

Verify a setter by reading the result back and reloading the saved asset. Some
UE 5.4 material-instance setters were observed returning false despite applying
the value; inspect the installed implementation before treating that return as a
reliable success flag. Likewise verify reflected struct-array assignments after
writeback rather than assuming edits to a temporary struct persisted.

Separate shader-compilation warm-up from steady-state performance measurements.
Use a real renderer for texture/shader appearance checks and inspect the resulting
images. Report cold persistence, component simulation, rendered preview, and
in-game artistic acceptance as distinct evidence.
