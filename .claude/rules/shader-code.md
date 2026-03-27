---
paths:
  - "Assets/Shaders/**"
---

# Shader Code Standards

All shader files in `Assets/Shaders/` must follow these standards to maintain
visual quality and performance in the s&box Source 2 renderer.

## Naming Conventions
- File naming: `[category]_[name]` — descriptive and purposeful
- Use descriptive names that indicate the material purpose

## Code Quality
- All parameters must have descriptive names and appropriate hints
- Group related parameters
- Comment non-obvious calculations (especially math-heavy sections)
- No magic numbers — use named constants or documented uniform values
- Include authorship and purpose comment at the top of each shader file

## Performance Requirements
- Document the target complexity budget for each shader
- Minimize texture samples in fragment shaders
- Avoid dynamic branching in fragment shaders — use `step()`, `mix()`, `smoothstep()`
- No texture reads inside loops
- Profile shader performance in the s&box editor before committing

## s&box-Specific Notes
- s&box uses Source 2's material system (`.vmat` files)
- Shaders are compiled through the s&box editor, not at runtime
- Test shaders in the s&box editor viewport at multiple LOD distances
- Check shader compilation warnings in the console output
