# CSGO UI Library Documentation

**CSGO UI Library** is a feature-rich Roblox UI framework inspired by Counter-Strike: Global Offensive cheat menus. The library is optimized for both desktop and mobile, supporting touch input, automatic scaling, and a comprehensive set of UI controls.


---
*Supported Execs:*
-Volt
-Potassium
-Volcano
-Wave
-Isaeva
-Synapse Z
-Velocity
-Seliware
-Bunni.fun
-SirHurt
-Xeno ( partially )
-Solara ( partially )
-Hydrogen
-MacSploit
-OpiumWare
-Delta
-Cryptic
-Vega X
-Codex

---

## Features

- **CSGO‑inspired Design** – Clean, modern, and professional aesthetics.
- **Hierarchical Structure** – Organize UI into Windows, Pages, and Sections.
- **Rich Control Set** – Toggle, Slider, Dropdown, Color Picker, Keybind, Textbox, Listbox, Button, Label.
- **Notifications & Global Chat** – Built‑in notification system and a complete chat widget.
- **Full Theming** – Customize colors, fonts, and gradients.
- **Mobile Support** – Touch events and automatic scaling.
- **Draggable & Resizable** – Windows can be moved and resized.
- **Configuration Management** – Save and load user settings (requires executor with `writefile`).
- **Performance Optimized** – Efficient rendering and event handling.

---

## Installation

Load the library directly from GitHub using the raw URL. No files need to be downloaded.

```lua
local CSGO = loadstring(game:HttpGet("https://raw.githubusercontent.com/Gh-anime/CSGO-UI-lib/main/main.lua"))()
```

After loading, the library is available under the variable `CSGO` (or any name you choose). You can now create your UI.

---

## Quick Start

This minimal example creates a window, adds a page, a section, and a toggle, and shows a notification.

```lua
local CSGO = loadstring(game:HttpGet("https://raw.githubusercontent.com/Gh-anime/CSGO-UI-lib/main/main.lua"))()

-- Create the main window
local Window = CSGO:Window({
    Name = "CSGO UI Demo",
    SubName = "Example Interface",
    Logo = "120959262762131"   -- optional
})

-- Add a page
local MainPage = Window:Page({ Name = "Main" })

-- Add a section to the page (first column)
local Section = MainPage:Section({ Name = "Controls" })

-- Add a toggle
Section:Toggle({
    Name = "Enable Feature",
    Flag = "EnableFeature",
    Default = false,
    Callback = function(state)
        print("Feature enabled:", state)
    end
})

-- Show a notification
CSGO:Notification({
    Title = "Welcome!",
    Description = "CSGO UI Library loaded successfully.",
    Duration = 3
})
```

---

## API Reference

### Window

The top-level container. All UI elements are placed inside a window.

```lua
local window = CSGO:Window({
    Name = "Window Title",   -- required
    SubName = "Subtitle",    -- optional
    Logo = "AssetId"         -- optional (Roblox asset ID)
})
```

#### Methods

| Method | Description |
|--------|-------------|
| `:SetOpen(bool)` | Opens or closes the window. |
| `:SetCenter()` | Centers the window on the screen. |
| `:SetTransparency(value)` | Adjusts background transparency (used in settings). |
| `:Init()` | Initializes the window (called automatically). |

### Page

Adds a page (tab) to a window. Pages are displayed in the left sidebar.

```lua
local page = window:Page({
    Name = "Page Name",      -- required
    Icon = "AssetId",        -- optional (Roblox asset ID)
    Columns = 2              -- optional, number of columns (default: 2)
})
```

#### Methods

| Method | Description |
|--------|-------------|
| `:Turn(bool)` | Activates or deactivates the page. |
| `:Section(data)` | Creates a new section on this page (see Section). |
| `:GlobalChat(side)` | Adds a global chat widget to the specified column (see GlobalChat). |

### Section

A container that holds UI controls. Sections are placed in columns (1-indexed) and can be collapsed.

```lua
local section = page:Section({
    Name = "Section Title",  -- required
    Description = "Desc",    -- optional
    Icon = "AssetId",        -- optional
    Side = 1                 -- optional, column index (default: 1)
})
```

#### Methods

| Method | Description |
|--------|-------------|
| `:ToggleBackground()` | Collapses or expands the section. |
| `:TweenElements(bool)` | Animates the section's content visibility. |

### Controls

