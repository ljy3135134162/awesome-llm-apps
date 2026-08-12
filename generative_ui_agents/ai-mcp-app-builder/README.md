# AI MCP App Builder

在聊天中描述一个 MCP 应用，即可获得一个实时运行、隔离在沙箱中的应用实例。项目基于 [CopilotKit](https://github.com/CopilotKit/CopilotKit)、[AG-UI](https://github.com/ag-ui-protocol/ag-ui)、[Mastra](https://mastra.ai/) 和 [E2B](https://e2b.dev/) 沙箱构建。

**Gen UI 概念——由 Agent 生成完整应用。** 大多数生成式 UI 都是从固定的组件目录中选择组件，而这个项目更进一步：Agent 会在运行时编写一个全新的 MCP 应用，Builder 为其创建 E2B 沙箱进行托管，并将应用直接以内嵌方式渲染，同时保留完整的双向工具访问能力。也就是说，Agent 输出的“组件”本身就是一个完整应用。

这个 monorepo 将 **MCP App Builder** Web UI（`apps/web`）连接到 **Mastra** Agent（`/api/mastra-agent`）。该 Agent 会创建运行 **`mcp-use-server`** 模板（`apps/mcp-use-server`）的 **E2B** 沙箱。项目还提供了一个可选的本地示例：位于 **`apps/threejs-server`** 的 [Three.js MCP 示例](https://github.com/modelcontextprotocol/ext-apps/tree/main/examples/threejs-server)，在完全本地运行时用作侧边栏默认项。

https://github.com/user-attachments/assets/4bb35806-5e42-43c0-a8fe-01c0d1e5b8b3

## 环境要求

- Node.js 20+
- [pnpm](https://pnpm.io/installation)（workspace 必需）
- OpenAI API Key（`OPENAI_API_KEY`）；`/api/mastra-agent` 还可选配置 **`OPENAI_MODEL`**（默认 **`gpt-5.5`**）

## 快速开始

从项目根目录（`generative_ui_agents/ai-mcp-app-builder`）执行：

```powershell
pnpm i
Copy-Item .env.example .env
# 编辑 .env：至少设置 OPENAI_API_KEY=sk-proj-...；如需创建沙箱，再添加 E2B_*（见下文）
pnpm dev
```

**`pnpm dev`** 会运行 **Turbo** 并启动 workspace 中配置的 **`dev`** 任务，包括 Next.js 应用及其他已配置应用——具体参见根目录的 `package.json` / `turbo.json`。

**单独运行各部分**

| 目标 | 命令 |
| --- | --- |
| 仅运行 Web 应用 | `pnpm --filter web dev`（从仓库根目录）或 `cd apps/web && pnpm dev` |
| Three.js MCP 示例（本地侧边栏默认项） | `cd apps/threejs-server && pnpm dev` |
| `mcp-use-server`（本地 MCP，而非 E2B 镜像） | `cd apps/mcp-use-server && pnpm dev` |

打开 Next.js 输出的地址（通常为 `http://localhost:3000`）。

## 动态 MCP UI（侧边栏）

- **MCP 服务器：** 可通过 URL 添加/删除服务器，并可选指定 `serverId`；服务器列表通过 **`x-mcp-servers`** 发送。内置默认服务器为 **Excalidraw**（`https://mcp.excalidraw.com`）。可通过 **`NEXT_PUBLIC_DEFAULT_MCP_SERVERS`** / **`DEFAULT_MCP_SERVERS`** 覆盖。
- **工具：** 使用紧凑列表展示；打开某个工具后，会在**模态框**中显示**详情和预览**，而不是增加第三个移动端标签页。
- **聊天：** 使用带建议功能的 CopilotKit v2 Chat。

### 移动端布局

- **标签页：** **Chat** 和 **Tools**（服务器 + 工具列表）。工具的**预览/详情**通过**模态框**打开。
- **桌面端：** 侧边栏 + 聊天栏布局（**`md+`**）。
- **聊天体验：** 设置了合理的间距和底部留白，避免输入框遮挡最新消息。

## 环境变量（E2B）

| 变量 | 说明 |
| --- | --- |
| `E2B_API_KEY` | 从 [e2b.dev/dashboard](https://e2b.dev/dashboard) 获取 |
| `E2B_TEMPLATE` | 运行 **`build.dev.ts`** / **`build.prod.ts`** 后，从 `Template.build` 输出中取得的 **`templateId`** |
| `E2B_REPO_URL` | 当 **`E2B_TEMPLATE`** 为空时使用——将仓库克隆到沙箱中，因此冷启动较慢。代码中的默认值为 **`mcp-use-server-template`** GitHub URL |

## 文档

**UI 入口：** `apps/web/app/page.tsx`（主题、布局、CopilotKit 接入）。

**外部文档**

- [CopilotKit](https://docs.copilotkit.ai)
- [Next.js](https://nextjs.org/docs)
- [MCP Apps / UI](https://mcpui.dev/guide/introduction)
