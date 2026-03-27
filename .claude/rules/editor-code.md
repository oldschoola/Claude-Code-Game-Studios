---
paths:
  - "Editor/**"
---

# Editor Code Rules

- Editor panels must NOT depend on game-runtime code — no `Component`, `GameObject`, or game state references
- Editor code runs in a separate context from the game — keep dependencies isolated
- Use s&box editor APIs for panel registration, tool creation, and asset management
- All editor tools must provide undo/redo support where applicable
- Editor panels should be responsive — never block the editor UI thread with heavy computation
- Document editor tool usage in tooltips or inline help
- Custom editors must gracefully handle missing or invalid asset references
