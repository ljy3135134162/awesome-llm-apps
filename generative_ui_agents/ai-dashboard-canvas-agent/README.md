# AI 仪表板 Canvas Agent

https://github.com/user-attachments/assets/9201d528-573f-43cc-9d31-571c362318a7

---

这是一个能够将**实时图表、指标和实时数据**填充到 Canvas 仪表板中的 Agent，而不只是流式输出文本。项目基于 [CopilotKit](https://github.com/CopilotKit/CopilotKit)、[AG-UI](https://github.com/ag-ui-protocol/ag-ui) 和 Google [ADK](https://google.github.io/adk-docs/) 构建。

**Gen UI 概念——Agentic Canvas。** 聊天区域只是一个轻量侧边栏，真正的工作界面是 Agent 持续写入的 Canvas。图表、KPI 和面板都是可寻址的 UI 产物，Agent 可以在多轮交互中放置、更新和重新排列它们。这种体验更接近一个协作者在使用白板，而不是传统聊天机器人逐条返回文本回复。

---

## 🔧 快速开始

```bash
# 从 awesome-llm-apps 仓库根目录开始
cd generative_ui_agents/ai-dashboard-canvas-agent

# 安装 JS 依赖和 Agent
pnpm install        # 也可以使用 npm/yarn/bun

# 单独安装 ADK Agent 的 Python 依赖
pnpm install:agent

# 设置 Google API Key
cp .env.example .env
# 编辑 .env，并设置 GOOGLE_API_KEY=...

# 同时启动 UI 和 Agent
pnpm run dev
```

### 📦 环境要求

- Node.js 18+
- Python 3.8+
- Google Makersuite API Key → [点击这里获取](https://makersuite.google.com/)
- 任意包管理器（推荐 pnpm）

💡 Lockfile（`package-lock.json`、`yarn.lock` 等）已加入 gitignore，每位开发者自行管理本地 Lockfile。

---

### 🛠 可用脚本

- `dev` → 启动 UI + Agent（默认）
- `dev:debug` → 使用 Debug 日志启动
- `dev:ui` → 仅运行 Next.js 应用
- `dev:agent` → 仅运行 ADK Agent
- `build / start` → 生产环境构建并启动服务器
- `lint` → 运行 ESLint
- `install:agent` → 在 `agent/.venv` 中安装 Python 依赖

---

### 🎨 自定义

- **主 UI** → `src/app/page.tsx`
- 修改主题、颜色和侧边栏外观
- 添加新的可视化组件
- 在 `/agent` 中扩展 Agent 逻辑

---

### 📚 文档

- [ADK](https://google.github.io/adk-docs/)
- [CopilotKit](https://github.com/CopilotKit/CopilotKit)
- [AG-UI](https://docs.ag-ui.com/introduction)
