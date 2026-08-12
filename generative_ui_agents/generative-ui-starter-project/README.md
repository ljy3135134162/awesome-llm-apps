# Generative UI Starter Project

这是一个由聊天驱动的看板应用，你和 Agent 可以共同操作同一份任务列表。项目基于 [CopilotKit](https://github.com/CopilotKit/CopilotKit)、[AG-UI](https://github.com/ag-ui-protocol/ag-ui) 和 [LangGraph](https://www.langchain.com/langgraph)，并构建在 Next.js 之上。同时，它也可以作为基于 [A2UI](https://a2ui.org/specification/) 的声明式生成式 UI Starter，其中包含一个航班搜索示例。

**Gen UI 概念——共享 Agent 状态。** 看板中的 To Do / Done 列保存在 Agent 状态中，并通过 `useAgent()` 与 React 双向同步。Agent 可以通过工具调用移动卡片；用户也可以直接在 UI 中点击、编辑和重新排序卡片。双方观察并操作的是同一份状态，无需额外的前端 Store，也不需要手动同步。

https://github.com/user-attachments/assets/47761912-d46a-4fb3-b9bd-cb41ddd02e34

## 环境要求

- Node.js 18+
- Python 3.8+
- [uv](https://docs.astral.sh/uv/)（Python 包管理器）
- 以下任意一种包管理器：
  - npm（默认）
  - [pnpm](https://pnpm.io/installation)
  - [yarn](https://classic.yarnpkg.com/lang/en/docs/install/)
  - [bun](https://bun.sh/)
- OpenAI API Key（用于 LangGraph Agent）

## 快速开始

1. 安装依赖（可使用 npm，或 pnpm/yarn/bun）：

```bash
npm install
```

这一步也会通过 `uv sync` 安装 Python Agent 依赖。

2. 配置环境变量：

```bash
cp .env.example .env
```

然后编辑 `.env` 文件并添加 OpenAI API Key：

```bash
OPENAI_API_KEY=your-openai-api-key-here
```

3. 启动开发服务器：

```bash
npm run dev
```

这会同时启动 UI 和 Agent 服务。

## 可用脚本

以下脚本也可以使用你偏好的包管理器执行：

- `dev` - 以开发模式同时启动 UI 和 Agent 服务
- `dev:debug` - 启动开发服务并启用 Debug 日志
- `dev:ui` - 仅启动 Next.js UI 服务
- `dev:agent` - 仅启动 LangGraph Agent 服务
- `build` - 构建生产环境 Next.js 应用
- `start` - 启动生产服务器
- `install:agent` - 安装 Agent 的 Python 依赖

## 项目结构

```text
├── src/                         # Next.js 前端源码
│   ├── app/
│   │   ├── page.tsx             # 主页面
│   │   └── api/copilotkit/      # CopilotKit API 路由
│   ├── components/
│   │   ├── example-canvas/      # Todo 列表 UI
│   │   ├── example-layout/      # 布局：聊天 + Canvas 并排
│   │   └── generative-ui/       # 生成式 UI 示例组件
│   └── hooks/
├── agent/                       # LangGraph Python Agent
│   ├── main.py                  # Agent 入口
│   └── src/
│       ├── todos.py             # Todo 工具和状态 Schema
│       └── query.py             # 示例数据查询工具
├── scripts/                     # Agent 配置与运行脚本
│   ├── setup-agent.sh / .bat
│   └── run-agent.sh / .bat
├── public/                      # 静态资源
├── next.config.ts
├── tsconfig.json
└── package.json
```

## A2UI —— Agent-to-User Interface

这个 Starter 内置了 [A2UI](https://a2ui.org/specification/) 支持，使 Agent 可以通过声明式方式生成丰富、可交互的 UI Surface。Agent 不再只返回纯文本，而是发送一份它希望渲染的 UI JSON 描述，再由前端转换成真实组件。

### 工作原理

A2UI 主要包含三个概念：

1. **Catalog** —— 一组组件定义（Schema）及对应的 React Renderer。只需要在 `layout.tsx` 中通过 `<CopilotKitProvider a2ui={{ catalog: demonstrationCatalog }}>` 注册一次。
2. **Surface** —— 一个实际渲染出来的 UI 实例。Agent 会创建 Surface、设置其中的组件，并向其绑定数据。
3. **Operations** —— Agent 从工具中返回 `a2ui.render(operations=[...])`，随后由中间件将这些操作流式传输到前端。

### 两种模式

| 模式 | 说明 | Agent 工具 | 前端 |
| --- | --- | --- | --- |
| **固定 Schema** | 组件布局预先定义，每次调用只改变数据 | `search_flights` | Schema 位于 `a2ui/schemas/flight_schema.json` |
| **动态 Schema** | 第二个 LLM 根据对话同时生成组件结构和数据 | `generate_a2ui` | 组件在运行时决定 |

两种模式在前端使用同一套 Catalog，区别只在于组件树从哪里生成。

### 关键文件

| 用途 | 路径 |
| --- | --- |
| Catalog 定义（Zod Schema） | `src/app/declarative-generative-ui/definitions.ts` |
| Catalog Renderer（React 组件） | `src/app/declarative-generative-ui/renderers.tsx` |
| Catalog 注册位置 | `src/app/layout.tsx` |
| 固定 Schema Agent 工具 | `agent/src/a2ui_fixed_schema.py` |
| 动态 Schema Agent 工具 | `agent/src/a2ui_dynamic_schema.py` |
| 航班 Schema JSON | `agent/src/a2ui/schemas/flight_schema.json` |
| Showcase 配置 | `showcase.json` |

### 添加自定义组件

1. 在 `definitions.ts` 中**定义**组件 Schema：

```typescript
MyWidget: {
  description: "给 Agent 阅读的简要说明。",
  props: z.object({ title: z.string(), value: z.number() }),
},
```

2. 在 `renderers.tsx` 中**实现渲染**：

```typescript
MyWidget: ({ props }) => (
  <div>{props.title}: {props.value}</div>
),
```

Renderer 会根据 Definition 进行类型检查。如果 Props 不匹配，TypeScript 会直接报错。

3. 从 Agent 中**使用该组件**。该组件会自动同时提供给固定 Schema 模板和动态 Schema LLM。

### 添加新的固定 Schema 工具

1. 在 `agent/src/a2ui/schemas/` 中创建一个 JSON Schema 文件，用来描述组件树。
2. 创建一个 Python 工具，通过 `a2ui.load_schema()` 加载 Schema，并使用你的数据返回 `a2ui.render(operations=[...])`。可以参考 `a2ui_fixed_schema.py` 的实现模式。

### Showcase 模式

`showcase.json` 控制哪些建议按钮会被视觉高亮。设置 `"showcase": "a2ui"` 可以突出 A2UI Demo；设置 `"showcase": "default"` 则不进行特殊高亮。使用 `npx copilotkit create --framework a2ui` 创建项目时，该配置会自动生成。

### 延伸阅读

- [A2UI Specification](https://a2ui.org/specification/)
- [CopilotKit A2UI Documentation](https://docs.copilotkit.ai)

## 文档

- [LangGraph Documentation](https://langchain-ai.github.io/langgraph/) - 了解 LangGraph 及其功能
- [CopilotKit Documentation](https://docs.copilotkit.ai) - 了解 CopilotKit 的能力
