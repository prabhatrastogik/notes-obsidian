# Get Package Manager - HomeBrew

Homebrew is a package manager for macOS that simplifies the installation of software.

[[Homebrew - MacOS Package Manager]]

# RayCast Setup

RayCast is a powerful productivity tool for macOS that allows you to manage your windows, applications, and scripts efficiently.

To install RayCast, run:

```zsh
brew install --cask raycast
```

### Things to Set Up (Access with `⌘ + ,`)
- **Window Management:** Configure hotkeys. Use the preset for Magnet to manage window layouts.
- **Navigation:** Set a hotkey for "switch windows" (`⌥ + ↹`).
- **Aliases:**
  - Set alias for "file search" as `f`.
  - Set alias for "Calendar" as `c`.

### Install Extensions
- `brew`: Install the Homebrew extension.

# Terminal Setup

Set up your terminal with WezTerm and Oh-My-ZSH.

[[Wezterm - Terminal Setup]]
[[Oh-My-ZSH Setup (Terminal)]]

# Tools Installation

Use Homebrew to install the following tools:

```zsh
brew install pyenv           # Python Version Manager
brew install n               # Node Version Manager. Usage: `n latest` to install the latest stable version.
brew install awscli          # AWS Command Line Interface
brew install git             # Version control system
brew install fzf             # Fuzzy file finder
brew install fd              # File search tool (useful in vim extensions)
brew install rg              # ripgrep, for live_grep in nvim
brew install bat             # Enhanced version of "cat"
brew install git-delta       # Improves `git diff` output
brew install tree            # File viewer on terminal (usage: tree)
brew install tree-sitter     # Parser generator tool and an incremental parsing library
```

### Editor Installation

Install Neovim, an extensible and highly configurable text editor:

```zsh
brew install neovim
```

### Cask Installations

Install graphical applications using Homebrew casks:

```zsh
brew install --cask google-chrome
brew install --cask visual-studio-code
```

### Configuration

#### For pyenv in .zshrc

Add the following line to your `.zshrc` file to initialize pyenv:

```zsh
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

#### Add Aliases in .zshrc

Add the following aliases to your `.zshrc` file for convenience:

```zsh
alias cat="bat -pp"
alias vim=nvim
```

# Additional Setup

- [[Setup CLI tools - fzf, delta]]
- [[Github Setup - Laptop]]