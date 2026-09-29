# CS2_Admin with T3Menu Support

A modified version of **CS2_Admin** for SwiftlyS2 that replaces the built-in SwiftlyS2 admin menu with the Panorama-based **T3Menu** interface.

## Features

- All CS2_Admin menu sections and submenus use T3Menu.
- Three visual styles:
  - `SourceMod`
  - `Stylish`
  - `Source2`
- Three navigation modes:
  - `KeyPress`
  - `Scrollable`
  - `Clickable`
- Automatically closes the built-in SwiftlyS2 menu.
- Early T3Menu API registration for compatibility with servers where another plugin interrupts shared-interface injection.
- Official CS2_Admin auto-updates are disabled to prevent the custom integration from being overwritten.
- Supports existing CS2_Admin permissions, commands, translations, and configuration files.

## Requirements

- Counter-Strike 2 Dedicated Server
- SwiftlyS2
- .NET 10
- T3Menu 1.3.0
- Required T3Menu Workshop addon

### Required Workshop addon

https://steamcommunity.com/sharedfiles/filedetails/?id=3790988631

### Original T3Menu repository

https://github.com/T3Marius/T3Menu

> Players must download the Workshop addon. Otherwise, the Panorama layout, styles, cursor interaction, and sounds may not work correctly.

## Installation

### 1. Stop the server

Fully stop the server before replacing any files.

Do not use hot reload or `sw plugins load` for the initial installation.

### 2. Remove previous versions

Remove the following directories if they exist:

```text
addons/swiftlys2/plugins/CS2_Admin
addons/swiftlys2/plugins/T3Menu
```

Check that no duplicate copies of `CS2_Admin.dll` remain:

```bash
find addons/swiftlys2/plugins -type f -name "CS2_Admin.dll"
```

Only one copy should exist after installation.

### 3. Extract the release archive

Extract the release archive into:

```text
game/csgo/addons/swiftlys2/plugins/
```

The resulting directory structure must be:

```text
addons/swiftlys2/plugins/
├── CS2_Admin/
│   ├── CS2_Admin.dll
│   └── resources/
└── T3Menu/
    ├── T3Menu.dll
    └── resources/
        └── exports/
            └── T3Menu.Contract.dll
```

> The plugin directory must be named `T3Menu`, matching the `T3Menu.dll` filename. Do not rename it to `T3MenuV1.3.0`.

### 4. Configure CS2_Admin

Open the main CS2_Admin configuration file:

```text
addons/swiftlys2/configs/plugins/CS2_Admin/config.json
```

Make sure the following options are configured:

```json
{
  "AutoUpdate": false,
  "DisableBuiltInMenuWhenUsingT3Menu": true
}
```

| Option | Value | Description |
|---|---:|---|
| `AutoUpdate` | `false` | Prevents the official updater from replacing this custom build |
| `DisableBuiltInMenuWhenUsingT3Menu` | `true` | Closes the built-in SwiftlyS2 menu when T3Menu is opened |

### 5. Start the server

Start the server normally.

Test T3Menu first:

```text
!t3menu_test
```

Then test the admin menu:

```text
!admin
```

If both menus open correctly, the installation is complete.

## T3Menu configuration

T3Menu controls the appearance and navigation mode of the admin menu.

Open the T3Menu configuration file:

```text
addons/swiftlys2/configs/plugins/T3Menu/t3menu.jsonc
```

The main options are:

```jsonc
{
  "Navigation": "Clickable",
  "Style": "Stylish",
  "ItemsPerPage": 6
}
```

Do not remove the other options from the configuration file. Only change the values you need.

## Visual styles

### SourceMod

```json
"Style": "SourceMod"
```

A compact classic menu inspired by SourceMod.

### Stylish

```json
"Style": "Stylish"
```

A modern card-based menu with animations and visual effects.

### Source2

```json
"Style": "Source2"
```

A menu designed to match the visual style of Counter-Strike 2.

## Navigation modes

### Clickable

```json
"Navigation": "Clickable"
```

The menu is controlled directly with the mouse.

- Holding Tab is not required.
- T3Menu automatically enables the cursor.
- Menu entries can be clicked directly.
- The cursor is disabled after the menu is closed.

Recommended configuration:

```jsonc
{
  "Navigation": "Clickable",
  "Style": "Stylish",
  "ItemsPerPage": 6
}
```

### Scrollable

```json
"Navigation
