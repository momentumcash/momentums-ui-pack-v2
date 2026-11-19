---
name: momentum-ui-pack
description: Momentum's UI Pack v2. One entry point for making Roblox game UI that looks front-page: HUDs, shops, inventories, egg hatch, daily rewards, settings, gamepass stores, popups, 3D/tilting menus and button juice. Picks the right build path (art-first PNG kit, headless JSON spec → PNG + Rojo + Luau, or 3D stage), applies one merged rulebook (depth stack, black lips, phone sizes, anti-slop, motion, audit) and checks the result before showing it. Use whenever someone wants Roblox UI/GUI made, restyled, animated, fixed or audited, or says their UI "looks basic / cheap / AI-made / breaks on mobile". Run /momentum-ui-pack.
argument-hint: "[help | hud | shop | inventory | hatch | daily | settings | gamepass | popup | 3d | juice | fix | audit]"
---

# Momentum's UI Pack v2

By @momentumcash. Credits in `CREDITS.md`.

One rule above all: **a Roblox UI is judged in one second by a kid on a phone.** Every choice below exists to pass that second.

## 0. Before anything

0. **Start from the kits (`kits/README.md`).** HUD Kit, Merchant GUI, Shop Index UI, and Nametag Kit are the default look: clone the closest window or button and restyle it. Plain-Frame builds read as cheap next to them. Also read `LOCAL.md` if it exists (this machine's own extras).
1. **Get a reference.** A screenshot the user sends beats any style word. Copy its layout and proportions, swap only the labels. No screenshot? Find 2–3 real front-page game UIs in the same genre and name what makes them work before building.
2. **Inspect what exists.** In an existing game, read the current ScreenGuis and scripts first. Keep every working button, RemoteEvent, instance name and script reference (`rules/izzy-debugging.md`). Never make an existing UI worse.
3. **Never invent content.** Pet names, prices, product ids, timers come from the user. Missing → a visible holder (`<unbound>`, `PRICE`), never a made-up value.
4. **Roblox Studio:** use the ONE open Studio window (check `list_roblox_studios`, never open a second). Never publish, never spend Robux, never upload without saying so.

## 1. Pick the path

| Job | Path | Why |
|---|---|---|
| Any screen the kits already have (HUD, Shop, Daily, Rewards, Codes, Gift, Lucky Wheel, Settings, Index, Merchant, nametags) | **Kits** (`kits/`) | Clone + restyle. Fastest and best-looking |
| Shop, inventory, egg hatch, gamepass store, daily rewards the kits don't have | **A. Art kit**, via the `cryptics-gui` skill if installed | The look comes from PNGs (studs, glare, gem bevels). Plain frames can't do that |
| Layout checks and quick wireframes only (never the final look) | **B. Headless spec** (`headless/`) | One JSON → PNG mockup + `.model.json` + Luau builder. The preview can't drift from what ships |
| Tilting glass menus, camera-pinned panels, lobby scenes, 3D item previews | **C. 3D stage** (`rules/3d-menus.md`) | Keeps clicks + gamepad working in 3D |
| Make an existing UI feel alive | **Juice** (`rules/juice.md`) | Hover/press/open motion without layout jitter |
| "It looks off" / breaks on phone | **Fix + audit** (`rules/izzy-debugging.md` + section 4) | Root causes, not offset nudges |

Paths mix: build the shop with A, the HUD with B, then add juice to both.

### A. Art kit (front-page look)
- If the `cryptics-gui` skill is installed, run it (`/cryptics-gui shop`, `hud`, `hatch`…) and follow it. It handles style pick, grey-box, HTML/Figma board, PNG export, Studio import, UIKit wiring, purchases and AuditUI.
- Not installed: tell the user the free official download is from Crypticplay_RBLX (YouTube / github.com/Crypticplayz/cryptics-gui releases). Until then, build path B and layer art on top.
- **Illustrative art (icons, mode cards, banners, logos) comes from image generation, not drawn shapes.** Glossy 3D, thick outline, transparent background, NO text in the art. Put the label on top as a real TextLabel. Make 3, pick the boldest.
- Alpha-bleed every transparent PNG before upload (Figma/Photoshop write black into clear pixels, which shows as a dark halo). Max 1024×1024. Upload as **Image**, never Decal.

### B. Headless spec
```bash
cd headless
python3 scripts/ui_build.py examples/coin_hud.json --out build     # PNG + .model.json + .lua
python3 scripts/verify_ui.py examples/coin_hud.json                # overlap, gaps, centring, text fit, tap size at 4 screens
python3 scripts/verify_labels.py --ui <ui dir> --client <client src>   # every <unbound> label must be wired by code
```
- Write a small Python generator with `scripts/ui_kit.py` (pill, window, shop_tile, slot, bar, toggle, slider) instead of hand-editing JSON.
- Restyle by copying `references/theme.json`. Numbers in `references/ui-recipes.md`: read it before inventing a value.
- Into Studio: paste the `.lua` into `execute_luau`, screenshot, then clear what you injected unless the user wants it kept.
- Fix against `verify_ui.py`'s text output; look at the PNG once at the end.
- `glyphs.py` / `icon_cutout.py` need `numpy scipy opencv-python` (optional).

### C. 3D stage
Read `rules/3d-menus.md`. If the `roblox-ui-3d-stage` skill is installed, read it too.

## 2. The look: house rules (all paths)

**Depth + identity.** Every panel/button = black 3D lip under it + face with a top-lit gradient + one black outline the same thickness everywhere on screen. Mixed stroke weights are the clearest sign of beginner or AI-made UI. **The lip is always black (#000), never a darker shade of the face.**

**Simulator grammar (kids read it instantly).**
- FredokaOne, Mixed Case (never all caps for HUD), white text + thick black stroke. Small dark body text: stroke 0.
- Picture-first tiles, 1–2 word labels, hints of 2–3 words max. No sentences on screen.
- Green price buttons with the currency icon on them. Red header slab + X top-right on shop windows. Rarity colours fixed: Common grey, Uncommon green, Rare blue, Epic purple, Legendary gold, Mythic red, Secret rainbow. Always show the rarity word too.
- **One button per job.** No Shop tile *and* Shop tab, no duplicate "+". Big action buttons only on touch; PC gets click + a short hint.
- Icons overflow their header/tile a little. Badges and a "!" bubble on the HUD button that opens each menu.
- Stud texture (Roblox brick look): a tiled ImageLabel over the face, Overlay feel, transparency 0.4–0.6. Use a real stud texture, never a homemade one.

**Genre exceptions.** Sports/competitive games: broadcast-style score bug, team colours, compact, no candy buttons (`rules/izzy-style-presets.md`). The HUD should belong to the world it sits on.

**Layout.**
- One hero per window, 8-pt spacing, one primary (brightest) action per window.
- Nothing touches (≥8px between siblings), nothing is cut off, children keep ≥16px from their parent's inner edge measured from the lip/outline, decorations (rays, halftone) stay clipped inside their card. Only intentional pop-outs (header icon, badges, close X) cross an edge.
- "Centred" is measured: a 0.5 scale position needs a 0.5 AnchorPoint.
- No box drawn around a control. Position-only frames are transparent.
- Grids: inventory/index = 5 columns, shop cards = 4 on desktop. `UIAspectRatioConstraint` in every grid cell.

**Anti-slop.** Before showing, run `rules/izzy-anti-slop.md`. Two or more of these = redo: same-size cards everywhere, glow/glare on everything, purple-blue gradients by default, unicode symbols as icons, emojis as icons, invented values, everything straight and centred, a UI you could paste into ten unrelated games unchanged.

## 3. Engineering rules (all paths)

- Scale for structure, Offset only for small details. `UIStroke` scales with `StrokeSizingMode = ScaledSize` (or scale strokes by window width in the controller).
- Phone first: test 640×360, 844×390, 1024×768, 1920×1080 (Studio Device Emulator). Tap targets ≥ 44pt (≈ 88px at 1280×720). Keep the thumbstick (bottom-left), jump (bottom-right) and the top bar clear. Hotbars go bottom-centre.
- Changing text (cash, names, prices, timers) is always a real TextLabel, never baked into an image. Format numbers (1.2K, 3.4M) so they never overflow. Test the longest real name.
- `TextScaled` shrinks without limit: pair it with `UITextSizeConstraint`. Text must stay legible on the smallest screen; widen the box or shorten the text instead of shrinking it.
- One transparent root frame per page so a script toggles one `.Visible`.
- `ClipsDescendants` doesn't clip rotated children: draw angles as a UIGradient transparency step on an unrotated element.
- Gamepad: every button Selectable, focus lands in the open window, B closes it. Respect `GuiService.ReducedMotionEnabled`.
- Buy buttons really buy, checked on the server (ProcessReceipt / gamepass ownership). Ask for every product and pass id; never invent one.
- No per-frame loops for static UI; disconnect what you connect; no BlurEffect stacks.

## 4. Before calling it done

1. Screenshot at phone size and desktop. Zoom-check edges, clipping, overlaps, lips, padding, text fit.
2. Path B: `verify_ui.py` 0 failures, `verify_labels.py` passes. Path A: AuditUI 0 errors.
3. Side by side with the reference. If it doesn't hold up, it isn't done.
4. Every button clicked once for real (open, close, tab, buy) in a playtest; check the console.
5. Remind the user to save the place.

## Files

- `rules/izzy-*.md`: design, responsive, motion, debugging, anti-slop, style presets (MIT, Izzy)
- `rules/style-craft.md`: 2026 styling APIs, asset export, studs, scroll grids
- `rules/juice.md`: hover/press/open motion that never jitters a layout
- `rules/3d-menus.md`: Screen3D / camera-pinned SurfaceGui menus, parallax, lobby scenes, viewport previews
- `kits/`: HUD Kit, Merchant GUI, Shop Index UI, Nametag Kit: the default look
- `headless/`: JSON spec → PNG + Rojo + Luau builder and checks (SyphoDev, free licence, see `headless/LICENSE-SyphoDev.txt`)
- `CREDITS.md`, `LICENSE.md`
