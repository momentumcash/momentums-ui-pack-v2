# 3D menus and lobby scenes (Momentum UI Pack)

Written for the pack. Deep-dive: the `roblox-ui-3d-stage` skill (Kitkopika) if installed.

1. **Keep real UI input in 3D.** Use `Screen3D` or a camera-pinned `SurfaceGui` parented to **PlayerGui** with `Adornee = part`, never a SurfaceGui in Workspace. Then clicks, hover, scrolling and gamepad `SelectedObject` just work, no raycast hacks. `LightInfluence = 0`, Brightness ~0.85–1.0 so Bloom doesn't wash labels out; `CanQuery = true` only on the active panel part.
2. **No hidden WorldModels.** A WorldModel inside a ViewportFrame keeps stepping while invisible. Use flat PNGs for big grids; mount the ViewportFrame + WorldModel lazily for the active tab only; orbit the viewport camera instead of spinning rigged parts; set `CurrentCamera = nil` and destroy on tab exit / Play.
3. **Depth of field won't blur the UI** if `NearIntensity = 0` (or the panel distance sits inside the focus band). Snapshot existing Lighting effects and restore them on exit.
4. **Parallax: clamped, smoothed, touch-safe.** In PreRender: cursor offset from centre clamped to [-1, 1], exponential smoothing `1 - math.exp(-k * dt)`, bounded travel/angle, skip on Touch and ReducedMotion, guard against the CameraModule resetting CameraType.
5. **Lobby scene in, clean teardown out.** Clone `MenuScene` from ReplicatedStorage (StreamingEnabled-safe). Staged NPC: anchor only the HumanoidRootPart, limbs unanchored + Massless, `EvaluateStateMachine = false`. On Play: fade a veil in, unbind render steps, stop + destroy AnimationTracks, destroy scene/UI, restore CoreGui, Lighting, controls, `CameraType.Custom` + CameraSubject.
6. Screenshot caveat: Studio MCP `screen_capture` may not see SurfaceGuis. Capture the window directly if a shot comes back empty.
