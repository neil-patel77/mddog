# Development Tool Kit — mddog

mddog is the first entry in the development tool kit. The kit lists the tools this fork's maintainer uses day to day, one file per tool.

## What mddog is

mddog is a terminal Markdown renderer. Instead of re-typing Markdown as terminal text, it renders the real page as images using the [kitty graphics protocol](https://sw.kovidgoyal.net/kitty/graphics-protocol/), in a single Rust binary with no browser and no Electron. It draws real fonts and heading sizes, diagrams and math as graphics, and images. Files are watched and re-rendered as they change, so live edits show up without a reload.

Upstream: [qu8n/mddog](https://github.com/qu8n/mddog), MIT licensed. Full docs at [mddog.pages.dev/docs](https://mddog.pages.dev/docs).

## Why it's in the tool kit

- True-page Markdown rendering right in the terminal for docs and runbooks: no browser, no Electron, no leaving the shell.
- It integrates with Claude Code through its plugin preview pane: the file opens beside the chat and refreshes as it changes, so the agent's edits are visible as they happen.

## Install

> **Requirements:** mddog runs on **macOS only**, and only in a terminal that supports the [kitty graphics protocol](https://sw.kovidgoyal.net/kitty/graphics-protocol/) — Ghostty, iTerm2, kitty, WezTerm, or Rex.

```sh
brew install qu8n/tap/mddog
```

Or, without Homebrew:

```sh
curl -fsSL https://raw.githubusercontent.com/qu8n/mddog/main/install.sh | sh
```

## Usage

```sh
mdd             # list Markdown files in the current directory to pick from
mdd README.md   # preview a file
```

Config lives at `~/.config/mddog/config.toml` (or `$XDG_CONFIG_HOME/mddog/config.toml` when `XDG_CONFIG_HOME` is set). The repo ships `config.example.toml` at its root; every value in it is a default, so only set the keys you change:

- `theme` — one built-in theme (`github-light`, `github-dark`, `obsidian-light`, `obsidian-dark`), a `light:`/`dark:` pair that follows the macOS appearance, or a path to a custom `.css` theme file.
- `copy_on_select` — copy a drag selection to the clipboard on release, instead of needing `Cmd-c`.

mddog reads the config only at startup, so restart it after a change. For writing your own theme, see [Themes](https://mddog.pages.dev/docs/themes).

## Claude Code plugin

Install the plugin, then ask for a file or run the command yourself:

```sh
claude plugin marketplace add qu8n/mddog && claude plugin install mddog@mddog
```

- Activate the pane with `/mddog <file.md>` or `/mddog @<file.md>`, or ask Claude to show you a file, e.g. "Show me the plan using mddog."
- The pane reloads when the file changes, so Claude's edits show as they land.
- To update: `claude plugin marketplace update mddog && claude plugin update mddog@mddog`, then start a new session or run `/reload-plugins`.

In the pane you can't click links or select text, and Claude Code shows images only in terminals it knows (Ghostty, iTerm2, kitty, WezTerm); in other terminals try `CLAUDE_CODE_FORCE_TERMINAL_IMAGES=1 claude`. Full details in the [Claude Code plugin docs](https://mddog.pages.dev/docs/claude-code).

## Homelab note

The dev-AI toolset runs on Linux: the Linux homelab hosts and their agent containers. mddog is macOS-only, so it serves the macOS workstation only — a known platform gap, not a blocker. On the Linux hosts, runbooks and docs are read with the usual terminal viewers; on the workstation the same files open as rendered pages.
