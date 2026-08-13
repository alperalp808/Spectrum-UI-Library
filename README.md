# Spectrum UI Library — v3.9.5

A modern, fully-featured GUI library for Roblox, designed for clean dashboards with rich theming, animated components, and a built-in control panel.

---

## Table of Contents

- [Quick Start](#quick-start)
- [Window Configuration](#window-configuration)
- [Key System (Authentication)](#key-system-authentication)
- [Themes](#themes)
- [Tabs & Sections](#tabs--sections)
- [Components](#components)
  - [Button](#button)
  - [Toggle](#toggle)
  - [Slider](#slider)
  - [Dropdown](#dropdown)
  - [Textbox](#textbox)
  - [Colorpicker](#colorpicker)
  - [Keybind](#keybind)
  - [Label](#label)
- [Notifications](#notifications)
- [Window Methods](#window-methods)
- [Dashboard / Control Panel](#dashboard--control-panel)
- [Two-Column Layout](#two-column-layout)
- [Notes & Tips](#notes--tips)

---

## Quick Start

```lua
local Spectrum = loadstring(game:HttpGet("YOUR_RAW_SCRIPT_URL"))()

local Window = Spectrum:AddWindow({
    Name            = "My Script",
    Theme           = "Aqua",
    ToggleKeybind   = Enum.KeyCode.RightControl,
    Size            = UDim2.new(0, 760, 0, 540),
    Notifications   = true,
    ToggleButton    = true,
    ControlPanel    = true,
})

local Tab     = Window:AddTab("Main")
local Section = Tab:AddSection("General")

Section:AddButton({
    Name     = "Say Hello",
    Callback = function()
        print("Hello, World!")
    end,
})
```

---

## Window Configuration

Pass a config table to `Spectrum:AddWindow()`:

| Key | Type | Default | Description |
|---|---|---|---|
| `Name` | string | `"Spectrum"` | Title shown in the top bar |
| `Theme` | string | `"Aqua"` | Starting theme (see [Themes](#themes)) |
| `ToggleKeybind` | KeyCode | `RightControl` | Keyboard shortcut to show/hide |
| `Size` | UDim2 | `760 × 540` | Window dimensions |
| `Notifications` | bool | `true` | Enable/disable toast notifications globally |
| `ToggleButton` | bool | `true` | Show the floating toggle button |
| `ControlPanel` | bool | `true` | Show the built-in Dashboard tab |
| `KeySystem` | table | `{Enabled=false}` | Authentication config (see below) |

---

## Key System (Authentication)

Lock your script behind a username/password login screen.

```lua
local Window = Spectrum:AddWindow({
    Name  = "My Script",
    KeySystem = {
        Enabled = true,
        Title   = "LOGIN REQUIRED",
        Note    = "Enter your credentials to continue.",
        Users   = {
            { User = "admin",   Pass = "secret123" },
            { User = "player1", Pass = "mypassword" },
        },
    },
})
```

When enabled, a centered login modal appears before the main window. On success the window animates open; on failure the password field flashes red.

---

## Themes

Seven built-in themes are available. Pass the name string to `Theme` on window creation, or call `Window:SetTheme()` at any time.

| Name | Accent Color | Mood |
|---|---|---|
| `Aqua` | Cyan `#00CCFF` | Default — cool blue-teal |
| `Green` | Emerald `#2ECC71` | Nature / hacker |
| `Dark` | Light grey `#C8C8C8` | Minimal monochrome |
| `Foggy` | Blue `#3898FF` | Soft blue-grey |
| `Sun` | Amber `#F39C12` | Warm golden |
| `Crimson` | Red `#C42238` | Bold red accent |
| `Light` | Royal blue `#4169E1` | Light / bright |

```lua
-- Switch theme at runtime
Window:SetTheme("Crimson")
```

---

## Tabs & Sections

### Adding a Tab

```lua
local Tab = Window:AddTab("Combat")
```

Tabs appear in the left sidebar. The first tab added is automatically selected.

### Adding a Section

```lua
local Section = Tab:AddSection("Aim Settings")
```

Sections are collapsible groups inside a tab. All components are added to sections.

---

## Components

Every component method returns an API object with at least `:Get()` and `:Set()` methods, plus an `.Instance` reference to the underlying UI frame.

### Button

A clickable button that fires a callback.

```lua
Section:AddButton({
    Name          = "Teleport to Spawn",
    Notifications = true,       -- optional: show a toast on click
    Callback      = function()
        -- your code here
    end,
})
```

**Keybind attachment** — bind a key to trigger the button:

```lua
local Btn = Section:AddButton({ Name = "Noclip", Callback = function() end })
Btn:AddKeybind("F")    -- press F to fire the button
```

---

### Toggle

An on/off switch.

```lua
local Toggle = Section:AddToggle({
    Name          = "Silent Aim",
    Default       = false,
    Notifications = true,
    Callback      = function(state)
        print("Toggle is now:", state)
    end,
})
```

**API:**

```lua
Toggle:Set(true)   -- force a value
Toggle:Get()       -- returns current boolean
Toggle:AddKeybind("G")  -- press G to flip the toggle
```

---

### Slider

A draggable numeric slider.

```lua
local Slider = Section:AddSlider({
    Name          = "Walk Speed",
    Min           = 16,
    Max           = 500,
    Default       = 16,
    Increment     = 1,
    Notifications = false,
    Callback      = function(value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
    end,
})
```

**API:**

```lua
Slider:Set(100)   -- jump to a specific value
Slider:Get()      -- returns current number
```

---

### Dropdown

A single-select or multi-select dropdown menu.

```lua
-- Single select
local Drop = Section:AddDropdown({
    Name     = "Game Mode",
    Options  = { "Normal", "Hardcore", "Creative" },
    Default  = "Normal",
    Callback = function(selected)
        print("Chose:", selected)
    end,
})

-- Multi-select
local MultiDrop = Section:AddDropdown({
    Name     = "Active Cheats",
    Options  = { "ESP", "Aimbot", "Fly", "Speed" },
    Default  = { "ESP" },   -- pre-selected items
    Multi    = true,
    Callback = function(selected)
        -- selected is a table: { ESP = true, Fly = true, ... }
    end,
})
```

**API:**

```lua
Drop:Set("Hardcore")           -- change selection
Drop:Get()                     -- returns current value / table
Drop:Refresh({ "A", "B" })    -- replace the options list
```

---

### Textbox

A text input field that fires on Enter.

```lua
local Box = Section:AddTextbox({
    Name            = "Target Player",
    PlaceholderText = "Enter username...",
    Default         = "",
    Notifications   = false,
    Callback        = function(text)
        print("Submitted:", text)
    end,
})
```

**API:**

```lua
Box:Set("Roblox")   -- set content programmatically
Box:Get()           -- returns current string
```

---

### Colorpicker

An HSV color picker with a hue bar.

```lua
local CP = Section:AddColorpicker({
    Name     = "ESP Color",
    Default  = Color3.fromRGB(255, 0, 0),
    Callback = function(color)
        print("New color:", color)
    end,
})
```

Clicking the color swatch opens the picker panel. Drag the large square to change saturation/brightness, drag the bottom bar to change hue.

**API:**

```lua
CP:Set(Color3.fromRGB(0, 255, 128))
CP:Get()   -- returns Color3
```

---

### Keybind

A standalone keybind element (not attached to another component).

```lua
local KB = Section:AddKeybind({
    Name     = "Fly Toggle",
    Default  = Enum.KeyCode.F,
    Mode     = "Toggle",   -- "Toggle" or "Button"
    Callback = function(state)
        -- state is a bool when Mode = "Toggle", nil when "Button"
    end,
})
```

Click the key display to rebind. Press the assigned key to fire the callback.

**API:**

```lua
KB:Set(Enum.KeyCode.H)
KB:Get()   -- returns current KeyCode
```

---

### Label

A read-only text display, useful for live stats.

```lua
local Lbl = Section:AddLabel("Status: Idle")
-- or
local Lbl = Section:AddLabel({ Text = "Status: Idle" })
```

**API:**

```lua
Lbl:Set("Status: Running")   -- update text at any time
```

---

## Notifications

Toast notifications slide in from the right side of the screen.

```lua
Window:Notify({
    Title    = "Success",
    Content  = "Operation completed successfully.",
    Duration = 3,   -- seconds before auto-dismiss
})
```

Global notifications (triggered automatically by components) can be disabled per-component by setting `Notifications = false`, or globally with `Notifications = false` in the window config.

---

## Window Methods

| Method | Description |
|---|---|
| `Window:Show()` | Open / reveal the window |
| `Window:Close()` | Hide the window with animation |
| `Window:Toggle()` | Flip between open and closed |
| `Window:Destroy()` | Fully remove the GUI from the screen |
| `Window:SetTheme(name)` | Switch to a different theme at runtime |
| `Window:SetToggleKeybind(KeyCode)` | Change the keyboard shortcut |
| `Window:Notify({...})` | Fire a toast notification |

---

## Dashboard / Control Panel

When `ControlPanel = true` (default), a **Dashboard** tab is automatically prepended with the following live panels:

**Left Column**

- **Profile & Account Summary** — avatar thumbnail, display name, username, account age badge, Premium status, Client ID copy button.
- **Theme Management** — dropdown to switch themes in real time.
- **Server & Game Info** — game name, Place ID copy, Job ID (server instance) copy.

**Right Column**

- **Live Performance Monitor** — FPS, ping (ms), RAM usage (MB), and data receive rate (KB/s), updated every 0.5 s.
- **Character Physics Monitor** — live Position, CFrame, and Velocity with copy-to-clipboard buttons.

**Full-width**

- **Player List** — lists every other player with:
  - Avatar thumbnail
  - Display name and username
  - **Teleport** button — moves your character next to them instantly
  - **View toggle** — redirects the camera to follow that player; toggle off to return to yourself

Set `ControlPanel = false` to hide this entire tab if you want a clean slate.

---

## Two-Column Layout

Tabs can be split into a left and right column for more compact layouts.

```lua
local Tab = Window:AddTab("Settings", true)  -- second arg = twoColumnMode

local LeftSec  = Tab:AddSection("Left Panel",  "Left")
local RightSec = Tab:AddSection("Right Panel", "Right")
-- Default (no alignment arg) spans full width
local FullSec  = Tab:AddSection("Full Width")
```

---

## Notes & Tips

- **Dragging** — the window is draggable by its top bar. The floating toggle button is also independently draggable.
- **Minimize** — the `–` button in the top bar collapses the window to just the title bar height; click again to restore.
- **Close** — the `X` button calls `Window:Destroy()` which removes the GUI entirely. Re-running the script creates a new instance.
- **Theme registry** — all themed elements (colors, strokes) update automatically when `SetTheme` is called; no manual refresh needed.
- **Exploit compatibility** — the library checks for `gethui`, `CoreGui`, and `PlayerGui` in that order for its parent, and calls `syn.protect_gui` when available.
- **Clipboard** — copy functions use `setclipboard` or `toclipboard` depending on your executor.
- **Sound IDs** — all sounds use Roblox asset IDs and play through `SoundService`; they auto-clean up via `Debris`.

---

> **Spectrum UI Library** is designed for use inside Roblox exploit executors. Always ensure your usage complies with the terms of service of the platform and any applicable rules of the experience you are playing.
