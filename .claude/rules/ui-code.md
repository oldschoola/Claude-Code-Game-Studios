---
paths:
  - "Code/UI/**"
---

# UI Code Rules

- UI must NEVER own or directly modify game state — display only, use events to request changes
- All UI text must go through the localization system — no hardcoded user-facing strings
- Support both keyboard/mouse AND gamepad input for all interactive elements
- All animations must be skippable and respect user motion/accessibility preferences
- UI sounds trigger through the audio event system, not directly
- UI must never block the game thread
- Scalable text and colorblind modes are mandatory, not optional
- Test all screens at minimum and maximum supported resolutions

## s&box Razor UI Rules

- Always use `PanelComponent` with Razor (.razor) files for game UI — never use built-in UI widgets directly
- Use `ScreenPanel` for 2D HUD overlays, `WorldPanel` for 3D in-world UI
- Check CSS compatibility before using any property — many standard CSS properties do NOT work in s&box:
  - UNSUPPORTED: `word-wrap`, `overflow-wrap`, `border-style`, `display:grid`, `@import`
  - Check `get_css_reference` tool for full compatibility list
- Style Razor components using the `<style>` block within .razor files
- Use `Bind()` for data binding between game state and UI elements
- Never put game logic inside Razor components — components should only display and dispatch events
