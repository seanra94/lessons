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
