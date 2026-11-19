# Style craft (Momentum UI Pack)

Written for the pack. Full deep-dive: the `roblox-ui-style-craft` skill (Kitkopika, github.com/Kitkopika/roblox-ui-skills) if installed.

## Asset export
- Transparent PNGs from Figma/Photoshop carry black RGB in clear pixels → dark halo when Roblox scales them. Alpha-bleed before upload (pixelfix, or fill clear pixels with the nearest edge colour). Keep ≤ 1024×1024.
- Design in Frames, not Rectangles (clipping, blend-mode shine, edge lighting compose cleanly).
- Tintable chrome (badges, gloss, 9-slices) export in white with ~4px padding, colour in Studio with `ImageColor3` / UIGradient.

## Native 2026 styling (use instead of old hacks)
- `UIShadow`: BlurRadius, Spread, Offset, negative ZIndex. Replaces 9-slice shadow PNGs. Text shadows follow the box, not the glyphs: use UIStroke for text.
- Per-corner `UICorner` (TopLeftRadius…). Don't mix with CornerRadius in one StyleSheet rule.
- `UIStroke.StrokeSizingMode = ScaledSize` (+ BorderOffset) so strokes don't go giant on phones.
- `UIGradient.Type` (Linear / Radial / Conical / Elliptical) + `TileMode` (Clamp / Repeat / Mirror). Watch dark ramps for banding.
- `StyleSheet` selectors: `::` targets modifiers (`.Card::UICorner`), `>` direct child, `>>` descendant (not CSS spaces). Built-ins like `@ViewportDisplaySizeSmall`, `@PreferredInput*`, `@ReducedMotionEnabledTrue` need no StyleQuery.

## Cartoon depth
Extrusion (black lip) + gradient face + gloss band + edge light + strong contrast. The flat version of the same card is the "before" picture in every guide.

## Studs
ImageLabel `ScaleType = Tile` + `TileSize` over the face, tinted by the face colour underneath, transparency ~0.4–0.6. On dark/deep colours one layer is too faint: stack two.

## Scroll grids that don't squish
Never mix UIGridLayout `Y.Scale` cells with `AutomaticCanvasSize = Y`. Either compute pixel `CellSize` from `AbsoluteWindowSize.X` (with `VerticalScrollBarInset = Always`), or Scale + `UIAspectRatioConstraint`. Check in Device Emulator. Scrollbar sits outside the cards.

## Type
FredokaOne has one weight (Regular). Need weights → BuilderSans / Nunito / Montserrat. Repeating labels: fixed TextSize + `TextTruncate.AtEnd`. Any `TextScaled` button gets a `UITextSizeConstraint`.
