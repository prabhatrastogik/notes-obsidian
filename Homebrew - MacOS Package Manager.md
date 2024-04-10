## Installation

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
## Commands

### Basic commands

| `brew install git`         | Install a package          |
| :------------------------- | -------------------------- |
| `brew uninstall git`       | Remove/Uninstall a package |
| `brew upgrade git`         | Upgrade a package          |
| `brew unlink git`          | Unlink                     |
| `brew link git`            | Link                       |
| `brew switch git 2.5.0`    | Change versions            |
| `brew list --versions git` | See what versions you have |

###  Package explore commands

| `brew info git`    | List versions, caveats, etc |
| ------------------ | --------------------------- |
| `brew cleanup git` | Remove old versions         |
| `brew edit git`    | Edit this formula           |
| `brew cat git`     | Print this formula          |
| `brew home git`    | Open homepage               |
| `brew search git`  | Search for formulas         |

### Global commands

| `brew update`   | Update brew and cask     |
| --------------- | ------------------------ |
| `brew upgrade`  | Upgrade all packages     |
| `brew list`     | List installed           |
| `brew outdated` | What’s due for upgrades? |
| `brew doctor`   | Diagnose brew issues     |

### Cask commands

Cask commands are used for installing and managing graphical applications.

| `brew install --cask firefox` | Install the Firefox browser |
| ----------------------------- | --------------------------- |
| `brew list --cask`            | List installed applications |

### Taps / 3rd party repos

| `brew tap`                       | List all the current tapped repositories (taps) |
| -------------------------------- | ----------------------------------------------- |
| `brew tap homebrew/cask-fonts`   | Add a new repo - cask-fonts                     |
| `brew untap homebrew/cask-fonts` | Remove repo - cask-fonts from brew              |
