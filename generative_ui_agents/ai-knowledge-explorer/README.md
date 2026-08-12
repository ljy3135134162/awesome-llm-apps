# AI Knowledge Explorer

把文件——文档或源代码——直接拖进聊天应用。Agent 会提取实体、概念和关系；对于代码，则提取模块、类、函数和依赖关系，然后渲染成一张可交互探索的知识图谱。

点击节点可以查看详细信息；双击节点可以继续展开，Agent 会提取子概念（代码场景下则是子组件）并加入图谱。你也可以直接在聊天中提问来导航整个知识图谱。

![Demo](./demo.gif)

## 工作原理

1. **拖入文件** —— 将文档（`.txt`、`.md`、`.json`、`.csv`）或代码文件（`.py`、`.ts`、`.js`、`.java`、`.go`、`.rs` 等）拖到 Canvas 中
2. **Agent 提取结构** —— LLM 会识别文件中的结构：文本中的实体和概念，代码中的模块和函数
3. **渲染图谱** —— Agent 每处理一个文件，节点和边都会逐步出现在界面中
4. **交互探索** —— 点击节点、展开节点、提问，并通过聊天引导 Agent 继续分析

## 架构

- **共享状态**：知识图谱（节点 + 边）保存在 Agent State 中，并通过 CopilotKit v2 与前端双向同步
- **Generative UI**：每次工具调用都会产生可见的 UI 变化，例如新增节点、新增边或展开详细信息
- **Human-in-the-loop**：点击进行选择，双击进行展开，通过聊天控制 Agent 的探索方向

### 技术栈

| 层级 | 技术 |
|---|---|
| 前端 | Next.js 16、React 19、TailwindCSS 4 |
| Agent | LangGraph（Python）、CopilotKit Middleware |
| 图谱 | react-force-graph-2d |
| LLM | OpenAI（可通过环境变量配置） |
| 协议 | AG-UI（状态流式同步） |

**关于默认模型（`gpt-4o`）**：提取流程（`extract_knowledge`、`find_connections`、`expand_node`）通过 Prompt 要求模型返回原始 JSON，然后由程序解析。当前没有专门针对 gpt-4o 配置 Function Calling Schema 或 Structured Output 模式，因此只要模型具备较好的指令遵循能力，通常都可以通过 `OPENAI_MODEL` 直接替换。选择 gpt-4o 是因为它作为一个成熟、成本较低的基线模型，通常能够稳定遵守“只返回 JSON”的指令，并不是因为它在此任务上与更新模型做过 Benchmark 对比。如果替换成更新或更偏推理型的模型，理论上也能工作，但需要重新确认模型是否仍然严格遵守“ONLY valid JSON”要求，因为部分模型即使被要求只输出 JSON，也可能附带说明文本或 Markdown 包装。

## 环境要求

- Node.js 18+
- Python 3.12
- [uv](https://docs.astral.sh/uv/) —— Agent 的 Python 依赖通过 uv 管理，执行 `npm install` 时会自动运行 `uv sync`。安装方式：macOS/Linux 可运行 `curl -LsSf https://astral.sh/uv/install.sh | sh`，Windows 可运行 `powershell -c "irm https://astral.sh/uv/install.ps1 | iex"`。如果本机尚未安装 Python 3.12，uv 会自动获取对应版本。

## 配置与启动

```bash
# 1. 安装依赖（同时会为 Python Agent 执行 `uv sync`）
npm install

# 2. 配置 API Key
cp .env.example .env
# 编辑 .env 并添加 OPENAI_API_KEY

# 3. 启动应用
npm run dev
```

该命令会同时启动 Next.js 前端（端口 3000）和 LangGraph Agent（端口 8125）。在 Windows 上，相同的 `npm` 脚本会自动调用 `scripts/` 下对应的 `.bat` 文件。

## 示例内容

项目内置两组示例，可分别体验文档模式和代码模式。在空白状态页面点击对应按钮即可立即加载。

**Documents** —— 3 个关于 AI Agent 的 Markdown 文件：
- `what-are-agents.md` —— 介绍 Agent 及核心组成部分（LLM、工具、记忆、规划）
- `agent-frameworks.md` —— 对比 LangGraph、CrewAI、AutoGen、CopilotKit
- `agent-challenges.md` —— 介绍幻觉、工具可靠性、评测、成本与安全问题

预期图谱规模：约 15–22 个节点、20–33 条边，覆盖 AI Agent 生态中的主要概念。

**Codebase** —— 3 个组成 FastAPI 认证系统的 Python 文件：
- `auth.py` —— JWT Token 创建、密码哈希、`TokenService` 类
- `routes.py` —— 登录、注册、刷新 Token Endpoint 与依赖注入
- `models.py` —— SQLAlchemy 的 `User`、`Post`、`AuditLog` 模型

预期图谱规模：约 20 个节点，用于展示模块、类、函数，以及它们之间的 Import / Call / Extend 关系。

## Agent 工具

| 工具 | 用途 |
|---|---|
| `extract_knowledge` | 解析文档或代码，提取实体、概念、关系，或模块、类、函数 |
| `find_connections` | 发现已有节点之间更深层的关联 |
| `expand_node` | 深入分析某个节点，并添加子概念和详细信息 |

## 项目结构

```
ai-knowledge-explorer/
├── agent/                    # Python LangGraph Agent
│   ├── main.py               # Agent 入口
│   └── src/
│       ├── state.py          # KnowledgeState Schema
│       └── tools.py          # extract / connect / expand 工具
├── src/                      # Next.js 前端
│   ├── app/
│   │   ├── page.tsx          # 主页面（聊天 + 图谱 Canvas）
│   │   ├── layout.tsx        # CopilotKit v2 Provider
│   │   └── api/
│   │       ├── copilotkit/   # CopilotKit Runtime Route
│   │       └── upload/       # 文件上传 Endpoint
│   ├── components/
│   │   ├── KnowledgeGraph.tsx # 力导向图可视化
│   │   ├── NodeDetail.tsx     # 节点选择后的详情面板
│   │   └── ToolReasoning.tsx  # 工具调用状态指示器
│   ├── hooks/
│   │   ├── use-knowledge-ui.tsx
│   │   └── use-suggestions.tsx
│   └── lib/
│       ├── types.ts           # KnowledgeNode、KnowledgeEdge 类型
│       └── example-content.ts # 内置示例文档和代码
├── package.json
└── .env.example
```
