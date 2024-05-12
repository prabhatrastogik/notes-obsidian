# FZF for file search / directory search and tab completion

## Setup

```zsh
brew install fzf fd rg

## Pager for git diff etc.
brew install git-delta

## Set up fzf key bindings and fuzzy completion
echo 'eval "$(fzf --zsh)"' >> ~/.zshrc

## Use fd for faster completions
echo 'export FZF_DEFAULT_COMMAND="fd --hidden --strip-cwd-prefix --exclude .git"' >> ~/.zshrc
echo 'export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"' >> ~/.zshrc

## Add to .zshrc
vim ~/.zshrc > G > o #paste below line at the end
export FZF_DEFAULT_OPTS='--height 40% --layout=reverse --border --preview "bat --color=always {}"'

echo '_fzf_compgen_path() {
  fd --hidden --follow --exclude ".git" . "$1"
}

# Use fd to generate the list for directory completion
_fzf_compgen_dir() {
  fd --type d --hidden --follow --exclude ".git" . "$1"
}' >> ~/.zshrc

## Setup fzf for github
cd ~ && git clone git@github.com:junegunn/fzf-git.sh.git
echo 'source $HOME/fzf-git.sh/fzf-git.sh' >> ~/.zshrc

## Config to add to ~/.gitignore for delta
[core]
    pager = delta

[interactive]
    diffFilter = delta --color-only

[delta]
    navigate = true     # use n and N to move between diff sections
    side-by-side = true # side by side for git
    line-numbers = true # Line numbers for both sides

    # delta detects terminal colors automatically; set one of these to disable auto-detection
    # dark = true
    # light = true

[merge]
    conflictstyle = diff3

[diff]
    colorMoved = default
```

## Usage
### Ctrl + T = Find files

- `nvim <C-t>` : to start fzf and open specific files
- `cat <C-t>` : fzf to search file to cat

Tab can be used for multiple selections; Ctrl + C to exit fzf search

### Opt + C = Find sub-directories and CD into it
- `<Opt-C>` : choose sub directory

### ** + TAB = Auto Completion with file search

```zsh
# Files under the current directory
# - You can select multiple items with TAB key
vim **<TAB>

# Files under parent directory
vim ../**<TAB>

# Files under parent directory that match `fzf`
vim ../fzf**<TAB>

# Files under your home directory
vim ~/**<TAB>

# Directories under current directory (single-selection)
cd **<TAB>

# Directories under ~/github that match `fzf`
cd ~/github/fzf**<TAB>

# Can select multiple processes with <TAB> or <Shift-TAB> keys
kill -9 **<TAB>

# Environment variables / Aliases
unset **<TAB>
export **<TAB>
unalias **<TAB>

# For ssh and telnet commands, fuzzy completion for hostnames is provided. The names are extracted from /etc/hosts and ~/.ssh/config.
ssh **<TAB>
telnet **<TAB>
```

### Git Addon
- CTRL-G CTRL-F for **F**iles
- CTRL-G CTRL-B for **B**ranches
- CTRL-G CTRL-T for **T**ags
- CTRL-G CTRL-R for **R**emotes
- CTRL-G CTRL-H for commit **H**ashes
- CTRL-G CTRL-S for **S**tashes
- CTRL-G CTRL-L for ref**l**ogs
- CTRL-G CTRL-W for **W**orktrees
- CTRL-G CTRL-E for **E**ach ref (`git for-each-ref`)






