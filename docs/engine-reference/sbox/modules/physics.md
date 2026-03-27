# s&box Physics Module

Last verified: 2026-03-27

## Key Types
- Physics uses Source 2's built-in physics (Bullet-based, VPhysics)
- Collision handled via collider components on GameObjects
- Raycasts, sweeps, and overlap queries available

## Notes

Physics in s&box is managed by the Source 2 engine core. Game code interacts
with it through the C# Component system. Specific API details should be
verified against the `sbox-public/` source code.

## Common Operations

Physics operations are typically performed through Components attached to
GameObjects. Check `sbox-public/engine/Sandbox.Engine/` for the latest
physics API surface.
