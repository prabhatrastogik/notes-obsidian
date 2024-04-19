# FZF for file search / directory search and tab completion

## Setup

```zsh
brew install fzf fd rg

## Set up fzf key bindings and fuzzy completion
echo 'eval "$(fzf --zsh)"' >> ~/.zshrc
echo 'export FZF_DEFAULT_COMMAND="fd --hidden --strip-cwd-prefix --exclude .git"' >> ~/.zshrc
echo 'export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"' >> ~/.zshrc
echo 'export FZF_DEFAULT_COMMAND="fd --hidden --strip-cwd-prefix --exclude .git"' >> ~/.zshrc
## Setup fzf got github
cd ~ && git clone git@github.com:junegunn/fzf-git.sh.git
echo 'source ~/fzf-git.sh/fzf-git.sh' >> ~/.zshrc

```

## Usage
### Ctrl + T = Find files

- `nvim <C-t>` : to start fzf and open specific files
- `cat <C-t>` : fzf to search file to cat

Tab can be used for multiple selections; Ctrl + C to exit fzf search

### Opt + C = Find sub-directories and CD into it
- `<Opt-C>` : choose sub directory

### ** + TAB = Auto Completion with file rearch

- `cd **<TAB>`: to choose directory
- `nvim **<TAB>` : to choose files
- `ssh **<TAB>`: to check past ssh connections
- `unalias **<TAB>`: to check out all aliases

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






