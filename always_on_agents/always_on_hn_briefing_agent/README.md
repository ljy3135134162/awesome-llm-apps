# 📰 常驻 Hacker News 简报 Agent

AgentScout 是一个基于 Google ADK 构建的常驻 Hacker News 简报 Agent。它会持续扫描 Hacker News 中与 AI Agent、MCP、编程 Agent、工作流自动化和 LLM 应用相关的高价值内容，并将最值得关注的链接整理成简洁的工程简报。

该应用既可以作为交互式 ADK Agent 运行，也可以作为定时后端服务运行。你可以通过 ADK Web 手动请求简报，也可以运行 FastAPI 调度接口，让 Cloud Scheduler 每天触发一次 Hacker News 简报，并通过 Gmail、Slack、Linear、Jira 或内部摘要工作流发送出去。

![常驻 Hacker News 简报 Agent 架构](assets/always-on-hn-briefing-agent.png)

## 功能特性

- **Hacker News 监控**：查找与 AI Agent、MCP、编程 Agent、自动化和 LLM 应用相关的 Hacker News 内容。
- **信号排序**：根据相关性、积分、评论数以及首页位置对内容进行评分。
- **简报生成**：生成干净的纯文本和 HTML 简报，包含摘要、链接和后续行动建议。
- **Google ADK Agent**：暴露 `root_agent`，用户可直接在 ADK Web 中请求简报。
- **支持调度的后端**：提供 HTTP 和 Pub/Sub 接口，可用于 Cloud Scheduler 或其他自动化系统。
- **Gmail 与 Webhook 投递**：当 `dry_run=false` 时，可通过 Gmail API 或通用 Webhook 发送简报。
- **安全投递流程**：默认启用 Dry Run，除非显式配置凭据，否则不会实际发送内容。

## 工作原理

1. AgentScout 从确定性的示例数据或 Hacker News 实时首页采集内容。
2. 过滤出 AI Agent 和 LLM 应用相关主题。
3. 对最适合工程师和产品开发者阅读的内容进行排序。
4. 生成文本版和 HTML 版每日简报。
5. 通过 ADK Web、HTTP Trigger 或 Pub/Sub Push Endpoint 返回结果。
6. 如果启用了投递，Scheduler API 会通过 Gmail 发送简报，或将其 POST 到 `AGENTSCOUT_WEBHOOK_URL`。

## 环境要求

- Python 3.10+
- 用于 ADK Web 的 Gemini API Key
- 可选：用于邮件直接投递的 Gmail OAuth 凭据
- 可选：用于 Slack、Linear、Jira、GitHub Issues、SendGrid 或内部工作流的 Webhook URL

## 安装

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/always_on_agents/always_on_hn_briefing_agent
pip install -r requirements.txt
export GOOGLE_API_KEY="your_gemini_api_key"
```

## 方式 1：在 ADK Web 中运行

如果希望与 Agent 对话并手动请求简报，可以使用 ADK Web。

```bash
adk web .
```

打开 ADK Web UI，然后选择 `always_on_hn_briefing_agent`。

可尝试以下提示词：

```text
给我今天的 AgentScout 简报。
```

```text
帮我筛选 Hacker News 上最重要的 3 条 AI Agent 和 LLM 应用新闻。
```

```text
找出 Hacker News 上与 MCP、编程 Agent 和工作流自动化有关的最高价值内容。
```

## 方式 2：在本地运行 Scheduler API

如果希望 AgentScout 像常驻后端服务一样运行，可以使用 Scheduler API。该接口也可以部署到 Cloud Run，并由 Cloud Scheduler 触发。

启动 Scheduler Backend：

```bash
uvicorn scheduler_api:app --host 0.0.0.0 --port 8000
```

在另一个终端中预览一次计划任务，但不执行实际投递：

```bash
curl "http://127.0.0.1:8000/agent-scout/dry-run?top_n=3&live=false"
```

以 Dry Run 模式触发调度接口：

```bash
curl -X POST "http://127.0.0.1:8000/agent-scout/trigger" \
  -H "Content-Type: application/json" \
  -d '{"dry_run": true, "top_n": 5, "live": false}'