All controls are created as methods on a section. They return a control object that can be manipulated at runtime.

---

#### Toggle

```lua
local toggle = section:Toggle({
    Name = "My Toggle",
    Flag = "Toggle1",
    Default = false,
    Callback = function(state) end
})
```

**Methods:**

| Method | Description |
|--------|-------------|
| `:Set(value)` | Sets the toggle state (true/false). |
| `:Get()` | Returns the current state. |
| `:SetVisibility(bool)` | Shows or hides the toggle. |
| `:Settings(size)` | Adds a settings menu (a popup) with nested controls (returns a Section object). |
| `:Colorpicker(data)` | Adds a color picker as a sub‑element (only if called on the toggle). |
| `:Keybind(data)` | Adds a keybind as a sub‑element. |

---

#### Slider

```lua
local slider = section:Slider({
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

**Methods:**

| Method | Description |
|--------|-------------|
| `:Set(value)` | Sets the slider value (clamped to Min/Max). |
| `:Get()` | Returns the current value. |
| `:SetVisibility(bool)` | Shows or hides the slider. |

---

#### Dropdown

```lua
local dropdown = section:Dropdown({
    Name = "My Dropdown",
    Flag = "Dropdown1",
    Items = {"Option1", "Option2"},
    Default = "Option1",
    Multi = false,         -- allow multiple selection
    Callback = function(value) end
})
```

**Methods:**

| Method | Description |
|--------|-------------|
| `:Set(option)` | Selects an option (or table of options if Multi is true). |
| `:Get()` | Returns the selected option(s). |
| `:SetVisibility(bool)` | Shows or hides the dropdown. |
| `:Add(option)` | Adds a new option to the dropdown. |
| `:Remove(option)` | Removes an option. |
| `:Refresh(list)` | Replaces all options with a new list. |

---

#### Color Picker

Attached to a label. The label serves as the button that opens the color picker.

```lua
local colorPicker = section:Label("Pick a color"):Colorpicker({
    Name = "Color",
    Flag = "ColorFlag",
    Default = Color3.fromRGB(255,255,255),
    Callback = function(color) end
})
```

**Methods:**

| Method | Description |
|--------|-------------|
| `:Set(color, alpha)` | Sets the color (Color3 or hex string) and alpha (0-1). |
| `:Get()` | Returns the current color and alpha. |
| `:SetOpen(bool)` | Opens or closes the color picker window. |

---

#### Keybind

```lua
local keybind = section:Keybind({
    Name = "My Keybind",
    Flag = "Keybind1",
    Default = Enum.KeyCode.X,
    Mode = "Toggle",       -- "Toggle", "Hold", or "Always"
    Callback = function(key) end
})
```

**Methods:**

| Method | Description |
|--------|-------------|
| `:Set(key)` | Sets the keybind (can be Enum.KeyCode, Enum.UserInputType, or a table with Mode and Key). |
| `:Get()` | Returns the current key, mode, and toggle state. |
| `:SetMode(mode)` | Changes the mode ("Toggle", "Hold", "Always"). |

---

#### Textbox

```lua
local textbox = section:Textbox({
    Flag = "Textbox1",
    Default = "",
    Numeric = false,
    Placeholder = "Type here...",
    Finished = true,       -- callback only when focus lost
    Callback = function(text) end
})
```

**Methods:**

| Method | Description |
|--------|-------------|
| `:Set(value)` | Sets the textbox content. |
| `:Get()` | Returns the current text. |
| `:SetVisibility(bool)` | Shows or hides the textbox. |

---

#### Listbox

```lua
local listbox = section:Listbox({
    Flag = "Listbox1",
    Default = "First",
    Size = 275,            -- height in pixels
    Items = {"First", "Second", "Third"},
    Multi = false,
    Callback = function(value) end
})
```

**Methods:**

| Method | Description |
|--------|-------------|
| `:Set(option)` | Selects an option (or table of options if Multi is true). |
| `:Get()` | Returns the selected option(s). |
| `:SetVisibility(bool)` | Shows or hides the listbox. |
| `:Add(option)` | Adds a new option to the list. |
| `:Remove(option)` | Removes an option. |
| `:Refresh(list)` | Replaces all options with a new list. |

---

#### Button

```lua
local button = section:Button({
    Name = "Click Me",
    Icon = "AssetId",      -- optional
    Callback = function() end
})
```

**Methods:**

| Method | Description |
|--------|-------------|
| `:Press()` | Simulates a button press (calls the callback with animation). |
| `:SetVisibility(bool)` | Shows or hides the button. |

---

#### Label

```lua
local label = section:Label("This is a label")
```

**Methods:**

| Method | Description |
|--------|-------------|
| `:SetText(text)` | Changes the label text. |
| `:SetVisibility(bool)` | Shows or hides the label. |
| `:Colorpicker(data)` | Adds a color picker to this label (returns the color picker object). |

---

### Notifications

Display a temporary notification that fades out after a specified duration.

```lua
CSGO:Notification({
    Title = "Title",
    Description = "Message body",
    Duration = 3,          -- seconds
    Icon = "AssetId",      -- optional, Roblox asset ID
    IconColor = {          -- optional, color gradient for the icon
        Start = Color3.fromRGB(255,0,0),
        End = Color3.fromRGB(255,255,0)
    }
})
```

---

### Global Chat

Add a fully functional chat widget to any page. It includes an input field, message history, and send button.

```lua
local chat = page:GlobalChat(columnIndex)   -- columnIndex optional (default: 1)
```

**Methods:**

| Method | Description |
|--------|-------------|
| `:OnMessageSendPressed(callback)` | Sets the callback that is called when the user sends a message. |
| `:GetTypedMessage()` | Returns the current text in the input field. |
| `:ClearText()` | Clears the input field. |
| `:SendMessage(avatarId, username, message, isLocalPlayer)` | Adds a message to the chat history. `isLocalPlayer` determines alignment. |
| `:SetStatus(text, color)` | Updates the status indicator (text and color). |
| `:SetStatusText(text)` | Updates the status text only. |
| `:SetVisibility(bool)` | Shows or hides the chat widget. |

---

## Theming

Modify the `CSGO.Theme` table before creating any windows to change the UI appearance. All color values are `Color3`.

```lua
CSGO.Theme.Accent = Color3.fromRGB(255, 0, 0)          -- primary accent
CSGO.Theme.AccentGradient = Color3.fromRGB(0, 195, 255)
CSGO.Theme.Background = Color3.fromRGB(12, 12, 14)
CSGO.Theme.Background2 = Color3.fromRGB(10, 10, 12)
CSGO.Theme.Text = Color3.fromRGB(235, 235, 235)
CSGO.Theme.Outline = Color3.fromRGB(25, 25, 28)
CSGO.Theme.Element = Color3.fromRGB(16, 16, 18)
CSGO.Theme["Section Background"] = Color3.fromRGB(10, 10, 12)
CSGO.Theme["Section Background 2"] = Color3.fromRGB(14, 14, 16)
CSGO.Theme["Section Top"] = Color3.fromRGB(28, 27, 31)
```

**Changing Theme at Runtime:**  
Use `CSGO:ChangeTheme(themeKey, newColor)` to update a theme value. All existing UI elements that reference that theme key will update automatically.

---

## Advanced Usage

### Configuration Management

The library includes a built‑in settings page that automatically saves and loads flagged controls. To enable configs, ensure your executor supports `writefile` and `readfile`. The settings page is created with:

```lua
local SettingsPage = CSGO:CreateSettingsPage(Window, KeybindList)
```

You can also manually save/load:

```lua
local configString = CSGO:GetConfig()
CSGO:LoadConfig(configString)
```

### Dynamic Control Manipulation

Each control returns an object that allows runtime changes:

```lua
local myToggle = section:Toggle({...})
myToggle:Set(true)
myToggle:SetVisibility(false)
```

### Custom Fonts

The library supports custom fonts via `CSGO.Fonts`. Available weights: "Light", "Regular", "SemiBold". You can change the global font:

```lua
CSGO.Font = CSGO.Fonts.SemiBold
CSGO:UpdateText()
```

### Mobile Adaptations

When `UserInputService.TouchEnabled` is true, the library automatically:
- Replaces mouse events with touch events.
- Adds a floating button to open the menu.
- Scales the UI slightly.

No additional configuration is needed.

---

## GitHub Repository

[https://github.com/Gh-anime/CSGO-UI-lib](https://github.com/Gh-anime/CSGO-UI-lib)

---

## License & Credits

- **Author:** Xen (GHP)
- **Documentation:** Xen

This library is open‑source and provided under the MIT License. You are free to use and modify it in your projects.
