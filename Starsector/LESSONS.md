# Starsector Lessons

## Starsector modding: concrete UI, weapon-AI, and state lessons

- **Burst beam damage is not `beamDps * shortWindow`.** For weapon-AI heuristics, separate continuous beams from burst beams. A useful committed-burst estimate is `packetDamage = beamDps * (burstDuration + beamChargeupTime / 3 + beamChargedownTime / 3)`. This matched vanilla reference cases during AGC work: Phase Lance as `1000 * (1.0 + 0.375/3 + 0.375/3) = 1250`, and Tachyon Lance as `750 * (2.5 + 0.75/3 + 0.75/3) = 2250`. Validate beam formulas against known vanilla weapon data before using them for targeting, waste, or overkill decisions.

- **Continuous beams and burst beams need separate AI packet models.** For continuous beams, a short decision-window approximation such as `beamDps * 0.5s` may be reasonable for cleanup logic. For burst beams, use the committed burst packet instead; otherwise high-impact burst weapons can be misclassified as low-damage cleanup beams.

- **Avoid armor-grid or geometry simulation in generic per-frame weapon-AI checks.** Per-weapon/per-target heuristics should usually stay O(1). Avoid armor-cell prediction, projectile geometry simulation, and expensive target reconstruction in hot paths unless profiling and runtime testing justify the cost.

- **For overkill or waste avoidance, bad matchups may inflate durability; good matchups usually should not deflate it below baseline.** If a weapon is strong against armor or shields, lowering the target's apparent health can perversely discourage the right weapon from firing because the target looks "too easy" to kill. For waste/overkill heuristics, a safer simple model is baseline hull/armor/shield durability plus penalties only for poor damage-type matchups.

- **Do not solve Starsector custom-UI overflow by guessing raw font asset paths.** A raw `TooltipMakerAPI.setParaFont("graphics/fonts/...")` override can crash in specific UI paths, and built-in "small" font helpers can change the font family/style rather than preserving the original look at a smaller size. Prefer the default proven `addPara(...)` path unless a font method is known-safe in the exact target screen.

- **Shared GUI text helpers can turn a visual experiment into a global crash.** Centralizing text rendering is useful, but a helper that mutates tooltip font state can break every screen routed through it, including fallback/error UI. Keep shared text helpers behavior-preserving until the font/text method has been proven safe in all affected UI flows.

- **Parameterized tag labels containing `%` should be tested through the exact UI text path used to render them.** Some Starsector tooltip/text helper overloads may treat strings as formatted or highlighted text. Labels such as `A<80%>` or `TF>90%` are safest through the simplest proven plain-text rendering path.

- **Use forwarded `InputEventAPI` events for custom campaign/refit-style UI input.** Direct LWJGL mouse polling can desync from panel coordinates after layout rebuilds, scrolling containers, or nested UI changes. Prefer Starsector's forwarded input events and explicitly consume handled wheel/click events.

- **Treat `ShipAPI.customData` as an untyped shared namespace.** When fixing a wrong-key storage bug, do not blindly read old data from the wrong key. Check that the value has the expected storage type, then migrate narrowly. This avoids treating unrelated weapon-tag, ship-mode, or other mod data as valid state for another subsystem.

- **`TooltipMakerAPI.addAreaCheckbox(...)` color arguments are not a clean four-state button API.** The observed Starsector runtime behavior for `addAreaCheckbox(text, data, base, bg, bright, ...)` is that `base` drives hover/glow, `bg` drives checked fill/border, and `bright` drives the built-in label text color. If you render visible text separately, `bright` will not fix the apparent label color or hover/fill state. There is no constructor slot for separate unchecked-hover and checked-hover colors.

- **For custom toggleable Starsector buttons, use explicit post-creation overrides if the four visual states matter.** A reliable pattern is: create the area checkbox with blank text if you render your own label; use the constructor `bg` for checked fill/border; then apply `setGlowOverride(...)` and `setBorderOverride(...)` after creation based on `button.isChecked`. Keep the override path shared between equivalent controls so tags, modes, or other toggle families do not drift.

