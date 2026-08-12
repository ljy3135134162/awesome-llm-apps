# 🖼️ 生成式 UI 与 Agentic 前端

**让 Agent 渲染 UI，而不只是输出文本。**

生成式 UI（Generative UI，Gen UI）应用允许 LLM 输出丰富、可交互的前端组件，而不仅仅是普通聊天消息。模型决定*展示什么*，前端负责渲染真实组件，用户可以点击、编辑并继续交互，从而把推理过程与界面操作真正闭环起来。

本目录收集了适用于常见技术栈的独立 Gen UI 应用模板：

- **AG-UI / CopilotKit** —— 面向 React 应用的流式 Agent ↔ UI 通信协议
- **Vercel AI SDK** —— 基于 `streamUI` / React Server Components 的生成式 UI
- **LangChain / LangGraph UI** —— 将结构化工具调用渲染为前端组件
- **自定义 Tool Call → Component Renderer** —— 适用于任意框架的最小化 DIY 实现模式
