# 🎯 教程 1：你的第一个 ADK Agent

欢迎迈出学习 Google ADK 的第一步。本教程将介绍如何使用 Google Agent Development Kit 创建一个简单 AI Agent 的核心概念。

## 🎯 你将学到什么

- **基础 Agent 创建**：如何创建第一个 ADK Agent
- **ADK 工作流**：理解 Agent 生命周期
- **简单文本处理**：基础输入/输出处理
- **Agent 配置**：关键参数与设置

## 🧠 核心概念：什么是 ADK Agent？

ADK Agent 是一种**可编程的 AI 助手**，可以：
- 处理用户输入（文本、图片等）
- 使用 Gemini 等 AI 模型理解并生成响应
- 根据指令执行特定任务
- 返回结构化或非结构化结果

可以把它理解成一个使用 AI 来处理复杂任务的**智能函数**。

## 🔧 关键组件

### 1. **LlmAgent 类**
这是在 ADK 中创建 AI Agent 的主要基础组件：
```python
from google.adk.agents import LlmAgent
```

### 2. **核心参数**
- `name`：Agent 的唯一标识
- `model`：使用的 AI 模型，例如 `gemini-3-flash-preview`
- `description`：Agent 的用途说明
- `instruction`：Agent 应该如何行动

### 3. **基础工作流**
1. **输入**：用户发送消息
2. **处理**：Agent 使用 AI 模型理解并生成响应
3. **输出**：Agent 返回结果

## 🚀 教程概览

本教程将创建一个**创意写作 Agent**，它可以：
- 帮助用户构思故事创意和角色
- 提供写作提示和灵感
- 协助设计情节结构与节奏
- 演示 ADK 的基础功能

## 📁 项目结构

```text
1_starter_agent/
├── README.md               # 当前文件：概念说明
├── requirements.txt        # 依赖
└── creative_writing_agent/ # Agent 实现
    ├── __init__.py         # 将目录声明为 Python 包
    └── agent.py            # Agent 主代码
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何创建基础 ADK Agent
- ✅ 关键 Agent 参数及其用途
- ✅ 如何运行和测试 Agent
- ✅ ADK 的基础工作流和生命周期

## 🚀 开始使用

1. **配置环境**：
   ```bash
   # 确保你已经拥有 Google AI API Key
   # 可从这里获取：https://aistudio.google.com/
   ```

2. **安装依赖**：
   ```bash
   # 安装所需依赖
   pip install -r requirements.txt
   ```

3. **运行创意写作 Agent**：
   ```bash
   # 启动 ADK Web 界面
   adk web
   
   # 在 Web 界面中选择：creative_writing_agent
   ```

4. **测试 Agent**：
   - 尝试询问故事创意：“我想写一个关于魔法森林的故事”
   - 请求角色设计帮助：“帮我为科幻故事创建一个主角”
   - 请求写作提示：“给我一个有创意的写作题目”
   - 咨询情节结构：“我的故事高潮应该如何安排？”

## 🔗 下一步

完成本教程后，可以继续学习：
- **[教程 2：模型无关 Agent](../2_model_agnostic_agent/README.md)** —— 创建可使用不同 AI 模型的 Agent
- **[教程 3：结构化输出 Agent](../3_structured_output_agent/README.md)** —— 学习生成类型安全的结构化响应
- **[教程 4：工具调用 Agent](../4_tool_using_agent/README.md)** —— 为 Agent 添加自定义工具和函数

## 💡 实用建议

- **从简单开始**：先实现基础能力，再逐步增加复杂度
- **频繁测试**：使用 ADK Web 界面反复测试 Agent
- **认真编写指令**：清晰的 Instruction 会带来更稳定的 Agent 行为
- **多做实验**：尝试不同模型和参数，观察效果差异
