# Spectrum UI Library v3.4.0

Spectrum UI is a modern, feature-rich, and lightweight user interface library designed for Luau script development. It features a clean aesthetic, built-in themes, a robust key system, and extensive component control.

---

## Features

- **Multiple Themes:** Easily switch themes on the fly.
- **Key System:** Advanced multi-user authentication support (Username + Password).
- **Rich Components:** Buttons, Toggles, Keybinds, Sliders, Textboxes, Colorpickers, Dropdowns (Single/Multi), and Labels.
- **Dynamic Updates:** Comprehensive runtime methods to control, get, and set element values programmatically.
- **Built-in Notifications:** Clean toast notifications for user interactions.

---

## Installation

Load the library via your script executor using `loadstring`:

```lua
local Spectrum = loadstring(game:HttpGet("YOUR_LIBRARY_URL_HERE"))()

Window Initialization & Methods
Creating a Window
local Window = Spectrum:AddWindow({
   Name = "Window Title",
   Theme = "Aqua", -- Options: "Aqua", "Dark", "Green", "Crimson", "Sun", "Foggy", "Light"
   ToggleKeybind = Enum.KeyCode.RightControl,
   Size = UDim2.new(0, 650, 0, 420),
   Notifications = true,
   ToggleButton = true,
   
   -- Optional Key System
   KeySystem = {
       Enabled = true,
       Title = "Security Check",
       Note = "Please enter your credentials.",
       Users = {
           { User = "admin", Pass = "1234" }
       }
   }
})
```

```
Window Methods
-- Create a Tab
local Tab = Window:AddTab("Tab Name")

-- Change Theme at runtime
Window:SetTheme("Dark")
```


```
-- Send a Notification
Window:Notify({
   Title = "Notification Title",
   Content = "This is a notification message.",
   Duration = 3
})
```


## Tabs & Sections
```
local Tab = Window:AddTab("Main Tab")
local Section = Tab:AddSection("General Section")
```


## UI Components & Usage


## 1. Button
```
Section:AddButton({
   Name = "Click Me",
   Notifications = true,
   Callback = function()
       print("Button clicked!")
   end
})
```

## 2. Toggle
```
local MyToggle = Section:AddToggle({
   Name = "Auto Farm",
   Default = false,
   Notifications = true,
   Callback = function(state)
       print("Toggle state:", state)
   end
})


-- Optional: Add a quick keybind to the toggle
MyToggle:AddKeybind("F")
```

## 3. Keybind
```
local MyKeybind = Section:AddKeybind({
   Name = "Teleport Key",
   Default = Enum.KeyCode.E,
   Mode = "Button", -- "Button" or "Toggle"
   Notifications = true,
   Callback = function()
       print("Keybind triggered!")
   end
})
```

## 4. Slider
```
local MySlider = Section:AddSlider({
   Name = "WalkSpeed",
   Min = 16,
   Max = 500,
   Default = 16,
   Increment = 1,
   Notifications = true,
   Callback = function(value)
       print("Slider value:", value)
   end
})
```

## 5. Textbox
```
local MyTextbox = Section:AddTextbox({
   Name = "Enter Command",
   PlaceholderText = "Type here...",
   Default = "",
   Notifications = true,
   Callback = function(text)
       print("Input text:", text)
   end
})
```

## 6. Colorpicker
```
local MyColorpicker = Section:AddColorpicker({
   Name = "Accent Color",
   Default = Color3.fromRGB(255, 255, 255),
   Notifications = true,
   Callback = function(color)
       print("Selected color:", color)
   end
})
```

## 7. Dropdown (Single & Multi)
```
-- Single Selection
local MyDropdown = Section:AddDropdown({
   Name = "Select Weapon",
   Options = {"Pistol", "Rifle", "Knife"},
   Default = "Pistol",
   Multi = false,
   Notifications = true,
   Callback = function(selected)
       print("Selected:", selected)
   end
})

-- Multi Selection
local MyMultiDropdown = Section:AddDropdown({
   Name = "Select Features",
   Options = {"ESP", "Aimbot", "Fly"},
   Default = {"ESP"},
   Multi = true,
   Notifications = true,
   Callback = function(selectedList)
       for k, v in pairs(selectedList) do
           print(k, v)
       end
   end
})
```

## 8. Label
```
local MyLabel = Section:AddLabel({
   Text = "Status: Idle"
})
```

## Element Methods & Functions
Most components support runtime modification via specific methods:

## Setting & Getting Values
```
-- Toggle
MyToggle:Set(true)
local state = MyToggle:Get()

-- Slider
MySlider:Set(150)
local value = MySlider:Get()

-- Textbox
MyTextbox:Set("New Text")
local text = MyTextbox:Get()

-- Dropdown
MyDropdown:Set("Rifle")
MyDropdown:Refresh({"Option 1", "Option 2"}, "Option 1")

-- Colorpicker
MyColorpicker:Set(Color3.fromRGB(255, 0, 0))
local color = MyColorpicker:Get()

-- Keybind
MyKeybind:Set(Enum.KeyCode.F)

-- Label
MyLabel:Set("Status: Running")

Universal Element Controls
-- Hide element
MyElement:Visible(false)

-- Show element
MyElement:Visible(true)

-- Completely remove element from UI
MyElement:Destroy()
```
# License
This library is open-source and free to use for script development purposes.

