# CSGO-UI-lib Documentation

## Introduction
CSGO-UI-lib is a modern, feature-rich Roblox UI library inspired by Counter-Strike: Global Offensive’s Cheat Menus. It provides a modular, themeable, and highly customizable framework for building professional game interfaces with minimal effort. The library supports a wide range of controls, advanced theming, notifications, and even a global chat system, all optimized for both desktop and mobile Roblox experiences.

---

## Features
- **CSGO-inspired design**
- **Windows, Pages, and Sections** for organized layouts
- **Toggles, Sliders, Dropdowns, Color Pickers, Keybinds, Textboxes, Listboxes, Labels, Buttons**
- **Notifications** and **Global Chat**
- **Theming** with easy color and style customization
- **Mobile support** (touch events, scaling)
- **Draggable and resizable windows**
- **Performance optimized**

---


## Usage
You do **not** need to download or manually install any files. Simply require the library directly in your script using the following one-liner:

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Gh-anime/CSGO-UI-lib/main/main.lua"))()
```

This will fetch and load the latest version of CSGO-UI-lib instantly, just like Rayfield. You can now immediately start building your UI.

---


## Getting Started
Below is a minimal example that creates a window, adds a toggle, and shows a notification. You can copy and paste this directly into your Roblox executor or script:

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Gh-anime/CSGO-UI-lib/main/main.lua"))()

local Window = Library:Window({
    Name = "Demo Window",
    SubName = "Example UI",
    Logo = "120959262762131" -- Optional asset ID
})

local MainPage = Window:Page({Name = "Main"})
local Section = MainPage:Section({Name = "Controls"})

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

You can add as many pages, sections, and controls as you need. All UI is created and managed in real time—no manual asset management required.

---


## API Reference

### Window
Create a new UI window. This is the root of your interface.
```lua
local window = Library:Window({
    Name = "Window Title",         -- Required
    SubName = "Subtitle",          -- Optional
    Logo = "AssetId"               -- Optional (Roblox asset ID)
})
```

### Page
Add a page (tab) to a window. Pages help organize your UI into logical groups.
```lua
local page = window:Page({
    Name = "Page Name",            -- Required
    Icon = "AssetId",              -- Optional (Roblox asset ID)
    Columns = 2                    -- Optional (default: 2)
})
```

### Section
Add a section to a page. Sections are vertical groups of controls, and can be placed in different columns.
```lua
local section = page:Section({
    Name = "Section Title",        -- Required
    Description = "Description",   -- Optional
    Icon = "AssetId",              -- Optional
    Side = 1                       -- Optional (column index, default: 1)
})
```

### Controls
All controls are added via section methods. Each returns a control object and can be customized with callbacks and options. See below for all supported controls:


#### Toggle
Creates a switchable on/off button.
```lua
section:Toggle({
    Name = "My Toggle",         -- Display text
    Flag = "UniqueFlag",        -- Unique identifier for saving/loading
    Default = false,            -- Initial state
    Callback = function(value)  -- Called when toggled
        print(value)
    end
})
```


#### Slider
Creates a draggable value bar.
```lua
section:Slider({
    Name = "My Slider",         -- Display text
    Flag = "SliderFlag",        -- Unique identifier
    Min = 0,                    -- Minimum value
    Max = 100,                  -- Maximum value
    Default = 50,               -- Initial value
    Decimals = 1,               -- Decimal places
    Suffix = "%",               -- Suffix (e.g. %, px)
    Callback = function(value)   -- Called on change
        print(value)
    end
})
```


#### Dropdown
Creates a dropdown menu for selecting one or more options.
```lua
section:Dropdown({
    Name = "My Dropdown",       -- Display text
    Flag = "DropdownFlag",      -- Unique identifier
    Items = {"Option1", "Option2"}, -- List of options
    Default = "Option1",        -- Default selected
    Multi = false,              -- Allow multiple selection
    Callback = function(value)   -- Called on change
        print(value)
    end
})
```


#### Color Picker
Lets the user pick a color (with optional alpha).
```lua
section:Label("Pick a color"):Colorpicker({
    Name = "Color",                 -- Display text
    Flag = "ColorFlag",             -- Unique identifier
    Default = Color3.fromRGB(255,255,255), -- Default color
    Callback = function(color)       -- Called on change
        print(color)
    end
})
```


#### Keybind
Lets the user assign a keyboard key or mouse button.
```lua
section:Keybind({
    Name = "My Keybind",         -- Display text
    Flag = "KeyFlag",            -- Unique identifier
    Default = Enum.KeyCode.X,     -- Default key
    Callback = function(key)      -- Called on change
        print(key)
    end
})
```


#### Textbox
Single-line text input (optionally numeric only).
```lua
section:Textbox({
    Flag = "TextboxFlag",        -- Unique identifier
    Default = "",                -- Default text
    Numeric = false,             -- Only allow numbers
    Placeholder = "Type here...",-- Placeholder text
    Finished = true,             -- Callback only when focus lost
    Callback = function(text)    -- Called on change
        print(text)
    end
})
```


#### Listbox
Scrollable list of options, with search.
```lua
section:Listbox({
    Flag = "ListboxFlag",        -- Unique identifier
    Default = "First",           -- Default selected
    Size = 275,                   -- Height in pixels
    Items = {"First", "Second", "Third"}, -- List of options
    Multi = false,                -- Allow multiple selection
    Callback = function(value)    -- Called on change
        print(value)
    end
})
```


#### Button
Clickable button for actions.
```lua
section:Button({
    Name = "Click Me",           -- Display text
    Callback = function()         -- Called on click
        print("Button pressed!")
    end
})
```


#### Label
Static text label (can be used as a header or description).
```lua
section:Label("This is a label")
```

---


## Notifications
Show a notification at any time from anywhere in your script:
```lua
Library:Notification({
    Title = "Title",             -- Notification title
    Description = "Message body", -- Main text
    Duration = 3                  -- Seconds to display
})
```

---


## Global Chat
Add a global chat UI to any page (great for multiplayer scripts):
```lua
local chat = page:GlobalChat(columnIndex)
chat:OnMessageSendPressed(function()
    chat:SendMessage(avatarId, username, message, status)
end)
```

---


## Theming & Customization
You can fully customize the UI’s appearance by editing the `Library.Theme` table **before** creating any windows. All colors use Roblox's `Color3` type. Example:
```lua
Library.Theme.Accent = Color3.fromRGB(255, 0, 0) -- Red accent
Library.Theme.Background = Color3.fromRGB(20, 20, 20)
Library.Theme.Text = Color3.fromRGB(255, 255, 255)
```
You can also change fonts, outlines, and more. See the source for all theme keys.

---


## Advanced Usage
- **Draggable/Resizable Windows:** All windows are draggable and resizable by default—no extra code needed.
- **Mobile Support:** UI adapts to touch input and screen size automatically.
- **Saving/Loading Configs:** The library supports saving/loading user settings to disk (where supported by executor).
- **Custom Fonts/Assets:** Use your own fonts and image assets by providing Roblox asset IDs in options.
- **Dynamic UI:** You can add, remove, or update controls at runtime.

---


## FAQ
**Q: Is this library safe to use?**
A: The code is open source and can be reviewed on GitHub. Always check scripts before running them in executors.

**Q: Can I use this in my own game or script?**
A: Yes! You are free to use and modify CSGO-UI-lib. Please credit the original author.

**Q: How do I report bugs or request features?**
A: Open an issue or pull request on the [GitHub repository](https://github.com/Gh-anime/CSGO-UI-lib).

---


## Credits
- Original Author: Gh-anime
- Documentation: GitHub Copilot (GPT-4.1)
- Inspired by Rayfield UI Library

---


For more advanced examples, updates, and the full source code, visit the [GitHub page](https://github.com/Gh-anime/CSGO-UI-lib).
