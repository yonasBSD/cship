# cship

**Beautiful, Blazing-fast, Customizable Claude Code Statusline.**

`cship` renders a live statusline for [Claude Code](https://claude.ai/code) sessions, showing session cost, context window usage, model name, API usage limits, and more — all configurable via a simple TOML file.

## Install

### Method 1: curl installer (recommended)

Auto-detects your OS and architecture (macOS arm64/x86_64, Linux x86_64/aarch64), downloads the binary to `~/.local/bin/cship`, creates a starter config at `~/.config/cship.toml`, and wires the `statusLine` entry in `~/.claude/settings.json`.

```sh
curl -fsSL https://raw.githubusercontent.com/stephenleo/cship/main/install.sh | bash
```

### Method 2: cargo install

Requires the Rust toolchain.

```sh
cargo install cship
```

After installing with `cargo`, wire the statusline manually in `~/.claude/settings.json`:

```json
{
  "statusLine": { "type": "command", "command": "cship" }
}
```

## Configuration

The default config file is `~/.config/cship.toml`. You can also place a `cship.toml` in your project root for per-project overrides. A minimal working example:

```toml
[cship]
lines = ["$cship.model $cship.cost $cship.context_bar"]
```

The `lines` array defines the rows of your statusline. Each element is a format string mixing `$cship.<module>` tokens (native cship modules) with Starship module tokens (e.g. `$git_branch`).

### Styling example

```toml
[cship]
lines = ["$cship.model $cship.cost $cship.context_bar"]

[cship.cost]
warn_threshold = 1.0
warn_style = "bold yellow"
critical_threshold = 5.0
critical_style = "bold red"
```

### Available modules

| Token | Description |
|-------|-------------|
| `$cship.model` | Claude model name |
| `$cship.cost` | Session cost in USD ($X.XX) |
| `$cship.context_bar` | Visual progress bar of context window usage |
| `$cship.context_window` | Context window tokens (used/total) |
| `$cship.usage_limits` | API usage limits (5hr / 7-day) |
| `$cship.vim` | Vim mode indicator |
| `$cship.agent` | Sub-agent name |
| `$cship.session` | Session identity info |
| `$cship.workspace` | Workspace/project directory |

Full configuration reference: **https://cship.dev**

## Debugging

Run `cship explain` to inspect what cship sees from Claude Code's context JSON — useful when a module shows nothing or behaves unexpectedly.

```sh
cship explain
```

## License

Apache-2.0
