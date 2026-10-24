# UMG, Slate, CommonUI, and native menus

Use for widgets, HUDs, menus, layout, UI performance, focus, and controller routes.
Version-specific persistence and preview observations originate primarily in
UE 5.4. Check the exact engine and the host game's UI contracts.

## Fit the project's architecture

Keep state and reusable behavior in an appropriate owner, with layout and art in
widgets. A C++ base with a Widget Blueprint child is useful when C++ is available;
preserve Blueprint-only projects and mod contracts. Add required modules according
to public/private usage rather than copying a dependency list blindly.

For player-facing widgets, establish the correct local owning player, retain the
intended reference, and add the widget to its panel, viewport, or CommonUI stack.
Choose retention, hiding, or removal from the actual lifetime requirement. Bind
and unbind delegates consistently across reconstruction and repeated presentation.

`BindWidget` names and types must match the Blueprint child and be checked by its
compilation. `BindWidgetOptional` allows an intentionally absent child. Confirm
animation-binding metadata, including transient requirements, in the target engine.
Guard members until initialization has established them.

## Input and return ownership

Inspect the real opener, input router, stack owner, focus target, and Back/close
path. Preserve the native CommonUI activation and input configuration when used.
Otherwise choose the controller input mode and cursor policy required by the page.
Restore the previous valid owner on return; a nested menu may need UI focus rather
than a hard-coded return to gameplay.

World pause and menu ownership are separate facts. A native pause page may remain
active while `IsGamePaused` is false. Use page lifetime or stack state when deciding
which UI should regain focus or which HUD hints should be hidden.

Exercise the real sequence: select the entry, press Accept, navigate inside, press
Back, and reopen. Cover any preview-key handling, custom controller dispatcher,
enabled/validity checks, and click delegates along that route. Mouse clicks and a
direct modal-opening fixture leave controller entry behavior untested.

For saved explicit navigation, verify the persisted target name after save/load.
In UE 5.4, a template pointer supplied to an explicit-navigation setter can be
replaced by name resolution during initialization; inspect `WidgetToFocus` and
test the instantiated widget. Runtime route changes can use live instances.

## Match native appearance and measure actual layout

Reuse the host's fonts, controls, materials, sounds, spacing, focus behavior, and
camera flow when the request is to look native. Preserve native borders and
padding in measurement fixtures; simplified controls can hide text overflow.

- Author around a consistent target resolution/scale and test supported sizes.
- Measure actual allotted geometry, including DPI and slot padding, alongside
  desired size. Use Fill/Auto intentionally at every relevant slot boundary.
- Check wrapped text, long captions, scroll limits, footer visibility, and focus
  at the smallest supported viewport.
- Enable size overrides through the appropriate setter or override flag; a saved
  numeric height alone does not establish an active `SizeBox` height override.
- Drive page visibility from its explicit state and owning frame. Run ordinary
  Tick/refresh paths after opening a page to detect a later visibility overwrite.

## Rendering and visibility evidence

Inspect the image actually produced. A successful renderer call and correct UMG
properties do not establish a complete preview. Offscreen Slate layout may need
prepass/paint settling; multiple virtual windows and cached clipping can produce
incomplete images. Keep preview workarounds in the test helper until runtime
behavior justifies a production change.

In UE 5.4, `IsVisible` can depend on a cached Slate widget. Distinguish property
visibility from a materialized Slate tree in headless fixtures. Geometry measured
with a null renderer is layout evidence, not rendered appearance. Physical
controller feel and normal in-game visuals require their own checks.

## Performance

Prefer event-driven updates for state that changes on events. Use recurring work
only at the frequency needed. Profile expensive bindings, layout invalidation,
large child populations, and nested scaling instead of banning useful panels.
Use virtualization or separate loading when the population warrants it.

Choose invalidation, volatility, and retainer rendering from measured behavior and
memory/render costs. Keep non-interactive elements out of hit testing while
preserving interactive descendants. Use the target engine's Widget Reflector,
Slate stats, and tracing to identify the actual owner of a cost.

[Epic UMG best practices, UE 5.4](https://dev.epicgames.com/documentation/en-us/unreal-engine/umg-best-practices-in-unreal-engine?application_version=5.4)
is a starting reference; resolve details against the installed version.
