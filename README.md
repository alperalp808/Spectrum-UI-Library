# Spectrum UI v3.7.0 — Test Panel

A comprehensive usage guide and feature overview for the Spectrum UI test script, showcasing the library's **Two-Column layout system**, standard single-column tabs, and the full set of available UI components.

---

## 📖 Overview

This script demonstrates every major feature of the Spectrum UI library, including:

- ✅ Two-column tabs (Left/Right sections)
- ✅ Standard single-column tabs
- ✅ All UI elements (Button, Toggle, Slider, Dropdown, Textbox, Colorpicker, Keybind)
- ✅ Theme system (7 built-in themes)
- ✅ Notification system
- ✅ Optional Key System (login gate)

---

## 🚀 Getting Started

### Loading the Library

The script attempts to load Spectrum UI from a remote source, falling back to a local file if the online source is unavailable:

```lua
local Spectrum = loadstring(game:HttpGet("<library URL>"))()

if not Spectrum then
    Spectrum = loadstring(readfile("spectrum_updated.lua"))()
end
```

### Creating the Main Window

```lua
local Window = Spectrum:AddWindow({
    Name = "Spectrum UI v3.7.0 | Test Panel",
    Theme = "Aqua",
    ToggleKeybind = Enum.KeyCode.RightControl,
    Size = UDim2.new(0, 840, 0, 520),
    Notifications = true,
    ControlPanel = true,
    KeySystem = { Enabled = false, ... }
})
```

| Parameter | Description |
|---|---|
| `Name` | Window title text |
| `Theme` | One of `Green`, `Dark`, `Foggy`, `Sun`, `Aqua`, `Crimson`, `Light` |
| `ToggleKeybind` | Key used to show/hide the whole window |
| `Size` | Window dimensions (`UDim2`) |
| `Notifications` | Enables the global notification system |
| `ControlPanel` | Enables the built-in control panel |
| `KeySystem` | Optional login/authentication gate before the UI loads |

---

## 🔑 Key System (Optional)

The Key System adds a login screen requiring a username/password before the window becomes accessible. Useful for gating access to the panel.

```lua
KeySystem = {
    Enabled = true,
    Title = "Spectrum Security",
    Note = "Please enter your credentials.",
    Users = {
        { User = "admin", Pass = "1234" }
    }
}
```

Set `Enabled = false` during testing/development.

---

## 🗂️ Tabs

Tabs are created with `Window:AddTab(name, twoColumn)`. The second argument determines the layout:

```lua
local TwoColumnTab   = Window:AddTab("📊 Two-Column Mode", true)
local SingleColumnTab = Window:AddTab("🎛️ Single-Column Mode", false)
```

- `true` → enables the **two-column** layout (Left/Right sections)
- `false` → standard **single-column** layout

This test panel defines four tabs:

| Tab | Layout | Purpose |
|---|---|---|
| 📊 Two-Column Mode | Two-column | Demonstrates Left/Right sections |
| 🎛️ Single-Column Mode | Single-column | Standard sequential controls |
| ⚙️ Settings | Single-column | Theme selector + keybind management |
| 🚀 Advanced | Single-column | System info, notification demos, live demo |

---

## 📊 Two-Column Layout

When a tab is created with `true`, use the `"Left"` and `"Right"` identifiers to split content into two side-by-side sections:

```lua
local LeftSection  = TwoColumnTab:AddSection("⬅️ Left Section", "Left")
local RightSection = TwoColumnTab:AddSection("➡️ Right Section", "Right")
```

**Left Section** contains: a label, two buttons, a toggle, a slider, and a textbox.
**Right Section** contains: a label, a colorpicker, a single-select dropdown, a multi-select dropdown, and a toggle.

---

## 🎛️ UI Components Reference

### Label
```lua
Section:AddLabel({ Text = "Some descriptive text" })
```

