# 🧠 教程 5.1：内存会话 Agent

这是进入 Session 管理的第一步。本教程介绍如何使用 `InMemorySessionService` 创建一个能够在单次会话中记住对话内容的 AI Agent。

## 🎯 你将学到什么

- **InMemorySessionService**：用于临时对话的基础 Session 管理
- **Session 创建**：如何创建和管理对话 Session
- **状态管理**：保存并读取对话上下文
- **事件追踪**：记录对话历史
- **多轮对话**：构建能够记住上下文的 Agent

## 🧠 核心概念：内存 Session

**InMemorySessionService** 会把 Session 数据保存在计算机 RAM 中。这意味着：
- ✅ **访问速度快**：不需要查询数据库
- ✅ **配置简单**：无需外部依赖
- ❌ **临时存储**：程序停止后数据会丢失
- ❌ **无法持久化**：程序重启后不能继续记忆

非常适合：
- 开发和测试
- 临时对话
- 原型验证记忆功能
- 单 Session 应用

## 🔧 关键组件

### 1. **InMemorySessionService**
```python
from google.adk.sessions import InMemorySessionService
```

### 2. **Session 生命周期**
```text
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│    创建     │───▶│    使用     │───▶│    关闭     │
│   SESSION   │    │   SESSION   │    │   SESSION   │
└─────────────┘    └─────────────┘    └─────────────┘
```

### 3. **Session 数据结构**
```python
{
    "session_id": "unique_session_id",
    "user_id": "user_identifier",
    "state": {
        "conversation_history": [...],
        "user_preferences": {...},
        "current_context": "..."
    },
    "events": [
        {"type": "user_input", "content": "...", "timestamp": "..."},
        {"type": "agent_response", "content": "...", "timestamp": "..."}
    ]
}
```

## 🚀 教程概览

本教程将创建一个**个人助理 Agent**，它能够：
- 记住你的姓名和偏好
- 追踪对话历史
- 提供个性化响应
- 演示基础 Session 管理

## 📁 项目结构

```text
5_1_in_memory_conversation/
├── README.md              # 本文件：概念说明
├── requirements.txt       # 依赖
├── agent.py               # 带 Session 管理的主 Agent
└── app.py                 # Streamlit Web 界面
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何创建和管理 Session
- ✅ 如何保存和读取对话状态
- ✅ 如何追踪对话事件
- ✅ 如何构建多轮对话
- ✅ Session 生命周期的基本管理方式

## 🚀 快速开始

1. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

2. **配置环境**：
   ```bash
   # 创建包含 Google AI API Key 的 .env 文件
   echo "GOOGLE_API_KEY=your_api_key_here" > .env
   ```

3. **运行 Agent**：
   ```bash
   # 启动 Streamlit 应用
   streamlit run app.py
   ```

4. **测试记忆能力**：
   - 告诉 Agent 你的名字：`My name is John`
   - 询问它记住了什么：`What do you know about me?`
   - 进行多轮对话，观察它如何保持上下文

## 🔍 代码解析

### 关键 Session 管理代码

```python
# 1. 创建 Session Service
session_service = InMemorySessionService()

# 2. 创建新 Session
session = await session_service.create_session(
    app_name="personal_assistant",
    user_id="user123"
)

# 3. 更新 Session State
await session_service.update_session_state(
    session_id=session.session_id,
    state={"user_name": "John", "preferences": ["travel", "music"]}
)

# 4. 添加事件以追踪对话
await session_service.add_event(
    session_id=session.session_id,
    event_type="user_input",
    content="My name is John"
)
```

## 🎯 测试 Agent

可以使用以下对话流程测试记忆能力：

### 流程 1：个人信息
```text
用户："My name is Alice"
Agent："Nice to meet you, Alice! How can I help you today?"

用户："What's my name?"
Agent："Your name is Alice! I remember you told me that."
```

### 流程 2：偏好
```text
用户："I love pizza and hiking"
Agent："Great! I'll remember that you love pizza and hiking."

用户："What are my interests?"
Agent："Based on our conversation, you love pizza and hiking!"
```

### 流程 3：上下文连续性
```text
用户："I'm planning a trip"
Agent："That sounds exciting! Since you mentioned hiking, would you like recommendations for hiking destinations?"

用户："Yes, where should I go?"
Agent："Given your love for hiking, I'd recommend..."
```

## 🔗 后续步骤

完成本教程后，可以继续：
- **[教程 5.2：持久化会话](../5_2_persistent_conversation_agent/README.md)**：学习基于数据库的 Session 存储
- **[教程 5.3：云端记忆](../README.md)**：探索基于云服务的记忆方案

## 💡 实用建议

- **测试多轮对话**：通过较长对话观察记忆是否持续生效
- **监控 Session State**：使用 Web 界面查看 Agent 当前记住了什么
- **尝试不同 State 数据**：测试在 Session State 中保存不同类型的数据
- **理解限制**：内存 Session 本质上是临时数据

## 🚨 重要说明

- **数据丢失**：应用重启后，内存 Session 会全部丢失
- **单进程限制**：Session 只在同一个 Python 进程中有效
- **内存占用**：较长的对话历史会持续占用 RAM
- **适合开发环境**：内存 Session 更适合开发和测试，不建议直接用于生产环境