- **Area-checkbox glow can look like a bright border.** The observed render order is black base, checked fill if checked, border, superclass button render, then glow when `glowAmount > 0`. If hover/glow is bright and the unchecked interior remains black, the glow can read as a bright outline rather than a filled hover. `setBorderThickness(0f)` may not remove this if the apparent border is actually the glow region; a small negative border thickness can be worth testing because the renderer uses border thickness to position the black fill and glow rectangles.

- **Cache reflective UI override methods and apply them only when state changes.** Calling methods such as `setGlowOverride` or `setBorderOverride` by generic reflection on every rebuild/frame can introduce visible lag even for unrelated UI buttons. Cache `Method` handles by runtime button class and skip reapplying if the checked state has not changed.

- **Avoid extra row/container fills around area-checkbox controls unless you intentionally want a second visual layer.** A parent fill can mask hover, make selected state too bright, or make two controls with identical checkbox colors appear different. For checkbox-driven state, first test with the row/container `fillColor` unset, then add fill only if it is part of the intended design.

- **Raw Starsector UI colors often render much darker than their RGB values suggest.** Checkbox glow/hover colors in particular can be dimmed or tinted by the engine; current AGC visual evidence suggests roughly 75% effective dimming for these controls. It can be correct to use very bright raw colors, including white for a neutral hover, to get a readable dark-gray or saturated hover in game.

- **For `addAreaCheckbox(...)`, put hover colors in the `base` slot.** Passing a hover color as `bg` or `bright` will not make a normal unchecked button hover with that color. `bg` is checked fill/border, and `bright` is built-in label text color.

- **Do not debug Starsector UI color bugs as simple palette problems until the render path is understood.** In AGC, multiple color retunes failed because the issue was slot semantics and render layering: `base/bg/bright` did not mean idle/hover/selected-hover, built-in text was blank, `setBgOverride` duplicated constructor behavior, and extra row fill changed the apparent result. First identify which UI layer owns idle fill, checked fill, glow, border, and label text.

- **A checked and unchecked Starsector area checkbox is still one widget class.** If the UI appears to have separate selected and unselected button types, that may just be state-specific overrides applied after creation. Prefer one shared helper that chooses colors from `button.isChecked` over forking separate widget paths unless the layout or input model truly differs.

- **UI libraries are not always drop-in replacements for Starsector campaign buttons.** LunaLib has custom-rendered `LunaUIButton` / `LunaToggleButton` elements and MagicLib has combat/bounty UI helpers, but those are framework-level components rather than direct `ButtonAPI` replacements for an existing custom campaign screen. Treat a migration to those libraries as a UI rewrite, not a small color fix.

- **Kotlin top-level helpers can create runtime classloader surprises in Starsector UI paths.** A top-level function in `SomeUiFile.kt` generates `SomeUiFileKt.class`. Starsector custom UI/plugin paths may compile and package correctly but still fail at runtime with `NoClassDefFoundError` if that generated helper class crosses a script/plugin classloader boundary. Prefer methods on already-loaded classes, companion/object methods in proven files, or deliberately test the exact runtime entry point before trusting the extraction.

- **A `NoClassDefFoundError` after a harmless-looking UI cleanup may be a classloader-boundary problem, not a missing compile dependency.** Check whether the change introduced a new generated Kotlin `*Kt` class, superclass, helper class, or moved code across package/class boundaries. Inspect the jar contents and test the affected in-game entry point; `compileKotlin` and `jar` are necessary but not sufficient for custom UI refactors.

- **Runtime-loaded Starsector UI code favors boring duplication over untested abstraction.** If extracting two duplicated lines introduces a new runtime-visible class, helper file, or superclass, the abstraction can be riskier than the duplication. In custom UI hot paths, prefer small helper methods inside an already-instantiated class or an existing stable utility object unless the new boundary has a concrete payoff and an in-game validation plan.

- **For cramped Starsector UI labels, truncate after wrapping, not before.** If a cell has two visual lines, first pack whole words into the two-line area, then add an ellipsis only to the final visible line if text remains. Truncating the raw string before wrapping can waste the second line and produce useless output such as a bare `...` where a meaningful word plus ellipsis would fit.
