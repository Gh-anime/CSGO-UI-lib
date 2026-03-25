# CSGO-UI-lib Documentation

## Introduction
CSGO-UI-lib is a modern, feature-rich Roblox UI library inspired by Counter-Strike: Global Offensive cheat menus. It provides a modular, themeable framework for building professional game interfaces with minimal effort. The library is optimized for both desktop and mobile, supporting touch events, scaling, and a wide range of controls.

---

## Features
- CSGO‑inspired design (clean, professional)
- Windows, Pages, and Sections for organized layouts
- Controls: Toggles, Sliders, Dropdowns, Color Pickers, Keybinds, Textboxes, Listboxes, Buttons, Labels
- Notifications and Global Chat system
- Theming (easy color/style customization)
- Mobile support (touch events, automatic scaling)
- Draggable & resizable windows
- Performance optimized

---

## Installation
You do not need to download any files. Load the library directly in your script:

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Gh-anime/CSGO-UI-lib/main/main.lua"))()
```

This fetches the latest version. You can now start building your UI.

---

## Quick Start
Here’s a minimal example that creates a window, adds a toggle, and shows a notification:

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Gh-anime/CSGO-UI-lib/main/main.lua"))()

local Window = Library:Window({
    Name = "Demo Window",
    SubName = "Example UI",
    Logo = "120959262762131"   -- Optional Roblox asset ID
})

local MainPage = Window:Page({ Name = "Main" })
local Section = MainPage:Section({ Name = "Controls" })

Section:Toggle({
    Name = "Enable Feature",
    Flag = "EnableFeature",
    Default = false,
    Callback = function(state)
        print("Feature enabled:", state)
    end
})

Library:Notification({
    Title = "Welcome!",
    Description = "CSGO-UI-lib loaded successfully.",
    Duration = 3
})
```

---

## API Reference

### Window
Create the main window (the root of your interface).

```lua
local window = Library:Window({
    Name = "Window Title",   -- Required
    SubName = "Subtitle",    -- Optional
    Logo = "AssetId"         -- Optional (Roblox asset ID)
})
```

### Page
Add a page (tab) to a window. Pages group controls logically.

```lua
local page = window:Page({
    Name = "Page Name",      -- Required
    Icon = "AssetId",        -- Optional (Roblox asset ID)
    Columns = 2              -- Optional (number of columns, default: 2)
})
```

### Section
Add a section to a page. Sections are vertical containers placed in a column.

```lua
local section = page:Section({
    Name = "Section Title",  -- Required
    Description = "Desc",    -- Optional
    Icon = "AssetId",        -- Optional
    Side = 1                 -- Optional (column index, default: 1)
})
```

### Controls
All controls are created using methods on a section. They return a control object that can be used for later manipulation (e.g., `:SetValue()`). Most controls accept a `Flag` string for saving/loading – ensure it is unique across your script.

#### Toggle
```lua
section:Toggle({
    Name = "My Toggle",
    Flag = "Toggle1",
    Default = false,
    Callback = function(state) end
})
```

#### Slider
```lua
section:Slider({
    Name = "My Slider",
    Flag = "Slider1",
    Min = 0,
    Max = 100,
    Default = 50,
    Decimals = 1,          -- optional
    Suffix = "%",          -- optional
    Callback = function(value) end
})
```

#### Dropdown
```lua
section:Dropdown({
    Name = "My Dropdown",
    Flag = "Dropdown1",
    Items = {"Option1", "Option2"},
    Default = "Option1",
    Multi = false,         -- allow multiple selection
    Callback = function(value) end
})
```

#### Color Picker
The color picker is attached to a label. Create a label, then call `:Colorpicker()` on it.

```lua
section:Label("Pick a color"):Colorpicker({
    Name = "Color",        -- display name
    Flag = "ColorFlag",
    Default = Color3.fromRGB(255,255,255),
    Callback = function(color) end
})
```

#### Keybind
```lua
section:Keybind({
    Name = "My Keybind",
    Flag = "Keybind1",
    Default = Enum.KeyCode.X,
    Callback = function(key) end
})
```

#### Textbox
```lua
section:Textbox({
    Flag = "Textbox1",
    Default = "",
    Numeric = false,
    Placeholder = "Type here...",
    Finished = true,       -- only callback when focus lost
    Callback = function(text) end
})
```

#### Listbox
```lua
section:Listbox({
    Flag = "Listbox1",
    Default = "First",
    Size = 275,            -- height in pixels
    Items = {"First", "Second", "Third"},
    Multi = false,
    Callback = function(value) end
})
```

#### Button
```lua
section:Button({
    Name = "Click Me",
    Callback = function() end
})
```

#### Label
```lua
section:Label("This is a label")
```

---

### Notifications
Show a temporary popup notification from anywhere:

```lua
Library:Notification({
    Title = "Title",
    Description = "Message body",
    Duration = 3   -- seconds
})
```

### Global Chat
Add a global chat widget to any page. It provides a built‑in message input and display area.

```lua
local chat = page:GlobalChat(columnIndex)   -- columnIndex is optional (default 1)
chat:OnMessageSendPressed(function()
    -- Called when the user sends a message
    chat:SendMessage(avatarId, username, message, status)
end)
```

---

## Theming & Customization
Modify the `Library.Theme` table **before** creating any windows to change the UI’s appearance. All colors are `Color3`.

```lua
Library.Theme.Accent = Color3.fromRGB(255, 0, 0)      -- primary accent
Library.Theme.Background = Color3.fromRGB(20, 20, 20)
Library.Theme.Text = Color3.fromRGB(255, 255, 255)
Library.Theme.Border = Color3.fromRGB(40, 40, 40)
```

Other theme keys (font, outline, etc.) can be found in the source.

---

## Advanced Usage

- **Draggable/Resizable Windows:** All windows are draggable and resizable by default.
- **Mobile Support:** The UI automatically adapts to touch input and screen size.
- **Saving/Loading Configs:** The library supports saving/loading user settings using `Flag` strings (requires writefile support in the executor).
- **Custom Fonts/Assets:** Use your own fonts and images by providing Roblox asset IDs where options accept them (e.g., `Logo`, `Icon`).
- **Dynamic UI:** You can add, remove, or update controls at runtime using their returned objects (e.g., `control:SetVisible(false)`).

---

## FAQ

**Q: Is this library safe to use?**  
A: The code is open source and can be reviewed on GitHub. Always verify scripts before running them in executors.

**Q: Can I use this in my own game or script?**  
A: Yes! You are free to use and modify CSGO-UI-lib. Please credit the original author.

**Q: How do I report bugs or request features?**  
A: Open an issue or pull request on the [GitHub repository](https://github.com/Gh-anime/CSGO-UI-lib).

---

**License & Credits**  
- Original Author: Xen
- Documentation: by Xen (GHP)