```

Dry Run 会返回生成后的简报以及投递状态，但不会真正发送任何内容。

为当前进程启用实时 Hacker News 扫描：

```bash
export AGENTSCOUT_LIVE_HN=true
```

也可以在单次请求中覆盖实时模式：

```bash
curl -X POST "http://127.0.0.1:8000/agent-scout/trigger" \
  -H "Content-Type: application/json" \
  -d '{"dry_run": true, "top_n": 5, "live": true}'
```

## 方式 3：启用定时投递

投递功能默认关闭。只有当请求体中包含 `"dry_run": false`，并且至少配置了一种投递方式时，AgentScout 才会真正发送邮件或调用 Webhook。

投递模式说明：

- `AGENTSCOUT_DELIVERY=gmail`：通过 Gmail API 发送。
- `AGENTSCOUT_DELIVERY=webhook`：POST 到 `AGENTSCOUT_WEBHOOK_URL`。
- 如果未设置 `AGENTSCOUT_DELIVERY`，当 Gmail 配置完整时优先使用 Gmail；否则如果配置了 Webhook URL，则使用 Webhook。

### Gmail 投递

如果希望 AgentScout 将每日简报直接发送到邮箱，可以使用 Gmail。创建具有 Gmail API 权限的 Google OAuth Client，并使用 `https://www.googleapis.com/auth/gmail.send` Scope 生成 Refresh Token，然后设置：

```bash
export AGENTSCOUT_DELIVERY="gmail"
export AGENTSCOUT_EMAIL_TO="you@example.com"
export AGENTSCOUT_EMAIL_FROM="you@example.com"
export AGENTSCOUT_GMAIL_CLIENT_ID="your_google_oauth_client_id"
export AGENTSCOUT_GMAIL_CLIENT_SECRET="your_google_oauth_client_secret"
export AGENTSCOUT_GMAIL_REFRESH_TOKEN="your_gmail_refresh_token"

curl -X POST "http://127.0.0.1:8000/agent-scout/trigger" \
  -H "Content-Type: application/json" \
  -d '{"dry_run": false, "top_n": 5, "live": true}'
```

AgentScout 会发送一封 Multipart 邮件，其中同时包含纯文本和 HTML 版本的简报。

### Webhook 投递

如果希望将简报发送到 Slack、Linear、Jira、GitHub Issues、SendGrid 或内部工作流，可以使用 Webhook。

```bash
export AGENTSCOUT_DELIVERY="webhook"
export AGENTSCOUT_WEBHOOK_URL="https://example.com/agent-brief-webhook"
export AGENTSCOUT_WEBHOOK_TOKEN="optional_bearer_token"

curl -X POST "http://127.0.0.1:8000/agent-scout/trigger" \
  -H "Content-Type: application/json" \
  -d '{"dry_run": false, "top_n": 5, "live": true}'
```

Webhook 会收到以下字段：`subject`、`text`、`html`、`stories` 和 `next_actions`。

## Cloud Scheduler 接口

将 Scheduler API 部署到 Cloud Run 或其他 HTTP 服务，在目标环境中配置好 Gmail 或 Webhook 投递后，就可以让 Cloud Scheduler 调用以下接口之一。

直接 HTTP Trigger：

```text
https://YOUR_CLOUD_RUN_URL/agent-scout/trigger
```

请求体：

```json
{
  "dry_run": false,
  "top_n": 5,
  "live": true
}
```

测试调度时应将 `dry_run` 设置为 `true`。只有在 Gmail 或 Webhook 已正确配置后，再将其改为 `false`。

推荐的工作日简报计划：

```text
0 9 * * 1-5
```

Pub/Sub Push Endpoint：

```text
https://YOUR_CLOUD_RUN_URL/agent-scout/pubsub
```

使用 Pub/Sub Push 时，需要将相同的 JSON Payload 编码为 Base64 后放入消息数据中。

## 输出示例

```json
{
  "subject": "AgentScout Hacker News brief - 2026-06-08",
  "watch_mode": "sample",
  "stories": [
    {
      "title": "Show HN: An open-source framework for reliable AI agent workflows",
      "points": 428,
      "comments": 116,
      "summary": "围绕 Agent 编排、重试、状态管理和工具执行等实际取舍展开的框架讨论。"
    }
  ],
  "next_actions": [
    "打开评论数最高的讨论帖，提取其中的反对意见或实现模式。"
  ]
}
```
