# Path-Specific Rules

Rules in `.claude/rules/` are automatically enforced when editing files in matching paths:

| Rule File | Path Pattern | Enforces |
| ---- | ---- | ---- |
| `gameplay-code.md` | `Code/**` | Component patterns, data-driven values, delta time, no UI refs |
| `engine-code.md` | `Libraries/**` | No modifications to third-party code without approval |
| `ai-code.md` | `Code/AI/**` | Performance budgets, debuggability, data-driven params |
| `network-code.md` | `Code/Networking/**` | `[Sync]`/`[Broadcast]` patterns, server-authoritative |
| `ui-code.md` | `Code/UI/**` | Razor PanelComponent, no game state ownership, CSS compat |
| `editor-code.md` | `Editor/**` | Editor panel conventions, no game-runtime dependencies |
| `design-docs.md` | `design/gdd/**` | Required 8 sections, formula format, edge cases |
| `narrative.md` | `design/narrative/**` | Lore consistency, character voice, canon levels |
| `data-files.md` | `Assets/Data/**` | JSON validity, naming conventions, schema rules |
| `test-standards.md` | `tests/**` | Test naming, coverage requirements, fixture patterns |
| `prototype-code.md` | `prototypes/**` | Relaxed standards, README required, hypothesis documented |
| `shader-code.md` | `Assets/Shaders/**` | s&box shader conventions, performance targets |
