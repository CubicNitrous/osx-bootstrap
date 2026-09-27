# macOS Bootstrap

Bootstrap a clean macOS installation with Homebrew, mise, and the dotfiles in
this repository.

## Before you start

- Sign in to a macOS administrator account and connect to the internet.
- Install Apple's Command Line Tools if macOS hasn't installed them yet:
  `xcode-select --install`.

## Install

Open Terminal or Ghostty and run:

```sh
mkdir -p ~/workspace
cd ~/workspace
git clone https://github.com/CubicNitrous/osx-bootstrap.git
cd osx-bootstrap
./scripts/setup.sh
```

The script installs Homebrew if needed, then `mise run setup` installs the
packages and apps in `Brewfile`, links the managed dotfiles into your home
directory, installs configured mise tools, and applies macOS preferences.
You'll be prompted to confirm preference changes and enter your administrator
password for system-level settings.

Existing dotfiles are preserved as timestamped `.backup.*` files before
symlinks are created. For example, when cloned to `~/workspace/osx-bootstrap`,
`~/.zshrc` links to `~/workspace/osx-bootstrap/dotfiles/.zshrc`, and the
managed `~/.config/*` files link to their matching paths under
`~/workspace/osx-bootstrap/dotfiles/.config/`. Keep the repository at that
path: moving or deleting it breaks the links.

To point your home dotfiles at another clone, run that clone's
`./scripts/setup.sh`. Existing links or files at managed paths are moved to
timestamped `.backup.*` paths before the new links are created.

If mise cannot update Safari preferences because Terminal or Ghostty lacks
Full Disk Access, grant access in **System Settings → Privacy & Security →
Full Disk Access**, quit and reopen the terminal, then run `./scripts/setup.sh`
again.

## After setup

- Choose the timezone in **System Settings → General → Date & Time**.
- Open a new terminal session to load the linked zsh config, NVM, and mise.
- In a project with an `.nvmrc`, run `nvm install` to install and select its
  specified Node.js version.
- Check macOS preference drift with `mise bootstrap macos defaults status`.

Go is managed by mise in `dotfiles/.config/mise/config.toml`. Node.js is managed
per-project by NVM. Apps, CLI packages, and VS Code extensions are listed in
`Brewfile`; edit the files in `dotfiles/` to keep your home config synchronized.
