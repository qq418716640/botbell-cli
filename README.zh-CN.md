[English](README.md) | [中文](README.zh-CN.md)

# botbell-cli

通过 [BotBell](https://botbell.app) 从命令行发送推送通知。

**单文件，零依赖**（仅需 bash + curl + jq）。

## 安装

```bash
# 下载
curl -fsSL https://raw.githubusercontent.com/qq418716640/botbell-cli/main/botbell -o /usr/local/bin/botbell
chmod +x /usr/local/bin/botbell

# 或克隆仓库
git clone https://github.com/qq418716640/botbell-cli.git
ln -s $(pwd)/botbell-cli/botbell /usr/local/bin/botbell
```

## 配置

```bash
export BOTBELL_TOKEN="bt_your_token"
```

添加到 `~/.bashrc` 或 `~/.zshrc` 以持久化。

## 使用

### 发送通知

```bash
botbell send "部署成功 ✅"

botbell send "构建报告" --title "📊 CI" --format markdown

botbell send "新订单" --title "订单 #1234" --url "https://dashboard.example.com"
```

### 审批门控

```bash
# 阻塞直到用户点击批准或拒绝
botbell approve "部署到生产环境？"

# 自定义超时
botbell approve "发布 v2.1.0？" --timeout 300

# 自定义 Actions
botbell approve "发布？" --actions '[{"key":"yes","label":"发布"},{"key":"no","label":"再等等"}]'
```

### 在 CI/CD 脚本中使用

```bash
#!/bin/bash
set -e

make build
make test

# 审批门控 — 脚本在此等待直到被批准
botbell approve "将构建 #${BUILD_NUMBER} 部署到生产环境？"

make deploy
botbell send "已部署 #${BUILD_NUMBER} ✅"
```

适用于任何 CI/CD：GitLab CI、CircleCI、Drone、Bitbucket Pipelines、本地脚本、Cron 任务。

## 命令

| 命令 | 说明 |
|------|------|
| `botbell send <message>` | 发送推送通知 |
| `botbell approve <message>` | 发送审批请求并等待 |
| `botbell help` | 显示帮助 |
| `botbell version` | 显示版本 |

## 选项

| 选项 | 说明 |
|------|------|
| `--title <text>` | 消息标题 |
| `--url <url>` | 附加 URL |
| `--image <url>` | 图片 URL |
| `--format <fmt>` | `"text"` 或 `"markdown"` |
| `--token <token>` | Bot Token（默认：`$BOTBELL_TOKEN`） |
| `--timeout <secs>` | 最大等待秒数（默认：1800） |
| `--poll <secs>` | 轮询间隔秒数（默认：5） |
| `--actions <json>` | 自定义 Actions JSON 数组 |

## 退出码

| 退出码 | 含义 |
|--------|------|
| 0 | 成功（或已批准） |
| 1 | 错误 / 已拒绝 / 超时 |

## 许可证

MIT
