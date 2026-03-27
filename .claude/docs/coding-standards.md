# Coding Standards

## Code Style (enforced by s&box `.editorconfig`)

- **Tabs** (size 4) for C# and Razor, **spaces** (size 2) for JSON/XML
- **CRLF** line endings
- **File-scoped namespaces** preferred
- **No `var`** — use explicit types (`PlayerController pc` not `var pc`)
- **No `this.`** qualification on members
- **Expression-bodied** for properties/accessors, not methods/constructors
- **CA2000** (Dispose before losing scope) is an error
- **Static local functions** are a warning
- **PascalCase** for types, methods, properties; `_camelCase` for private fields; `s_camelCase` for private static fields

## s&box-Specific Patterns

- **Components**: Extend `Component` (not MonoBehaviour). Use `GetComponent<T>()`, `Components.Create<T>()`
- **GameObjects**: Use `Scene.CreateObject()` to spawn, `GameObject.Destroy()` to remove
- **Networking**: Use `[Sync]` attribute to replicate properties from server to clients
- **RPCs**: Use `[Broadcast]` attribute for multicast calls (server -> all clients)
- **Authority**: Check `IsProxy` to determine if running on the owning client or server
- **UI**: Always use Razor `PanelComponent` with HTML/CSS — not built-in UI widgets
- **Events**: Use `Sandbox.Event` system for inter-component communication
- **Game Resources**: Use `[GameResource]` for data definitions loaded from asset files
- **Localization**: Use s&box localization system, never hardcode player-facing strings
- **CSS in Razor UI**: Many standard CSS properties are unsupported (word-wrap, overflow-wrap, display:grid, @import). Check compatibility before using any CSS property.

## General Standards

- All game code must include doc comments on public APIs
- Every system must have a corresponding architecture decision record in `docs/architecture/`
- Gameplay values must be data-driven (external config), never hardcoded
- All public methods must be unit-testable (dependency injection over singletons)
- Commits must reference the relevant design document or task ID
- **Verification-driven development**: Write tests first when adding gameplay systems.
  For UI changes, verify with screenshots. Compare expected output to actual output
  before marking work complete. Every implementation should have a way to prove it works.

## Design Document Standards

- All design docs use Markdown
- Each mechanic has a dedicated document in `design/gdd/`
- Documents must include these 8 required sections:
  1. **Overview** -- one-paragraph summary
  2. **Player Fantasy** -- intended feeling and experience
  3. **Detailed Rules** -- unambiguous mechanics
  4. **Formulas** -- all math defined with variables
  5. **Edge Cases** -- unusual situations handled
  6. **Dependencies** -- other systems listed
  7. **Tuning Knobs** -- configurable values identified
  8. **Acceptance Criteria** -- testable success conditions
- Balance values must link to their source formula or rationale
