# s&box Rendering Module

Last verified: 2026-03-27

## Key Concepts
- Source 2 renderer (forward+ and deferred paths)
- Materials: `.vmat` files define material properties
- Textures: `.vtex` files (various formats: color, normal, roughness, etc.)
- Models: `.vmdl` files (compiled from import)

## Notes

Rendering in s&box is managed by the Source 2 native engine core. Game code
has limited direct control — most rendering configuration happens through
materials and the editor. C# code handles:

- Setting material properties on components
- Controlling visibility
- Particle system integration
- Post-processing effects (where exposed)

For rendering-specific questions, consult:
- `technical-artist` agent for material/VFX work
- `sbox-specialist` agent for API guidance
- `sbox-public/engine/Sandbox.Engine/` for source code
