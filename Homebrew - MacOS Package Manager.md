## Installation

To install Homebrew, run the following command in your terminal. This command uses `curl` to fetch and execute the Homebrew installation script from the official repository:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## Commands

### Basic Commands

These commands are used for basic package management with Homebrew:

| Command                        | Description                           |
|-------------------------------|---------------------------------------|
| `brew install git`            | Install the specified package (e.g., Git). |
| `brew uninstall git`          | Remove the specified package.        |
| `brew upgrade git`            | Upgrade the specified package to its latest version. |
| `brew unlink git`             | Unlink the specified package, making it unavailable. |
| `brew link git`               | Link the specified package, making it available again. |
| `brew switch git 2.5.0`       | Change to a specific version of the package. |
| `brew list --versions git`    | List all installed versions of the specified package. |

### Package Exploration Commands

These commands allow you to explore package details:

| Command                        | Description                           |
|-------------------------------|---------------------------------------|
| `brew info git`               | Display information about the package, including versions and caveats. |
| `brew cleanup git`            | Remove old versions of the specified package to free up space. |
| `brew edit git`               | Open the formula for the specified package in your editor for editing. |
| `brew cat git`                | Print the formula for the specified package in the terminal. |
| `brew home git`               | Open the homepage of the specified package in your default web browser. |
| `brew search git`             | Search for packages (formulas) related to the specified query. |

### Global Commands

These commands perform global operations on Homebrew:

| Command                        | Description                           |
|-------------------------------|---------------------------------------|
| `brew update`                 | Update Homebrew and all installed casks to the latest versions. |
| `brew upgrade`                | Upgrade all installed packages to their latest versions. |
| `brew list`                   | List all installed packages.         |
| `brew outdated`               | Show which installed packages have newer versions available. |
| `brew doctor`                 | Diagnose potential issues with your Homebrew installation. |

### Cask Commands

Cask commands are specifically for managing graphical applications on macOS:

| Command                             | Description                           |
|-------------------------------------|---------------------------------------|
| `brew install --cask firefox`      | Install the Firefox browser.         |
| `brew list --cask`                 | List all installed graphical applications. |

### Taps / Third-Party Repositories

These commands manage additional repositories (taps) for Homebrew:

| Command                          | Description                                                 |
| -------------------------------- | ----------------------------------------------------------- |
| `brew tap`                       | List all currently tapped repositories (third-party repos). |
| `brew tap homebrew/cask-fonts`   | Add the `cask-fonts` repository for font management.        |
| `brew untap homebrew/cask-fonts` | Remove the `cask-fonts` repository from Homebrew.           |