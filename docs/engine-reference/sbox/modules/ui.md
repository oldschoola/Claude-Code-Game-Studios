# s&box UI (Razor) Module

Last verified: 2026-03-27

## Key Types
- `PanelComponent` — Base class for all UI panels
- `ScreenPanel` — Full-screen 2D HUD overlay
- `WorldPanel` — 3D in-world UI panel

## Patterns

**Basic panel (.razor file)**:
```razor
@using Sandbox.UI;

<div class="my-panel">
    <span>@Label</span>
</div>

<style>
    .my-panel {
        color: white;
        font-family: Roboto;
    }
</style>

@code {
    public string Label { get; set; } = "Hello";
}
```

**Opening a panel from game code**:
```csharp
var panel = Hud.Current.RootPanel.AddChild<MyPanel>();
```

## CSS Compatibility

Many standard CSS properties do NOT work in s&box:

**Supported**: `display: flex` (with limits), `position`, `width`, `height`,
`color`, `background`, `font-family`, `font-size`, `padding`, `margin`,
`border`, `border-radius`, `opacity`, `text-align`, `overflow`

**NOT supported**: `word-wrap`, `overflow-wrap`, `border-style`, `display: grid`,
`@import`, degree-based gradients, `gap` (use margins instead)

Always test CSS in the s&box editor — failures are often silent.

## Notes
- UI must NEVER own game state — display only, dispatch events
- Use `Bind()` for reactive data binding
- All user-facing text must go through the localization system
