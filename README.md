# Spectrum UI Library — v3.7.0

**Two-Column Tab Support Edition**

An English usage guide and introduction to the Spectrum UI library — a Lua-based interface framework for building draggable, themeable windows with tabs, sections, and a full set of interactive controls.

---

## ✨ Highlights

- 🆕 **Two-column tab layout** — split a tab's content into Left/Right sections
- 🎨 **7 built-in themes** — `Green`, `Dark`, `Foggy`, `Sun`, `Aqua`, `Crimson`, `Light`, switchable live at runtime
- 🔔 **Built-in notification system** with slide-in/out animations
- 🔐 **Optional Key System** login screen (username/password gate)
- ⌨️ **Keybind support** on buttons, toggles, and standalone keybind elements
- 📊 **Optional Dashboard tab** showing FPS, ping, memory usage, network data, and player/place info
- 🖱️ Draggable window and floating toggle button
- 🎬 Smooth tweened animations throughout (open/close, hover, click bounce, popups)

---

## 📦 Installation

Spectrum UI is a single Lua module that returns a library table. Load it with `loadstring`, typically from a URL, and call `AddWindow` to get started:

```lua
local Spectrum = loadstring(game:HttpGet("<raw-source-url>"))()

local Window = Spectrum:AddWindow({
    Name = "My Panel",
    Theme = "Aqua",
})
```

---

## 🪟 Creating a Window

```lua
local Window = Spectrum:AddWindow({
    Name = "My Panel",              -- Window title
    Theme = "Aqua",                 -- Green | Dark | Foggy | Sun | Aqua | Crimson | Light
    ToggleKeybind = Enum.KeyCode.RightControl,
    Size = UDim2.new(0, 736, 0, 484),
    Notifications = true,           -- Global notification switch
    ControlPanel = true,            -- Adds a built-in "Dashboard" tab
    ToggleButton = true,            -- Floating show/hide button
    KeySystem = {
        Enabled = false,
        Title = "Spectrum Security",
        Note = "Please enter your credentials.",
        Users = { { User = "admin", Pass = "1234" } }
    }
})
```

### Window options

| Option | Type | Default | Description |
|---|---|---|---|
| `Name` | string | `"Spectrum"` | Text shown in the top bar |
| `Theme` | string | `"Aqua"` | Initial theme name |
| `ToggleKeybind` | `Enum.KeyCode` | `RightControl` | Key that shows/hides the window |
| `Size` | `UDim2` | `0,736,0,484` | Window size |
| `Notifications` | boolean | `true` | Master switch for `Window:Notify()` |
| `ControlPanel` | boolean | `true` | Adds an automatic Dashboard tab |
| `ToggleButton` | boolean | `true` | Shows a small draggable floating toggle button |
| `KeySystem` | table | `{Enabled=false}` | Optional login gate, see below |

### Window methods

| Method | Description |
|---|---|
| `Window:Show()` | Opens the window with a spring animation |
| `Window:Close()` | Closes (hides) the window |
| `Window:Toggle()` | Shows if hidden, hides if shown |
| `Window:Destroy()` | Fully removes the UI from the game |
| `Window:SetTheme(name)` | Switches every themed element live |
| `Window:SetToggleKeybind(keyCode)` | Changes the show/hide hotkey |
| `Window:Notify({Title, Content, Duration})` | Fires a toast notification |
| `Window:AddTab(name, twoColumn)` | Creates a new tab, returns a `Tab` object |

---

## 🔐 Key System (Login Gate)

When `KeySystem.Enabled = true`, a centered login card appears before the main window, requiring a matching username/password pair from the `Users` list. Only after a correct match does `Window.Authenticated` become `true` and the window auto-opens.

```lua
KeySystem = {
    Enabled = true,
    Title = "Spectrum Security",
    Note = "Please enter your credentials.",
    Users = {
        { User = "admin", Pass = "1234" },
        { User = "user",  Pass = "spectrum2026" },
    }
}
```

Incorrect credentials flash the password field red and clear it; correct credentials play a success sound and animate the login card away.

---

## 🗂️ Tabs

```lua
local Tab = Window:AddTab("My Tab", true)   -- two-column layout
local Tab = Window:AddTab("My Tab", false)  -- standard single-column layout
```

The second argument controls layout mode:

- **`true`** → the tab's page becomes a horizontal flex container; sections created with `"Left"` or `"Right"` sit side by side, each taking roughly half the width.
- **`false`** (or omitted) → sections stack vertically in a single column, the default behavior.

---

## 📐 Sections

Sections are card-style containers with a title, an accent underline, and an internal list of components.

```lua
local Section = Tab:AddSection("Section Name", alignment)
```

| `alignment` | Effect | Valid only when |
|---|---|---|
| `"Left"` | Placed in the left half | Tab created with `twoColumn = true` |
| `"Right"` | Placed in the right half | Tab created with `twoColumn = true` |
| `"Single"` / omitted | Full-width, stacked normally | Any tab |

---

## 🎛️ Components Reference

All components live on a `Section` object and return an **API table** you can use afterward (`:Set()`, `:Get()`, etc. where applicable).

### AddLabel
```lua
Section:AddLabel({ Text = "Some descriptive text" })
```
`API:Set(text)` updates the label text.

### AddButton
```lua
local Btn = Section:AddButton({
    Name = "My Button",
    Notifications = true,
    Callback = function() end
})
Btn:AddKeybind("F")  -- optional hotkey shortcut
```

