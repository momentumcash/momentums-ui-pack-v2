# Juice: motion that feels good (Momentum UI Pack)

Written for the pack. Deep-dive: the `roblox-ui-dynamic-juice` skill (Kitkopika) if installed. Timing basics: `izzy-interaction-motion.md`.

## Never animate a layout-owned Size/Position
Inside UIListLayout/UIGridLayout that jitters siblings (grid overwrites CellSize). Use **Slot → Hitbox → Visual + UIScale**: the layout-sized button (Hitbox, `AutoButtonColor = false`) stays still so its click area never flickers; scale the centred inner Visual (AnchorPoint 0.5,0.5) through a UIScale. Lift the slot's ZIndex on hover.

## Pick the motion engine
- Simple hover/press: `StyleRule:SetPropertyTransition` on `:Hover`/`:Press` (RepeatCount/Reverses are ignored).
- Interruptible, tactile: a spring (e.g. Fraktality's `spr`) so enter → leave → click keeps velocity with no tween races.
- Choreographed sweeps: TweenService (never the deprecated `:TweenSize`/`:TweenPosition`) or one shared `RunService.PreRender` loop driving `UIGradient.Offset` for all shines.
- Cancel/replace the previous tween when state changes fast.

## CanvasGroup traps
It forces ClipsDescendants (inset content so lips/shadows aren't cut), blurs on low graphics, degrades above 2048px, and **re-rasterizes every frame if any child changes**. Use it only for the open/close `GroupTransparency` fade; set `Interactable = false` the moment closing starts and `Visible = false` when closed. Keep shimmers and live timers outside a resting CanvasGroup.

## Layering
Move the outer container; animate sparkles/gleams inside clipped inner frames; let shadows trail the card by ~0.05–0.10 s.

## Numbers
Routine controls 0.12–0.30 s. Back/Elastic/Bounce sparingly. Reward reveals and wins can be bigger: result → reward → VFX/SFX → details → clear next button.

## Touch, gamepad, accessibility
No hover scaling on Touch. Keep SelectionGained/Lost feedback for gamepad. Reset stuck presses on `UserInputService.InputEnded`. `GuiService.ReducedMotionEnabled` → bounces/zooms/shimmers collapse to quick fades.

## UI sounds
Preload (`ContentProvider:PreloadAsync`), debounce hover (~0.035 s), vary PlaybackSpeed ±3–5%, clone short sounds so fast clicks don't cut tails. Short and restrained; important actions may sound stronger.
