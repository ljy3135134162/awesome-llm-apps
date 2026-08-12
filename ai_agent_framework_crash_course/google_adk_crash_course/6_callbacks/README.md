# 📋 教程 6：回调（Callbacks）

## 🎯 你将学到什么
- **Agent 生命周期回调**：监控 Agent 的创建、初始化和清理
- **LLM 交互回调**：跟踪模型请求、响应和 Token 使用情况
- **工具执行回调**：监控工具调用、参数和执行结果

## 💡 核心概念：回调

回调是在 Agent 执行到特定阶段时自动触发的函数。借助回调，你可以在不修改核心逻辑的情况下，对 Agent 的行为进行监控、日志记录和控制。

### **回调流程图**
```text
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  Agent 启动     │───▶│   LLM 请求      │───▶│   工具执行      │
│    回调         │    │    回调         │    │    回调         │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  Agent 结束     │    │   LLM 响应      │    │   工具结果      │
│    回调         │    │    回调         │    │    回调         │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### **为什么要使用回调？**
- **监控**：跟踪 Agent 的性能和行为
- **日志**：记录交互，便于调试和分析
- **控制**：根据特定事件动态调整行为
- **集成**：将 Agent 接入外部系统
- **调试**：了解 Agent 内部实际发生了什么

## 📖 教程概览

本教程介绍 Google ADK 中三类重要回调模式：

1. **Agent 生命周期回调**：监控 Agent 创建、初始化和清理事件
2. **LLM 交互回调**：跟踪模型请求、响应和 Token 使用情况
3. **工具执行回调**：监控工具调用、参数和执行结果

每个子教程都提供一个聚焦于特定回调模式的简洁示例。

## 📁 项目结构

```text
6_callbacks/
├── README.md                           # 本文件：概念说明
├── 6_1_agent_lifecycle_callbacks/      # Agent 生命周期监控
│   ├── README.md                       # 生命周期回调模式
│   ├── agent.py                        # 带生命周期回调的 Agent
│   ├── app.py                          # Streamlit 界面
│   └── requirements.txt                # 依赖
├── 6_2_llm_interaction_callbacks/      # LLM 请求/响应跟踪
│   ├── README.md                       # LLM 回调模式
│   ├── agent.py                        # 带 LLM 回调的 Agent
│   ├── app.py                          # Streamlit 界面
│   └── requirements.txt                # 依赖
└── 6_3_tool_execution_callbacks/       # 工具执行监控
    ├── README.md                       # 工具回调模式
    ├── agent.py                        # 带工具回调的 Agent
    ├── app.py                          # Streamlit 界面
    └── requirements.txt                # 依赖
```

## 🎯 学习目标

完成本教程后，你将理解：

- ✅ **回调基础**：Google ADK 中回调的工作方式
- ✅ **生命周期监控**：跟踪 Agent 的创建、初始化和清理
- ✅ **LLM 跟踪**：监控模型请求、响应和性能
- ✅ **工具监控**：跟踪工具执行和返回结果
- ✅ **实际应用**：了解回调在真实项目中的用途
- ✅ **调试技巧**：利用回调排查问题

## 🚀 快速开始

### **前置条件**
- Python 3.11+
- Google AI Studio API Key
- 已具备 Google ADK 基础知识（教程 1-5）

### **配置**
1. **获取 API Key**：访问 [Google AI Studio](https://aistudio.google.com/)
2. **创建 `.env` 文件**：添加 `GOOGLE_API_KEY=your_key_here`
3. **安装依赖**：`pip install -r requirements.txt`

### **运行教程**
```bash
# Agent 生命周期回调
cd 6_1_agent_lifecycle_callbacks
streamlit run app.py

# LLM 交互回调
cd ../6_2_llm_interaction_callbacks
streamlit run app.py

# 工具执行回调
cd ../6_3_tool_execution_callbacks
streamlit run app.py
```

## ⚙️ 回调模式

### **1. Agent 生命周期回调**
```python
def on_agent_start(agent_name: str):
    print(f"▶️ Agent {agent_name} started")

def on_agent_end(agent_name: str, result: str):
    print(f"✅ Agent {agent_name} completed: {result}")

# 注册回调
agent = LlmAgent(
    name="my_agent",
    model="gemini-3-flash-preview",
    on_start=on_agent_start,
    on_end=on_agent_end
)
```

### **2. LLM 交互回调**
```python
def on_llm_request(model: str, prompt: str):
    print(f"📤 LLM Request to {model}: {prompt[:50]}...")

def on_llm_response(model: str, response: str, tokens: int):
    print(f"📥 LLM Response from {model}: {tokens} tokens")

# 注册回调
agent = LlmAgent(
    name="my_agent",
    model="gemini-3-flash-preview",
    on_llm_request=on_llm_request,
    on_llm_response=on_llm_response
)
```

### **3. 工具执行回调**
```python
def on_tool_start(tool_name: str, params: dict):
    print(f"🔧 Tool {tool_name} started with params: {params}")

def on_tool_end(tool_name: str, result: str):
    print(f"✅ Tool {tool_name} completed: {result}")

# 注册回调
agent = LlmAgent(
    name="my_agent",
    model="gemini-3-flash-preview",
    tools=[my_tool],
    on_tool_start=on_tool_start,
    on_tool_end=on_tool_end
)
```

## 📊 应用场景

### **监控与分析**
- 跟踪 Agent 性能指标
- 监控 Token 使用量和成本
- 分析工具调用模式
- 调试 Agent 行为

### **日志与调试**
- 记录所有 Agent 交互
- 排查工具执行问题
- 监控 LLM 响应质量
- 跟踪错误模式

### **集成与控制**
- 接入外部监控系统
- 实现自定义错误处理
- 添加认证与校验
- 动态控制 Agent 行为

## 🔗 后续步骤

完成本教程后，可以继续学习：

- **[高级 Agent 模式](../6_callbacks/README.md)**：复杂 Agent 架构
- **[生产部署](../7_plugins/README.md)**：将 Agent 部署到生产环境
- **[自定义工具](../4_tool_using_agent/README.md)**：构建自定义工具与集成

## 📚 其他资源

- [Google ADK 文档](https://google.github.io/adk-docs/)
- [Callback API 参考](https://google.github.io/adk-docs/api-reference/python/)
- [最佳实践指南](https://google.github.io/adk-docs/best-practices/)
