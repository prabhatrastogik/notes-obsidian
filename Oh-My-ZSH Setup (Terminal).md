---
tags:
- terminal
- zsh
---

## Installation

```zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## Configuration

Configure OMZ for basic plugins

```zsh
# touch ~/.zshrc  
  
# Syntax Highlighting plugin download  
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting  
  
# Auto-suggestions plugin download  
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions  
  
vim ~/.zshrc  
  
## inside .zshrc add this to plugins section  
plugins=(git python brew macos zsh-syntax-highlighting zsh-autosuggestions)