# Personal Assistant Agent

这是一个基础个人助理 Agent 示例，用于展示如何通过 OpenAI Agents SDK 创建最简单的 Agent。

## 🎯 本示例展示的内容

- **基础 Agent 定义**：使用名称和 Instructions 创建简单 Agent
- **模型配置**：使用默认模型配置
- **基础 Instructions**：定义简单的对话行为

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **配置环境**：
   ```bash
   cp ../env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行 Agent**：
   ```python
   from agents import Runner
   from agent import root_agent

   result = Runner.run_sync(root_agent, "Hello, introduce yourself!")
   print(result.final_output)
   ```

## 💡 核心概念

- **Agent 定义**：通过 `Agent()` 类配置基础参数
- **Instructions**：使用自然语言指令约束和引导 Agent 行为
- **模型选择**：可以使用 SDK 默认模型，也可以根据需要显式指定模型

## 🧠 最小 Agent 工作流

一个最简单的 OpenAI Agent 通常只需要三部分：

```text
用户输入
   │
   ▼
Agent
  ├── name
  ├── instructions
  └── model
   │
   ▼
Runner.run_sync()
   │
   ▼
最终响应
```

这类基础 Agent 不依赖工具、Handoff、Session 或复杂工作流，因此非常适合用来理解 SDK 的核心执行方式。

## 🔗 后续步骤

这个示例展示的是最基础的 Agent 用法。后续可以继续学习：

- [Starter Agent 主教程](../README.md) —— 不同的 Agent 执行方式
- [教程 2：结构化输出](../../2_structured_output_agent/README.md) —— 使用 Pydantic Schema 定义输出
- [教程 3：工具调用 Agent](../../3_tool_using_agent/README.md) —— 为 Agent 添加工具和函数
