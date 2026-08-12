# MCP Apps Generative UI Showcase

https://github.com/user-attachments/assets/48eeab8d-7845-4d06-83ef-d518a807da03

在聊天界面中直接预订航班、酒店，管理投资组合，以及操作看板。项目基于 [CopilotKit](https://github.com/CopilotKit/CopilotKit)、[AG-UI](https://github.com/ag-ui-protocol/ag-ui) 和 [MCP Apps](https://github.com/modelcontextprotocol/ext-apps) 构建，用于展示 MCP Apps Extension（SEP-1865）如何把交互式 UI 直接渲染到聊天中。

**Gen UI 概念——基于工具协议的沙箱化聊天内应用。** MCP Server 注册工具（如 `workout-generator`、`create-portfolio`、`create-board` 等），并通过 `_meta["ui/resourceUri"]` 将每个工具关联到一个 HTML/JS 资源。当 Agent 调用工具时，CopilotKit 会把对应应用挂载为聊天中的沙箱 iframe；iframe 再通过 JSON-RPC `postMessage` 与 MCP 工具双向通信。最终效果是：多步骤向导、拖拽看板、实时图表等完整交互界面可以直接嵌入聊天中运行——业务逻辑由服务器掌控，展示层由聊天界面承载。

## 在线演示

**https://web-app-production-9af6.up.railway.app**

## 示例应用

| 应用 | 说明 | 示例提示词 |
| --- | --- | --- |
| **✈️ 航班预订** | 5 步向导：搜索航班、选择座位、填写乘客信息 | “帮 2 名乘客预订 1 月 20 日从 JFK 到 LAX 的航班” |
| **🏨 酒店预订** | 4 步向导：搜索酒店、比较房型、完成住宿预订 | “帮我找巴黎 1 月 15 日到 18 日、2 位客人的酒店” |
| **📈 投资模拟器** | 带实时图表的投资组合管理，支持买入/卖出 | “创建一个 10,000 美元的科技股投资组合” |
| **📋 Kanban 看板** | 支持列与卡片拖拽的任务管理 | “为我的软件项目创建一个 Kanban 看板” |

## 快速开始

### 1. 安装依赖

```bash
# 从 mcp-apps 目录开始
npm install

cd mcp-server
npm install
cd ..
```

### 2. 设置环境变量

在 `mcp-apps` 目录创建 `.env.local`：

```bash
OPENAI_API_KEY=sk-...
```

### 3. 构建并运行

```bash
# 终端 1：构建并启动 MCP Server
cd mcp-server
npm run build
npm run dev
# Server 运行在 http://localhost:3001/mcp

# 终端 2：运行 Next.js 前端（从 mcp-apps 目录）
npm run dev
# Frontend 运行在 http://localhost:3000
```

打开 http://localhost:3000，然后尝试上面的示例提示词。

## 工作原理

MCP Apps 是运行在聊天侧栏沙箱 iframe 中的交互式 HTML/JS 应用。它们通过基于 `postMessage` 的 JSON-RPC 与 MCP Server 通信。

```text
用户："Book a flight from JFK to LAX"
        ↓
AI 调用 search-flights 工具
        ↓
MCPAppsMiddleware 拦截调用并获取 HTML 资源
        ↓
CopilotKit 在 iframe 中渲染 flights-app.html
        ↓
用户与向导 UI 交互
        ↓
UI 通过 postMessage 调用 MCP 工具 → Server
```

### 工具注册模式

```typescript
// Tool 通过 _meta 声明其 UI 资源
server.registerTool(
  "search-flights",
  {
    inputSchema: { origin, destination, departureDate, passengers },
    _meta: { "ui/resourceUri": "ui://flights/flights-app.html" },
  },
  handler,
);

// Resource 提供对应 HTML
server.registerResource(
  "flights-app",
  "ui://flights/flights-app.html",
  {
    mimeType: "text/html+mcp", // 标记为 MCP App
  },
  () => ({ contents: [{ text: htmlContent }] }),
);
```

## 项目结构

```text
mcp-apps/
├── src/app/
│   ├── page.tsx                    # 主 Demo 页面
│   └── api/copilotkit/route.ts     # CopilotKit + MCPAppsMiddleware
├── mcp-server/
│   ├── server.ts                   # 包含全部工具的 MCP Server
│   ├── src/
│   │   ├── flights.ts              # 15 个机场、6 家航空公司
│   │   ├── hotels.ts               # 10 个城市、30 家酒店
│   │   ├── stocks.ts               # 18 只股票与投资组合数据
│   │   └── kanban.ts               # 看板模板
│   └── apps/
│       ├── flights-app.html        # 航班预订向导
│       ├── hotels-app.html         # 酒店预订向导
│       ├── trading-app.html        # 投资模拟器
│       └── kanban-app.html         # Kanban 看板
└── README.md
```

## 核心技术

- **CopilotKit**（`@copilotkit/*`）——支持 MCP Apps 的 AI Chat 界面
- **AG-UI MCP Apps Middleware** —— 连接 MCP Server 与 CopilotKit
- **MCP SDK**（`@modelcontextprotocol/sdk`）——Model Context Protocol Server
- **Vite** —— 将每个应用打包为单一、独立的 HTML 文件

## 部署

Demo 在 Railway 上部署为两个服务：

| 服务 | URL |
| --- | --- |
| Web App | https://web-app-production-9af6.up.railway.app |
| MCP Server | https://mcp-server-production-bbb4.up.railway.app |

生产环境中，请将 `MCP_SERVER_URL` 环境变量设置为已部署 MCP Server 的地址。
