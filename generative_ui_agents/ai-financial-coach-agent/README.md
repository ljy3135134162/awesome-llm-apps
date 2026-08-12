# AI 财务教练 Agent

这是一个多 Agent 财务教练应用，可以分析你的预算、规划储蓄，并制定债务偿还策略，同时将结果以交互式 UI 卡片的形式展示在独立的报告标签页中。项目基于 [CopilotKit](https://github.com/CopilotKit/CopilotKit)、[AG-UI](https://github.com/ag-ui-protocol/ag-ui) 和 Google [ADK](https://google.github.io/adk-docs/) 构建，前端使用 Next.js。

https://github.com/user-attachments/assets/edd4fa8d-ecc5-4b5d-90ff-27b21af5af94

**Gen UI 概念——由工具调用渲染组件。** 顶层 Coach Agent 会根据每轮对话选择合适的工具：从自然语言中更新你的财务画像（例如“我的月收入是 8000 美元”）、单独运行某个阶段（预算 / 储蓄 / 债务），或者执行完整的 Budget → Savings → Debt 流程。每次工具调用都会在聊天区域中流式显示一个状态标签，同时对应的 UI 卡片会出现在报告标签页中。

## 环境要求

- Node.js 18+
- Python 3.12+
- Google Makersuite API Key（供 ADK Agent 使用，可从 https://makersuite.google.com/app/apikey 获取）
- 以下任一包管理器：
  - npm（默认）
  - [pnpm](https://pnpm.io/installation)
  - [yarn](https://classic.yarnpkg.com/lang/en/docs/install/)
  - [bun](https://bun.sh/)

## 开始使用

1. 安装依赖（可使用 npm，也可使用 pnpm/yarn/bun）：

```bash
npm install
```

2. 安装 ADK Agent 的 Python 依赖：

```bash
npm run install:agent
```

> **注意：** 该命令会自动在 `agent` 目录中创建 `.venv` 虚拟环境。
>
> 如果希望手动激活虚拟环境，可以运行：
>
> ```bash
> source agent/.venv/bin/activate
> ```

3. 配置 Google API Key：

```bash
export GOOGLE_API_KEY="your-google-api-key-here"
```

4. 启动开发服务器：

```bash
npm run dev
```

该命令会同时启动 UI 和 Agent 服务。

## 可用脚本

以下脚本也可以使用你偏好的包管理器运行：

- `dev` - 以开发模式同时启动 UI 和 Agent 服务
- `dev:debug` - 启动开发服务并启用 Debug 日志
- `dev:ui` - 仅启动 Next.js UI 服务
- `dev:agent` - 仅启动 ADK Agent 服务
- `build` - 构建用于生产环境的 Next.js 应用
- `start` - 启动生产服务器
- `install:agent` - 安装 Agent 所需的 Python 依赖

## 自定义

主 UI 组件位于 `src/app/page.tsx`。你可以：

- 修改主题颜色和样式
- 添加新的前端 Action
- 自定义 CopilotKit 侧边栏外观

## 📚 文档

- [ADK Documentation](https://google.github.io/adk-docs/) - 了解 ADK 及其功能
- [CopilotKit Documentation](https://docs.copilotkit.ai) - 了解 CopilotKit 的能力
- [Next.js Documentation](https://nextjs.org/docs) - 了解 Next.js 的功能与 API
