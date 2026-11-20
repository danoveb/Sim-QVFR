# RF2 OpenVR/OpenComposite chain

RF2 and ACC are both registered as OpenVR applications with a game-local `openvr_api.dll`.
OpenComposite translates their OpenVR calls into the OpenXR API layer.

The first shared implementation step is `games/openvr_bridge.h`:

- accepts the two `XrView` values observed by the layer;
- converts their asymmetric FOV angles to tangents;
- publishes them through a small seqlock because the layer and D3D11 renderer may use different threads;
- is reset with the OpenXR session;
- contains no game-renderer assumptions.

RF2's adapter snapshots this data and feeds its verified `focus_projection()` / `clip_remap()` path.
The RF2 D3D11 backend remains separate because OpenVR submission happens after RF2's FP16 MSAA
scene passes. A future submit correlation module should record eye, submitted D3D11 texture,
texture bounds and frame sequence, but must not replace the existing scene-pass/resolve hook.
