# 🎯 教程 1：你的第一个 OpenAI Agent

欢迎进入 OpenAI Agents SDK 学习之旅的第一步。本教程将介绍最基础的概念：如何使用 OpenAI Agents SDK 创建一个简单的 AI Agent。

## 🎯 你将学到什么

- **基础 Agent 创建**：如何创建你的第一个 OpenAI Agent
- **OpenAI SDK 工作流**：理解 Agent 的执行生命周期
- **简单文本处理**：基础输入与输出处理
- **Agent 配置**：核心参数和设置

## 🧠 核心概念：什么是 OpenAI Agent？

OpenAI Agent 可以理解为一个**可编程的 AI 助手**，它能够：
- 处理用户输入，例如文本、语音等
- 使用 GPT 等 AI 模型理解请求并生成响应
- 根据你提供的 Instructions 执行特定任务
- 返回结构化或非结构化结果

可以把它理解成一个利用 AI 处理复杂任务的**智能函数**。

## 🔧 关键组件

### 1. **Agent 类**
OpenAI Agents SDK 中创建 AI Agent 的核心组件：
```python
from agents import Agent
```

### 2. **核心参数**
- `name`：Agent 的唯一名称
- `instructions`：定义 Agent 应如何行动
- `model`：指定使用的 AI 模型

### 3. **基础工作流**
1. **输入**：用户发送消息
2. **处理**：Agent 使用 AI 模型理解请求并生成响应
3. **输出**：Agent 返回结果

## 🚀 教程概览

本教程包含两个重点示例：

### **1. Personal Assistant Agent**（`personal_assistant_agent/`）
- 基础 Agent 创建和配置
- 简单 Instructions 与角色定义
- Agent 类的核心用法

### **2. Execution Demo Agent**（`execution_demo_agent/`）
- 演示不同的 Agent 执行方式
- 同步、异步和流式执行模式
- Runner 类使用示例

## 📁 项目结构

```text
1_starter_agent/
├── README.md                    # 本文件：概念说明
├── requirements.txt             # 依赖
├── personal_assistant_agent/    # 基础 Agent 创建
│   ├── __init__.py
│   └── agent.py                 # 简单 Agent 定义
├── execution_demo_agent/        # 执行方式演示
│   ├── __init__.py
│   └── agent.py                 # 同步、异步、流式示例
├── app.py                       # Streamlit Web 界面（可选）
└── env.example                  # 环境变量模板
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何创建基础 OpenAI Agent
- ✅ Agent 核心参数及其用途
- ✅ 如何同步和异步运行 Agent
- ✅ OpenAI Agents SDK 的基础工作流与生命周期
- ✅ 如何使用流式响应

## 🚀 快速开始

1. **准备环境**：
   ```bash
   # 准备 OpenAI API Key
   # 获取地址：https://platform.openai.com/api-keys
   ```

2. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

3. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

4. **配置环境变量**：
   ```bash
   # 复制环境变量模板
   cp env.example .env

   # 编辑 .env 并添加 OpenAI API Key
   # OPENAI_API_KEY=sk-your_openai_key_here
   ```

5. **测试 Agent**：
   ```bash
   # 直接运行 Agent
   python agent.py

   # 或运行 Streamlit Web 界面
   streamlit run app.py
   ```

6. **尝试不同执行方式**：
   - 同步执行：`What's the weather like today?`
   - 异步执行：`Tell me a story about AI`
   - 流式响应：`Explain machine learning in detail`

## 🧪 推荐测试提示词

- **常识问题**：`What's the capital of France?`
- **创作任务**：`Write a short poem about technology`
- **问题解决**：`How can I improve my productivity?`
- **概念解释**：`Explain quantum computing in simple terms`

## 🔗 后续步骤

完成本教程后，可以继续：
- **[教程 2：结构化输出 Agent](../2_structured_output_agent/README.md)** —— 学习创建类型安全的结构化响应
- **[教程 3：工具调用 Agent](../3_tool_using_agent/README.md)** —— 为 Agent 添加自定义工具和函数
- **[教程 4：Agent 运行方式](../4_running_agents/README.md)** —— 掌握不同执行模式

## 💡 实用建议

- **从简单开始**：先实现基础功能，再逐步增加复杂度
- **频繁测试**：通过不同 Prompt 观察 Agent 行为
- **重视 Instructions**：清晰的指令通常能带来更稳定的 Agent 行为
- **主动实验**：比较同步、异步和流式执行方式的区别

## 🚨 故障排查

- **API Key 问题**：确认 `.env` 中包含有效的 `OPENAI_API_KEY`
- **Import 错误**：确认已经执行 `pip install -r requirements.txt`
- **Rate Limit**：如果触发速率限制，请稍后重新尝试