### AddToggle
```lua
local Tgl = Section:AddToggle({
    Name = "My Toggle",
    Default = false,
    Notifications = true,
    Callback = function(state) end
})
Tgl:Set(true)
Tgl:Get()
Tgl:AddKeybind("G")
```

### AddSlider
```lua
local Sld = Section:AddSlider({
    Name = "My Slider",
    Min = 0, Max = 100,
    Default = 50,
    Increment = 5,
    Callback = function(value) end
})
Sld:Set(80)
Sld:Get()
```

### AddTextbox
```lua
local Box = Section:AddTextbox({
    Name = "My Textbox",
    PlaceholderText = "Type here...",
    Default = "",
    Callback = function(text) end
})
Box:Set("hello")
Box:Get()
```
Callback fires when Enter is pressed while focused.

### AddColorpicker
```lua
local Cp = Section:AddColorpicker({
    Name = "My Color",
    Default = Color3.fromRGB(0, 204, 255),
    Callback = function(color) end
})
Cp:Set(Color3.fromRGB(255, 0, 0))
Cp:Get()
```
Opens a floating saturation/value box plus a hue bar.

### AddDropdown
```lua
-- Single-select
local Dd = Section:AddDropdown({
    Name = "Mode",
    Options = {"Attack", "Defense", "Balanced"},
    Default = "Balanced",
    Callback = function(selected) end
})

-- Multi-select
local MultiDd = Section:AddDropdown({
    Name = "Multi Select",
    Options = {"A", "B", "C"},
    Default = {"A", "C"},
    Multi = true,
    Callback = function(selected) end
})

Dd:Set("Attack")
Dd:Get()
Dd:Refresh({"New1", "New2"})  -- replace the option list
```

### AddKeybind (standalone)
```lua
local Kb = Section:AddKeybind({
    Name = "Custom Keybind",
    Default = Enum.KeyCode.H,
    Mode = "Toggle",  -- "Button" or "Toggle"
    Callback = function(state) end
})
Kb:Set(Enum.KeyCode.J)
Kb:Get()
```
In `"Toggle"` mode a small indicator dot lights up in the accent color while active.

All components except `AddLabel` and `AddKeybind` accept `Notifications = true/false` to auto-fire a `Window:Notify()` toast whenever their callback runs (subject to the window's global `Notifications` setting).

---

## 🎨 Theming

```lua
Window:SetTheme("Dark")
```

Built-in themes: `Green`, `Dark`, `Foggy`, `Sun`, `Aqua`, `Crimson`, `Light`

Every themed element (backgrounds, strokes, text, accents) updates instantly and tab button highlighting refreshes to match the active theme.

---

## 🔔 Notifications

```lua
Window:Notify({
    Title = "Title Text",
    Content = "Body text of the notification.",
    Duration = 3
})
```

Notifications stack bottom-right, slide in with a back-ease animation, and slide out automatically after `Duration` seconds. Disable them globally with:

```lua
Window.GlobalNotifications = false
```

---

## 📊 Dashboard Tab (Optional)

When `ControlPanel = true` (default), Spectrum automatically appends a **Dashboard** tab containing:

- Live **FPS**, **Ping**, **Memory usage**, and **Network data received** widgets, refreshed twice per second
- A **profile card** showing the local player's avatar thumbnail, display name, username, and user ID
- The current place's name and a **copy Place ID** button (uses `setclipboard` if the executor supports it)

---

## ⌨️ Keybind System

Three ways to attach hotkeys:

| Method | Where | Behavior |
|---|---|---|
| `Button:AddKeybind("F")` | On a button | Pressing the key re-triggers the button's callback |
| `Toggle:AddKeybind("G")` | On a toggle | Pressing the key flips the toggle |
| `Section:AddKeybind({...})` | Standalone | Independent keybind element with `"Button"` or `"Toggle"` mode |

Clicking a keybind's key-button puts it into listening mode (`"..."`) until the next key press is captured.

---

## 🧩 Full Example

```lua
local Spectrum = loadstring(game:HttpGet("<raw-source-url>"))()

local Window = Spectrum:AddWindow({
    Name = "Example Panel",
    Theme = "Aqua",
    ToggleKeybind = Enum.KeyCode.RightControl,
})

local Tab = Window:AddTab("Main", true)

local Left = Tab:AddSection("Controls", "Left")
Left:AddButton({ Name = "Run Action", Callback = function() print("ran!") end })
Left:AddToggle({ Name = "Enable Feature", Default = false, Callback = function(s) end })

local Right = Tab:AddSection("Preferences", "Right")
Right:AddDropdown({ Name = "Mode", Options = {"A", "B"}, Default = "A", Callback = function(v) end })
Right:AddColorpicker({ Name = "Accent Color", Default = Color3.new(1,1,1), Callback = function(c) end })

Window:Notify({ Title = "Ready", Content = "Panel loaded successfully.", Duration = 3 })
```

---

## 📌 Notes on Two-Column Mode

- Pass `true` as the second argument of `Window:AddTab()` to enable the two-column layout for that tab.
- Use `"Left"` / `"Right"` as the second argument of `Tab:AddSection()` to place a section on a given side; each takes up roughly half the row width.
- Omit the argument, or pass `"Single"`, for a full-width section that stacks normally — this works in both layout modes.
- Single-column tabs (`AddTab(name, false)`) ignore the alignment argument and always stack sections vertically.
