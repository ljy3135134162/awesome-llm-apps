# AI 深度研究 Agent

这是一个深度研究助手，能够制定研究计划、搜索 Web、写入虚拟文件系统，并将每一次工具调用实时渲染为工作区中的卡片。项目基于 [CopilotKit](https://github.com/CopilotKit/CopilotKit)、[Deep Agents](https://docs.copilotkit.ai/integrations/langgraph/deep-agents)、[AG-UI](https://github.com/ag-ui-protocol/ag-ui) 和 [Tavily](https://www.tavily.com/) 构建，底层技术栈为 Next.js + LangGraph（Python）。

https://github.com/user-attachments/assets/68d5729f-91f9-4fd9-a579-cd1a8f4aad8d

**Gen UI 概念——工具渲染组件 + Sidecar 工作区。** Deep Agent 会调用四种工具：`write_todos`、`write_file`、`read_file` 和 `research`。每个工具调用都会在聊天区域内联渲染为状态卡片，同时更新旁边的工作区面板，包括研究计划、文件以及可展开的工具执行结果。前端使用 `useDefaultTool` 将 Agent 的文件系统同步到本地 React State，而不是使用 `useCoAgent`，从而避免 Python `Dict` 与 TypeScript `Array` 之间的类型不匹配问题。

## 环境要求

- Node.js 18+
- Python 3.12+
- [OpenAI API Key](https://platform.openai.com/api-keys)
- [Tavily API Key](https://app.tavily.com/home)
- 用于安装 Python 依赖的 [uv](https://docs.astral.sh/uv/)（也可以使用 pip）

## 开始使用

1. 安装 Node 依赖：

```bash
npm install
```

2. 安装 Agent 的 Python 依赖：

```bash
cd agent
uv venv && source .venv/bin/activate
uv pip install -e .
cd ..
```

或者使用 pip：

```bash
cd agent
python -m venv .venv && source .venv/bin/activate
pip install -e .
cd ..
```

3. 分别在项目根目录和 `agent/` 目录中，将 `.env.example` 复制为 `.env`，然后填写 `OPENAI_API_KEY` 和 `TAVILY_API_KEY`。

4. 启动 Agent（终端 1）：

```bash
cd agent
uv run python main.py
```

5. 启动前端（终端 2）：

```bash
npm run dev
```

打开 [http://localhost:3000](http://localhost:3000)，然后让助手研究任意主题即可。

## 架构

```text
[用户提出研究问题]
        ↓
Next.js 前端（CopilotChat + Workspace）
        ↓
CopilotKit Runtime → LangGraphHttpAgent
        ↓
Python 后端（FastAPI + AG-UI）
        ↓
Deep Agent（research_assistant）
    ├── write_todos        （规划，内置）
    ├── write_file         （文件系统，内置）
    ├── read_file          （文件系统，内置）
    └── research(query)
            └── 内部 Deep Agent [线程隔离]
                    └── internet_search（Tavily）
```

## 环境变量

| 变量 | 必需 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `OPENAI_API_KEY` | 是 | - | [获取 API Key](https://platform.openai.com/api-keys) |
| `TAVILY_API_KEY` | 是 | - | [获取 API Key](https://app.tavily.com/home) |
| `OPENAI_MODEL` | 否 | `gpt-5.5` | 使用的模型（`gpt-5.5`） |
| `LANGGRAPH_DEPLOYMENT_URL` | 否 | `http://localhost:8123` | 后端 URL |
| `SERVER_HOST` | 否 | `0.0.0.0` | 后端监听地址 |
| `SERVER_PORT` | 否 | `8123` | 后端端口 |

## 进一步了解

- [Deep Agents 文档](https://docs.copilotkit.ai/integrations/langgraph/deep-agents)
- [Building Frontends for Deep Agents](https://www.copilotkit.ai/blog/how-to-build-a-frontend-for-langchain-deep-agents-with-copilotkit)
- [CopilotKit 文档](https://docs.copilotkit.ai)
- [Tavily 文档](https://docs.tavily.com/welcome)

## 许可证

遵循上游项目许可证，详见 [`CopilotKit/CopilotKit`](https://github.com/CopilotKit/CopilotKit)。
