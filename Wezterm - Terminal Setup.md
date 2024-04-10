---
tags:
- terminal
---

## Installation

```zsh
brew install --cask wezterm
```

WezTerm Config: https://wezfurlong.org/wezterm/config/files.html

Create Profile at `~/.config/wezterm/wezterm.lua`
```lua
-- Pull in the wezterm API
local wezterm = require 'wezterm'
local bindings = require 'keys' -- reads from `keys.lua`

-- This will hold the configuration.
local config = wezterm.config_builder()

-- This is where you actually apply your config choices

-- For example, changing the color scheme:
config.color_scheme = 'Tokyo Night Storm'
config.font_size = 14

config.use_fancy_tab_bar = false

config.disable_default_key_bindings = true
config.keys = bindings.keys

-- and finally, return the configuration to wezterm
return config
```

Add file for keys at `~/.config/wezterm/keys.lua`
```lua
local wezterm = require 'wezterm'
local act = wezterm.action

return {
    keys = {
        { key = 'RightArrow', mods = 'SUPER',          action = act.ActivateTabRelative(1) },
        { key = 'LeftArrow',  mods = 'SUPER',          action = act.ActivateTabRelative(-1) },
        { key = '[',          mods = 'SUPER',          action = act.ActivateTabRelative(-1) },
        { key = '{',          mods = 'SHIFT|SUPER',    action = act.ActivateTabRelative(-1) },
        { key = ']',          mods = 'SUPER',          action = act.ActivateTabRelative(1) },
        { key = '}',          mods = 'SHIFT|SUPER',    action = act.ActivateTabRelative(1) },

        { key = 'LeftArrow',  mods = 'SHIFT|SUPER',    action = act.MoveTabRelative(-1) },
        { key = 'RightArrow', mods = 'SHIFT|SUPER',    action = act.MoveTabRelative(1) },

        { key = 'h',          mods = 'SHIFT|CTRL',     action = act.SplitVertical { domain = 'CurrentPaneDomain' } },
        { key = 'v',          mods = 'SHIFT|CTRL',     action = act.SplitHorizontal { domain = 'CurrentPaneDomain' } },

        { key = 'LeftArrow',  mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Left' },
        { key = 'RightArrow', mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Right' },
        { key = 'UpArrow',    mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Up' },
        { key = 'DownArrow',  mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Down' },

        { key = 'LeftArrow',  mods = 'SHIFT|ALT|CTRL', action = act.AdjustPaneSize { 'Left', 1 } },
        { key = 'RightArrow', mods = 'SHIFT|ALT|CTRL', action = act.AdjustPaneSize { 'Right', 1 } },
        { key = 'UpArrow',    mods = 'SHIFT|ALT|CTRL', action = act.AdjustPaneSize { 'Up', 1 } },
        { key = 'DownArrow',  mods = 'SHIFT|ALT|CTRL', action = act.AdjustPaneSize { 'Down', 1 } },

        { key = "LeftArrow",  mods = "OPT",            action = wezterm.action { SendString = "\x1bb" } },
        { key = "RightArrow", mods = "OPT",            action = wezterm.action { SendString = "\x1bf" } },

        { key = 'Enter',      mods = 'SUPER',          action = act.ToggleFullScreen },

        { key = '1',          mods = 'SUPER',          action = act.ActivateTab(0) },
        { key = '2',          mods = 'SUPER',          action = act.ActivateTab(1) },
        { key = '3',          mods = 'SUPER',          action = act.ActivateTab(2) },
        { key = '4',          mods = 'SUPER',          action = act.ActivateTab(3) },
        { key = '5',          mods = 'SUPER',          action = act.ActivateTab(4) },
        { key = '9',          mods = 'SUPER',          action = act.ActivateTab(-1) },

        { key = 'c',          mods = 'SUPER',          action = act.CopyTo 'Clipboard' },
        { key = 'v',          mods = 'SUPER',          action = act.PasteFrom 'Clipboard' },
        { key = 'f',          mods = 'SUPER',          action = act.Search 'CurrentSelectionOrEmptyString' },
        { key = 'Copy',       mods = 'NONE',           action = act.CopyTo 'Clipboard' },
        { key = 'Paste',      mods = 'NONE',           action = act.PasteFrom 'Clipboard' },

        { key = 'h',          mods = 'SUPER',          action = act.HideApplication },
        { key = 'k',          mods = 'SUPER',          action = act.ClearScrollback 'ScrollbackOnly' },
        { key = 'l',          mods = 'SUPER',          action = act.ShowDebugOverlay },
        { key = 'n',          mods = 'SUPER',          action = act.SpawnWindow },
        { key = 'p',          mods = 'SUPER',          action = act.ActivateCommandPalette },
        { key = 'q',          mods = 'SUPER',          action = act.QuitApplication },
        { key = 'r',          mods = 'SUPER',          action = act.ReloadConfiguration },

        { key = 't',          mods = 'SUPER',          action = act.SpawnTab 'CurrentPaneDomain' },
        { key = 'w',          mods = 'SUPER',          action = act.CloseCurrentTab { confirm = true } },

        { key = 'x',          mods = 'SUPER',          action = act.ActivateCopyMode },
    },

    key_tables = {
        copy_mode = {
            { key = 'Tab',        mods = 'NONE',  action = act.CopyMode 'MoveForwardWord' },
            { key = 'Tab',        mods = 'SHIFT', action = act.CopyMode 'MoveBackwardWord' },
            { key = 'Enter',      mods = 'NONE',  action = act.CopyMode 'MoveToStartOfNextLine' },
            { key = 'Escape',     mods = 'NONE',  action = act.CopyMode 'Close' },
            { key = 'Space',      mods = 'NONE',  action = act.CopyMode { SetSelectionMode = 'Cell' } },
            { key = '$',          mods = 'NONE',  action = act.CopyMode 'MoveToEndOfLineContent' },
            { key = '$',          mods = 'SHIFT', action = act.CopyMode 'MoveToEndOfLineContent' },
            { key = ',',          mods = 'NONE',  action = act.CopyMode 'JumpReverse' },
            { key = '0',          mods = 'NONE',  action = act.CopyMode 'MoveToStartOfLine' },
            { key = ';',          mods = 'NONE',  action = act.CopyMode 'JumpAgain' },
            { key = 'F',          mods = 'NONE',  action = act.CopyMode { JumpBackward = { prev_char = false } } },
            { key = 'F',          mods = 'SHIFT', action = act.CopyMode { JumpBackward = { prev_char = false } } },
            { key = 'G',          mods = 'NONE',  action = act.CopyMode 'MoveToScrollbackBottom' },
            { key = 'G',          mods = 'SHIFT', action = act.CopyMode 'MoveToScrollbackBottom' },
            { key = 'H',          mods = 'NONE',  action = act.CopyMode 'MoveToViewportTop' },
            { key = 'H',          mods = 'SHIFT', action = act.CopyMode 'MoveToViewportTop' },
            { key = 'L',          mods = 'NONE',  action = act.CopyMode 'MoveToViewportBottom' },
            { key = 'L',          mods = 'SHIFT', action = act.CopyMode 'MoveToViewportBottom' },
            { key = 'M',          mods = 'NONE',  action = act.CopyMode 'MoveToViewportMiddle' },
            { key = 'M',          mods = 'SHIFT', action = act.CopyMode 'MoveToViewportMiddle' },
            { key = 'O',          mods = 'NONE',  action = act.CopyMode 'MoveToSelectionOtherEndHoriz' },
            { key = 'O',          mods = 'SHIFT', action = act.CopyMode 'MoveToSelectionOtherEndHoriz' },
            { key = 'T',          mods = 'NONE',  action = act.CopyMode { JumpBackward = { prev_char = true } } },
            { key = 'T',          mods = 'SHIFT', action = act.CopyMode { JumpBackward = { prev_char = true } } },
            { key = 'V',          mods = 'NONE',  action = act.CopyMode { SetSelectionMode = 'Line' } },
            { key = 'V',          mods = 'SHIFT', action = act.CopyMode { SetSelectionMode = 'Line' } },
            { key = '^',          mods = 'NONE',  action = act.CopyMode 'MoveToStartOfLineContent' },
            { key = '^',          mods = 'SHIFT', action = act.CopyMode 'MoveToStartOfLineContent' },
            { key = 'b',          mods = 'NONE',  action = act.CopyMode 'MoveBackwardWord' },
            { key = 'b',          mods = 'ALT',   action = act.CopyMode 'MoveBackwardWord' },
            { key = 'b',          mods = 'CTRL',  action = act.CopyMode 'PageUp' },
            { key = 'c',          mods = 'CTRL',  action = act.CopyMode 'Close' },
            { key = 'd',          mods = 'CTRL',  action = act.CopyMode { MoveByPage = (0.5) } },
            { key = 'e',          mods = 'NONE',  action = act.CopyMode 'MoveForwardWordEnd' },
            { key = 'f',          mods = 'NONE',  action = act.CopyMode { JumpForward = { prev_char = false } } },
            { key = 'f',          mods = 'ALT',   action = act.CopyMode 'MoveForwardWord' },
            { key = 'f',          mods = 'CTRL',  action = act.CopyMode 'PageDown' },
            { key = 'g',          mods = 'NONE',  action = act.CopyMode 'MoveToScrollbackTop' },
            { key = 'g',          mods = 'CTRL',  action = act.CopyMode 'Close' },
            { key = 'h',          mods = 'NONE',  action = act.CopyMode 'MoveLeft' },
            { key = 'j',          mods = 'NONE',  action = act.CopyMode 'MoveDown' },
            { key = 'k',          mods = 'NONE',  action = act.CopyMode 'MoveUp' },
            { key = 'l',          mods = 'NONE',  action = act.CopyMode 'MoveRight' },
            { key = 'm',          mods = 'ALT',   action = act.CopyMode 'MoveToStartOfLineContent' },
            { key = 'o',          mods = 'NONE',  action = act.CopyMode 'MoveToSelectionOtherEnd' },
            { key = 'q',          mods = 'NONE',  action = act.CopyMode 'Close' },
            { key = 't',          mods = 'NONE',  action = act.CopyMode { JumpForward = { prev_char = true } } },
            { key = 'u',          mods = 'CTRL',  action = act.CopyMode { MoveByPage = (-0.5) } },
            { key = 'v',          mods = 'NONE',  action = act.CopyMode { SetSelectionMode = 'Cell' } },
            { key = 'v',          mods = 'CTRL',  action = act.CopyMode { SetSelectionMode = 'Block' } },
            { key = 'w',          mods = 'NONE',  action = act.CopyMode 'MoveForwardWord' },
            { key = 'y',          mods = 'NONE',  action = act.Multiple { { CopyTo = 'ClipboardAndPrimarySelection' }, { CopyMode = 'Close' } } },
            { key = 'PageUp',     mods = 'NONE',  action = act.CopyMode 'PageUp' },
            { key = 'PageDown',   mods = 'NONE',  action = act.CopyMode 'PageDown' },
            { key = 'End',        mods = 'NONE',  action = act.CopyMode 'MoveToEndOfLineContent' },
            { key = 'Home',       mods = 'NONE',  action = act.CopyMode 'MoveToStartOfLine' },
            { key = 'LeftArrow',  mods = 'NONE',  action = act.CopyMode 'MoveLeft' },
            { key = 'LeftArrow',  mods = 'ALT',   action = act.CopyMode 'MoveBackwardWord' },
            { key = 'RightArrow', mods = 'NONE',  action = act.CopyMode 'MoveRight' },
            { key = 'RightArrow', mods = 'ALT',   action = act.CopyMode 'MoveForwardWord' },
            { key = 'UpArrow',    mods = 'NONE',  action = act.CopyMode 'MoveUp' },
            { key = 'DownArrow',  mods = 'NONE',  action = act.CopyMode 'MoveDown' },
        },

        search_mode = {
            { key = 'Enter',     mods = 'NONE', action = act.CopyMode 'PriorMatch' },
            { key = 'Escape',    mods = 'NONE', action = act.CopyMode 'Close' },
            { key = 'n',         mods = 'CTRL', action = act.CopyMode 'NextMatch' },
            { key = 'p',         mods = 'CTRL', action = act.CopyMode 'PriorMatch' },
            { key = 'r',         mods = 'CTRL', action = act.CopyMode 'CycleMatchType' },
            { key = 'u',         mods = 'CTRL', action = act.CopyMode 'ClearPattern' },
            { key = 'PageUp',    mods = 'NONE', action = act.CopyMode 'PriorMatchPage' },
            { key = 'PageDown',  mods = 'NONE', action = act.CopyMode 'NextMatchPage' },
            { key = 'UpArrow',   mods = 'NONE', action = act.CopyMode 'PriorMatch' },
            { key = 'DownArrow', mods = 'NONE', action = act.CopyMode 'NextMatch' },
        },

    }
}
```

## [[Oh-My-ZSH Setup (Terminal)]]