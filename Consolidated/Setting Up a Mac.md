# Get Package Manager - HomeBrew

[[Homebrew - MacOS Package Manager]]

# RayCast Setup

```zsh
brew install --cask raycast
```

Things to set up (`⌘ + ,`)
- Window Management (Hotkeys - Preset -> Magnet)
- Navigation (Set Hotkey for "switch windows" `⌥ + ↹`)
- Alias for "file search" `f`
- Alias for "Calendar" `c`
Install Extensions
- brew

# Terminal Setup 

[[Wezterm - Terminal Setup]]
[[Oh-My-ZSH Setup (Terminal)]]

# Tools Installation

```zsh
brew install pyenv # Python Version Manager
brew install n   # Node Version Manager. Usage" `n latest` to installed latest stable version 
brew install awscli
brew install git
brew install fzf   # Fuzzy file finder
brew install fd    # File search tool (useful in vim extensions)
brew install bat   # "cat" replacement on steriods
brew install diff-so-fancy # Dramatically improves `git diff` output
brew install tree  # Awesome file viewer on terminal [usage: tree]
brew install tree-sitter

# Editor
brew install neovim

# Casks
brew install --cask google-chrome
brew install --cask visual-studio-code

# Config for pyenv in .zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc

# Add aliases in .zshrc
alias cat=bat
alias vim=neovim
```

[[Github Setup - Laptop]]