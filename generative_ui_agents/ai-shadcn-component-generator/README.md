# AI Shadcn 组件生成器

> 本项目移植自上游仓库 [CopilotKit/shadify](https://github.com/CopilotKit/shadify)。

用自然语言描述一个 UI，即可获得一个实时、可交互的 [shadcn/ui](https://ui.shadcn.com/) 组件，并可导出为干净的 React 代码。

https://github.com/user-attachments/assets/b14bebd6-527a-48bd-94f5-d27fea8808aa

**Gen UI 概念——Schema 驱动的组件组合。** 完整的 shadcn 组件 Schema 会作为 Agent 上下文传入，因此模型能够准确知道有哪些基础组件、每个组件支持哪些 Props，以及它们可以如何嵌套。Agent 的“输出”实际上是由这些基础组件组成的一棵结构化组件树，这棵树会流式发送到浏览器，并挂载为真正的 React 组件，同时还能导出为代码。也就是说，设计系统本身就是 Agent 的动作空间。

## 技术栈

- **[shadcn/ui](https://ui.shadcn.com/)** —— AI 会直接使用真实的 shadcn 组件进行组合，包括 Card、Chart、Form、Menu 和 Layout 等。每个生成结果都具备良好的可访问性和完整样式，并且使用的正是你可以通过 `npx shadcn add` 添加到自己项目中的基础组件。
- **[CopilotKit](https://github.com/CopilotKit/CopilotKit)** —— 将 Agent 生成的结构化 UI 实时流式传输到浏览器。同时会把完整组件 Schema 作为 Agent 上下文传入，让模型明确知道自己可以构建哪些内容。
- **[AG-UI](https://github.com/ag-ui-protocol/ag-ui)** —— Agent ↔ UI 协议，用于在 LangGraph 后端和 React 前端之间传递工具调用与组件指令。
- **[LangGraph](https://www.langchain.com/langgraph)** —— Agent 后端。负责推理、工具调用（Web Search、通过 Tavily 抓取网站内容）以及多轮对话记忆。
- **[Render](https://render.com/)** —— 三个服务都可以通过单个 `render.yaml` Blueprint 部署。Render 会通过 `fromService` 引用自动连接各服务 URL，只需推送到 `main` 分支即可上线。

## 架构

项目是一个包含三个服务的 pnpm monorepo：

```text
UI (React + Vite)  →  Runtime (Hono + CopilotKit)  →  Agent (FastAPI + LangGraph)
```

| 服务 | 路径 | 作用 |
|---|---|---|
| `ui` | `apps/ui` | 聊天界面、组件渲染、代码导出 |
| `runtime` | `apps/runtime` | CopilotKit Runtime，将消息路由到 Agent |
| `agent` | `apps/agent` | 带搜索工具的 LangGraph Agent，返回结构化 UI |

## 快速开始

```bash
pnpm install
```

添加 API Keys：

```bash
# apps/runtime/.env
OPENAI_API_KEY=sk-...

# apps/agent/.env
OPENAI_API_KEY=sk-...
TAVILY_API_KEY=tvly-...
```

然后启动：

```bash
pnpm dev
```

UI 默认运行在 [localhost:5173](http://localhost:5173)，Runtime 运行在 4000 端口，Agent 运行在 8123 端口。

## 部署

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy)

也可以直接连接你的仓库，`render.yaml` 已经定义好全部服务。
