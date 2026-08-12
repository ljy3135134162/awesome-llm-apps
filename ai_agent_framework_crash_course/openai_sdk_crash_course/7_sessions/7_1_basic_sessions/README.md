# 基础 Session

本示例展示如何使用 `SQLiteSession` 管理基础会话记忆，让 Agent 自动保存和恢复对话历史。

## 🎯 本示例展示的内容

- **内存 Session**：适合开发阶段的临时会话存储
- **持久化 Session**：基于文件的会话存储，适合长期保存
- **多轮对话**：自动保留上下文，无需手动拼接历史消息
- **Session Memory**：避免手动处理 `.to_input_list()`

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **配置环境变量**：
   ```bash
   cp ../env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行示例**：
   ```python
   import asyncio
   from agent import in_memory_session_example, persistent_session_example

   # 测试内存 Session
   asyncio.run(in_memory_session_example())

   # 测试持久化 Session
   asyncio.run(persistent_session_example())
   ```

## 💡 核心概念

- **SQLiteSession**：自动管理对话记忆
- **内存与持久化存储**：根据使用场景选择不同存储方式
- **Session ID**：通过唯一标识区分不同对话
- **自动上下文管理**：无需手动维护多轮消息链

## 🧪 可用示例

### 内存 Session
- 临时保存对话
- 进程结束后数据会丢失
- 适合开发和测试

### 持久化 Session
- 将对话保存到文件
- 应用重启后仍可保留历史
- 适合需要长期保存会话的应用

### 多轮对话
- 支持连续对话流程
- 自动保留上下文
- 让对话自然延续

## 🔗 后续步骤

- [Memory Operations](../7_2_memory_operations/README.md) —— 学习高级记忆操作
- [Multi Sessions](../7_3_multi_sessions/README.md) —— 学习同时管理多个会话
