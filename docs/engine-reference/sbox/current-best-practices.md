# s&box Current Best Practices

Last verified: 2026-03-27

## Component Architecture

- Use `Component` as the base class for all game behaviors
- Attach components to `GameObject`s via `Components.Create<T>()`
- Use `Simulate()` for per-frame updates (equivalent of Update/Tick)
- Use `OnEnabled()` / `OnDisabled()` for lifecycle management

## Networking

- Always check `IsProxy` before modifying gameplay-critical state
- Use `[Sync]` only for properties clients need to observe
- Use `[Broadcast]` for server-to-all-clients notifications
- Use Network Storage v3 (sbox.cool) for persistent multiplayer data
- Never use local file saves for multiplayer game state

## UI (Razor)

- Always use `PanelComponent` with .razor files — never built-in UI widgets
- `ScreenPanel` for 2D HUD, `WorldPanel` for 3D in-world UI
- UI must never own or modify game state — display only, dispatch events
- Check CSS compatibility before using properties — many are unsupported
- Use `Bind()` for reactive data binding

## Code Style

- No `var` — explicit types only
- No `this.` qualification
- File-scoped namespaces
- Expression-bodied properties: yes; methods/constructors: no
- Tabs (size 4) for C#, spaces (size 2) for JSON/XML
- CRLF line endings

## Asset Pipeline

- Use s&box editor for asset creation and import
- Models: `.vmdl`, Materials: `.vmat`, Textures: `.vtex`, Sounds: `.vsnd`
- Assets are compiled during the build process
- Use `GameResource<T>` for data definitions loaded from assets
