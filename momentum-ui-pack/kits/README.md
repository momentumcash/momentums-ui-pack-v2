# Kits: the default look

Free Roblox UI kits, checked safe (no web requests, no remote code, no obfuscation; purchase prompts only on Buy clicks with your own ids). These are what every screen starts from.

| File | What's in it |
|---|---|
| `HUD Kit.rbxm` | Full simulator HUD in one file: cash counter, left menu (Shop, Daily, Rewards, Codes, Gift, Lucky Wheel), right offers, Daily / Rewards / Codes / Gift / Wheel / Shop / Settings windows, saving + server rewards + codes + gifting. Drop in StarterGui, it installs itself. Put your product/pass ids in HUDConfig |
| `Merchant GUI.rbxm` | Traveling merchant window: deal cards, timer, crate spin. No scripts |
| `Shop Index UI.rbxm` | Two juicy HUD buttons (Shop, Index): hover bounce + lean, click particles, falling icons, animated gradient outline, sounds. All tunable through attributes on each script |
| `Nametag Kit.rbxm` | Overhead tags (role icon + name, country flag, live mic bubble) + Roles panel (press H) to give roles and make tags. Install steps in its READ ME script. Needs Game Settings → Security → Studio Access to API Services ON |

**Use:** in Studio, right-click StarterGui → Insert from File and pick the `.rbxm`. Then clone the closest window/button and restyle it. Never rebuild these looks from plain Frames.