### Button
```lua
Section:AddButton({
    Name = "Button Name",
    Notifications = true,
    Callback = function() end
})
```

### Toggle
```lua
Section:AddToggle({
    Name = "Toggle Name",
    Default = false,
    Notifications = true,
    Callback = function(state) end
})
```

### Slider
```lua
Section:AddSlider({
    Name = "Slider Name",
    Min = 0,
    Max = 100,
    Default = 50,
    Increment = 5,
    Callback = function(value) end
})
```

### Textbox
```lua
Section:AddTextbox({
    Name = "Textbox Name",
    PlaceholderText = "Enter text...",
    Default = "",
    Callback = function(text) end
})
```

### Colorpicker
```lua
Section:AddColorpicker({
    Name = "Color Picker",
    Default = Color3.fromRGB(0, 204, 255),
    Callback = function(color) end
})
```

### Dropdown (single-select)
```lua
Section:AddDropdown({
    Name = "Dropdown Name",
    Options = {"Option A", "Option B"},
    Default = "Option A",
    Callback = function(selected) end
})
```

### Dropdown (multi-select)
```lua
Section:AddDropdown({
    Name = "Multi Dropdown",
    Options = {"1", "2", "3", "4"},
    Default = {"1", "3"},
    Multi = true,
    Callback = function(selected) end
})
```

### Keybind (button/toggle shortcut)
```lua
local MyButton = Section:AddButton({ Name = "My Button", Callback = function() end })
MyButton:AddKeybind("F")
```

### Keybind (standalone)
```lua
Section:AddKeybind({
    Name = "Custom Keybind",
    Default = Enum.KeyCode.H,
    Mode = "Toggle", -- or "Hold"
    Callback = function(state) end
})
```

Every component accepts an optional `Notifications = true` flag to automatically fire a notification when its callback runs.

---

## 🔔 Notifications

Trigger a notification anywhere in the script:

```lua
Window:Notify({
    Title = "Title Text",
    Content = "Body text of the notification.",
    Duration = 3 -- seconds
})
```

Global notifications can be toggled at runtime:

```lua
Window.GlobalNotifications = false
```

---

## 🎨 Theming

Switch themes at runtime with:

```lua
Window:SetTheme("Dark")
```

Available themes: `Green`, `Dark`, `Foggy`, `Sun`, `Aqua`, `Crimson`, `Light`

The Settings tab includes a dropdown that lets the user pick a theme interactively, calling `Window:SetTheme(theme)` on selection.

---

## ⌨️ Default Test Keybinds

| Key | Action |
|---|---|
| `RightControl` | Show / hide the window |
| `F` | Triggers "Shortcut Button" |
| `G` | Triggers "Shortcut Toggle" |
| `H` | Triggers "Custom Keybind" (Toggle mode) |

---

## 🗒️ Tab & Section Summary

```
├─ Two-Column Tab  : 2 Sections (Left + Right)
├─ Single-Column Tab: 1 Section
├─ Settings Tab     : Theme & Keybind management
├─ Advanced Tab     : Info & Live Demo
└─ Dashboard Tab    : (Automatic)
```

---

## 📌 Notes on Two-Column Mode

- Pass `true` as the second argument of `Window:AddTab()` to enable two-column layout.
- Use `"Left"` / `"Right"` as the second argument of `Tab:AddSection()` to place a section on a given side.
- Pass `false` (or omit the argument) for a standard single-column layout.

---

## ✅ Quick Start Checklist

1. Load the Spectrum library (remote or local fallback).
2. Create the window with `Spectrum:AddWindow({...})`.
3. Add tabs with `Window:AddTab(name, isTwoColumn)`.
4. Add sections with `Tab:AddSection(name, side?)`.
5. Populate sections with components (`AddButton`, `AddToggle`, `AddSlider`, etc.).
6. Optionally wire up keybinds with `:AddKeybind()`.
7. Press the configured `ToggleKeybind` to open the panel.
