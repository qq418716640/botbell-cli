[English](README.md) | [中文](README.zh-CN.md)

# botbell-cli

Send push notifications from the command line via [BotBell](https://botbell.app).

**Single file, zero dependencies** (just bash + curl + jq).

## Install

```bash
# Download
curl -fsSL https://raw.githubusercontent.com/qq418716640/botbell-cli/main/botbell -o /usr/local/bin/botbell
chmod +x /usr/local/bin/botbell

# Or clone
git clone https://github.com/qq418716640/botbell-cli.git
ln -s $(pwd)/botbell-cli/botbell /usr/local/bin/botbell
```

## Setup

```bash
export BOTBELL_TOKEN="bt_your_token"
```

Add to your `~/.bashrc` or `~/.zshrc` to persist.

## Usage

### Send notification

```bash
botbell send "Deploy succeeded ✅"

botbell send "Build report" --title "📊 CI" --format markdown

botbell send "New order" --title "Order #1234" --url "https://dashboard.example.com"
```

### Approval gate

```bash
# Blocks until user taps Approve or Reject
botbell approve "Deploy to production?"

# Custom timeout
botbell approve "Ship v2.1.0?" --timeout 300

# Custom actions
botbell approve "Release?" --actions '[{"key":"yes","label":"Ship it"},{"key":"no","label":"Not yet"}]'
```

### In CI/CD scripts

```bash
#!/bin/bash
set -e

make build
make test

# Approval gate — script stops here until approved
botbell approve "Deploy build #${BUILD_NUMBER} to prod?"

make deploy
botbell send "Deployed #${BUILD_NUMBER} ✅"
```

Works in any CI/CD: GitLab CI, CircleCI, Drone, Bitbucket Pipelines, local scripts, cron jobs.

## Commands

| Command | Description |
|---------|-------------|
| `botbell send <message>` | Send a push notification |
| `botbell approve <message>` | Send approval request and wait |
| `botbell help` | Show usage |
| `botbell version` | Show version |

## Options

| Option | Description |
|--------|-------------|
| `--title <text>` | Message title |
| `--url <url>` | Attached URL |
| `--image <url>` | Image URL |
| `--format <fmt>` | `"text"` or `"markdown"` |
| `--token <token>` | Bot token (default: `$BOTBELL_TOKEN`) |
| `--timeout <secs>` | Max wait seconds (default: 1800) |
| `--poll <secs>` | Poll interval (default: 5) |
| `--actions <json>` | Custom actions JSON array |

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success (or approved) |
| 1 | Error / rejected / timed out |

## License

MIT
