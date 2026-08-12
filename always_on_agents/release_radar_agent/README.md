# 📡 Release Radar Agent：依赖版本雷达智能体

Release Radar 是一个基于 Google ADK 构建的常驻型依赖更新简报 Agent。它会读取 `requirements.txt` 或 `package.json`，将已识别的依赖映射到对应的 GitHub 仓库，并只报告真正需要关注的变化：破坏性变更、弃用、安全修复、撤回版本以及大版本升级。

该应用既可以在 ADK Web 中交互运行，也可以作为定时 FastAPI 服务运行。示例模式使用确定性数据，不会发起 GitHub 请求。定时投递默认处于 dry-run 模式，只有在配置 Gmail 或 Webhook 后才会真正发送。

## 功能特性

- **依赖清单解析**：读取 Python `requirements.txt`，以及 npm 的运行时依赖和开发依赖。
- **GitHub Release 扫描**：使用 GitHub REST API 获取版本发布信息，可选配置 Token。
- **影响等级排序**：对安全问题、破坏性变更、撤回版本、大版本升级和弃用信息进行评分。
- **补丁噪声过滤**：自动过滤没有明显影响信号的普通 Patch 和 Minor 版本更新。
- **文本与 HTML 简报**：按依赖分组展示版本变化、更新原因、影响程度和 Release 链接。
- **Google ADK Agent**：暴露 `root_agent`，支持交互式询问依赖更新情况。
- **定时任务接口**：支持直接 HTTP 和 Pub/Sub 触发。
- **显式启用投递**：仅在明确配置后，才支持通过 Gmail 或通用 Webhook 发送简报。

## 工作原理

1. `radar.py` 解析依赖清单，并把已知包或 GitHub 依赖 URL 映射到对应仓库。
2. 示例模式加载固定的 GitHub 风格数据；实时模式会获取最近的 GitHub Releases。
3. `ranker.py` 对 Release Notes 进行分类，并过滤常规更新噪声。
4. `delivery.py` 将筛选后的版本变化渲染为文本和 HTML。
5. ADK Web 用于交互式返回简报，`scheduler_api.py` 提供定时 HTTP 与 Pub/Sub 接口。
6. 只有在 `dry_run=false` 且 Gmail 或 Webhook 配置完整时，Scheduler 才会真正发送简报。

源码模块主要使用 Python 标准库完成依赖解析、GitHub 访问、渲染与投递。FastAPI 用于提供 Scheduler API，Google ADK 则负责交互式 Agent 能力。

## 环境要求

- Python 3.10+
- 用于 ADK Web 的 Gemini API Key
- 实时扫描时需要项目中的 `requirements.txt` 或 `package.json`
- 可选 GitHub Token，用于提高 API Rate Limit
- 可选 Gmail OAuth 凭证或 Webhook URL，用于投递简报

## 安装

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/always_on_agents/release_radar_agent
pip install -r requirements.txt
export GOOGLE_API_KEY="your_gemini_api_key"
```

## 在 ADK Web 中运行

默认先使用确定性示例数据运行：

```bash
adk web .
```

打开 ADK Web，选择 `release_radar_agent`，然后可以尝试：

```text
给我今天的依赖版本更新简报。
```

如果希望扫描真实项目，请先设置依赖清单的绝对路径，并启用实时 GitHub 请求：

```bash
export RELEASE_RADAR_MANIFEST="/absolute/path/to/requirements.txt"
export RELEASE_RADAR_LIVE_GITHUB=true
export RELEASE_RADAR_GITHUB_TOKEN="optional_github_token"
adk web .
```

Release Radar 可以识别直接指向 GitHub 的依赖 URL，以及常见包，例如 Pydantic、FastAPI、Requests、OpenAI、Anthropic、Google ADK、LangChain、React、Vite 和 Zod。未映射的依赖不会导致扫描失败，而是会被列在扫描说明中。

## 运行 Scheduler API

在服务环境中设置项目依赖清单，然后从当前目录启动后端：

```bash
export RELEASE_RADAR_MANIFEST="/workspace/requirements.txt"
export RELEASE_RADAR_LIVE_GITHUB=true
uvicorn scheduler_api:app --host 0.0.0.0 --port 8000
```

预览确定性示例简报，不执行投递：

```bash
curl "http://127.0.0.1:8000/release-radar/dry-run?top_n=5&live=false"
```

通过 Scheduler 路径扫描真实依赖，同时保持关闭投递：

```bash
curl -X POST "http://127.0.0.1:8000/release-radar/trigger" \
  -H "Content-Type: application/json" \
  -d '{"dry_run": true, "live": true, "top_n": 10}'
```

服务端只会从 `RELEASE_RADAR_MANIFEST` 读取依赖清单，因此请求体无法指定任意本地文件。该路径必须真实存在于 Scheduler 服务环境中。如果需要扫描大量仓库，可以在服务环境中设置 `RELEASE_RADAR_GITHUB_TOKEN`。

## 启用投递

投递存在两层独立保护。请求必须设置 `"dry_run": false`，并且至少完整配置一个投递提供方。否则 API 会返回 skipped 状态，并且不会发送任何内容。

### Gmail

创建具有 Gmail API 权限的 Google OAuth Client，并使用 `https://www.googleapis.com/auth/gmail.send` Scope 生成 Refresh Token，然后设置：

```bash
export RELEASE_RADAR_DELIVERY="gmail"
export RELEASE_RADAR_EMAIL_TO="you@example.com"
export RELEASE_RADAR_EMAIL_FROM="you@example.com"
export RELEASE_RADAR_GMAIL_CLIENT_ID="your_google_oauth_client_id"
export RELEASE_RADAR_GMAIL_CLIENT_SECRET="your_google_oauth_client_secret"
export RELEASE_RADAR_GMAIL_REFRESH_TOKEN="your_gmail_refresh_token"
```

### Webhook

如果希望将依赖简报发送到 Slack、Linear、Jira 或内部工作流，可以使用 Webhook：

```bash
export RELEASE_RADAR_DELIVERY="webhook"
export RELEASE_RADAR_WEBHOOK_URL="https://example.com/dependency-brief"
export RELEASE_RADAR_WEBHOOK_TOKEN="optional_bearer_token"
```

配置完成后，可以触发实时投递：

```bash
curl -X POST "http://127.0.0.1:8000/release-radar/trigger" \
  -H "Content-Type: application/json" \
  -d '{"dry_run": false, "live": true, "top_n": 10}'
```

## Cloud Scheduler

将 FastAPI 服务部署到 Cloud Run 或其他 HTTP 服务后，Cloud Scheduler 可以直接调用以下接口：

```text
https://YOUR_SERVICE_URL/release-radar/trigger
```

也可以将 Base64 编码后的 JSON 发布到 Pub/Sub Push Endpoint：

```text
https://YOUR_SERVICE_URL/release-radar/pubsub
```

推荐的工作日早晨执行计划：

```text
0 9 * * 1-5
```

在验证部署期间，请保持 `dry_run=true`。

## 测试

```bash
python3 -m pytest always_on_agents/release_radar_agent/tests/unit -q
```

测试使用确定性的示例 Release 数据，并在验证投递安全性时对网络访问进行 Patch。

本应用继承仓库根目录中的 Apache-2.0 许可证。
