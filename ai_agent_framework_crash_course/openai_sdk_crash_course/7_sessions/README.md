# 🎯 教程 7：Session 与记忆管理

本教程介绍 OpenAI Agents SDK 内置的 Session 机制，学习如何自动维护多轮对话历史，而无需在每轮交互之间手动处理 `.to_input_list()`。

## 🎯 你将学到什么

- **自动记忆**：由 Session 自动维护对话历史
- **SQLite Session**：使用内存或持久化方式保存会话
- **记忆操作**：添加、读取和管理会话条目
- **Session 管理**：维护多个独立 Session，并了解自定义实现方式

## 🧠 核心概念：什么是 Session？

Session 提供**自动化的对话记忆管理**。可以把它理解为一个智能对话数据库，它能够：

- 自动保存全部对话历史
- 在每次 Agent 运行前恢复上下文
- 根据不同 Session ID 隔离多个对话
- 在应用重启后继续保留持久化历史

```text
┌─────────────────────────────────────────────────────────────┐
│                       SESSION 工作流                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  用户输入                                                   │
│      │                                                      │
│      ▼                                                      │
│  ┌─────────────┐   1. 读取历史                              │
│  │   Session   │◀───────────────────────────────────────┐   │
│  │    Memory   │                                        │   │
│  └─────────────┘   2. 将历史加入本轮输入                  │   │
│      │                                                  │   │
│      ▼                                                  │   │
│  ┌─────────────┐                                        │   │
│  │    Agent    │   3. 带上下文执行                      │   │
│  │    Runner   │                                        │   │
│  └─────────────┘                                        │   │
│      │                                                  │   │
│      ▼                                                  │   │
│  ┌─────────────┐   4. 保存新消息                         │   │
│  │    响应     │────────────────────────────────────────┘   │
│  └─────────────┘                                            │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 教程概览

本教程包含三类 Session 示例：

### **1. 基础 SQLite Session**（`7_1_basic_sessions/`）
- 内存与持久化 Session
- 自动保存对话历史
- 基础多轮对话

### **2. 高级记忆操作**（`7_2_memory_operations/`）
- 使用 `get_items()`、`add_items()`、`pop_item()` 操作记忆
- 修改或纠正对话内容
- 管理 Session 数据

### **3. 多 Session 管理**（`7_3_multi_sessions/`）
- 管理多个独立对话上下文
- Session 隔离与组织
- 自定义 Session 实现思路

## 📁 项目结构

```text
7_sessions/
├── README.md                   # 本文件：概念说明
├── requirements.txt            # 依赖
├── streamlit_sessions_app.py   # 交互式 Streamlit 示例
├── 7_1_basic_sessions/
│   ├── agent.py                # SQLite Session 基础
│   └── README.md               # 基础 Session 文档
├── 7_2_memory_operations/
│   ├── agent.py                # 高级记忆操作
│   └── README.md               # 记忆操作文档
├── 7_3_multi_sessions/
│   ├── agent.py                # 多 Session 管理
│   └── README.md               # 多 Session 文档
└── env.example                 # 环境变量模板
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何使用 `SQLiteSession` 自动管理记忆
- ✅ 内存 Session 与持久化 Session 的区别
- ✅ 如何执行高级记忆操作
- ✅ 如何管理多个并发 Session
- ✅ 何时使用 Session，何时手动维护上下文

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

3. **配置环境变量**：
   ```bash
   cp env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

4. **启动交互式示例**：
   ```bash
   streamlit run streamlit_sessions_app.py
   ```

也可以分别运行各个示例：

```bash
python 7_1_basic_sessions/agent.py
python 7_2_memory_operations/agent.py
python 7_3_multi_sessions/agent.py
```

## 🧪 示例场景

### 基础 Session
- `What city is the Golden Gate Bridge in?` → `What state is it in?`
- `My name is Alice` → `What's my name?`
- `I work as a developer` → `What do I do for work?`

### 记忆操作
- 使用 `pop_item()` 修正上一条消息
- 使用 `clear_session()` 清空历史
- 手动插入自定义对话条目

### 多 Session
- 不同用户：`user_123`、`user_456`
- 不同业务上下文：`support_ticket_789`、`sales_inquiry_101`
- 不同应用：`chatbot_session`、`assistant_session`

## 🔧 常见 Session 模式

### 1. **基础用法**
```python
from agents import Agent, Runner, SQLiteSession

agent = Agent(name="Assistant", instructions="Reply concisely.")
session = SQLiteSession("conversation_123")

result = await Runner.run(agent, "Hello", session=session)
```

### 2. **持久化与内存模式**
```python
# 内存模式：进程结束后数据丢失
session = SQLiteSession("user_123")

# 文件持久化模式
session = SQLiteSession("user_123", "conversations.db")
```

### 3. **记忆操作**
```python
items = await session.get_items()

await session.add_items([
    {"role": "user", "content": "Hello"}
])

last_item = await session.pop_item()

await session.clear_session()
```

## 💡 Session 设计建议

- **使用有意义的 Session ID**：例如 `user_12345`、`support_ticket_789`
- **生产环境使用持久化存储**：避免应用重启后丢失历史
- **保持上下文隔离**：不同用户或业务流程使用不同 Session
- **利用 `pop_item()` 修正对话**：适合撤销或纠正上一轮消息
- **及时清理**：需要重新开始对话时清空 Session

## 🔗 后续步骤

完成本教程后，可以继续：
- **[教程 8：Handoff 与任务委派](../8_handoffs_delegation/README.md)** —— Agent 之间的任务转交
- **[教程 9：多 Agent 编排](../9_multi_agent_orchestration/README.md)** —— 构建复杂多 Agent 工作流
- **[教程 10：Tracing 与可观测性](../10_tracing_observability/README.md)** —— 生产环境监控与调试

## 🚨 故障排查

- **记忆未持久化**：确认 `SQLiteSession` 配置了数据库文件路径
- **Session 冲突**：为不同对话上下文使用唯一 Session ID
- **性能问题**：高并发场景可考虑实现自定义 Session 后端
- **数据库错误**：检查 SQLite 数据库文件的读写权限

## 💡 实用建议

- 开发阶段先使用内存 Session
- 在项目初期规划好 Session ID 规则
- 监控历史长度和存储规模
- 测试应用重启后的持久化恢复能力
- 大规模生产环境提前规划自定义 Session 后端
