# 多 Session 管理

本示例演示如何为不同用户、不同业务上下文以及不同对话类型同时管理多个独立 Session。

## 🎯 本示例展示的内容

- **多用户 Session**：为不同用户维护彼此独立的对话
- **基于上下文的 Session**：按客服、销售等业务场景划分不同 Session
- **共享 Session**：让多个 Agent 使用同一段对话历史
- **Session 组织方式**：Session 命名策略和管理模式

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
   from agent import multi_user_sessions, context_based_sessions

   # 测试多用户 Session
   asyncio.run(multi_user_sessions())

   # 测试基于业务上下文的 Session
   asyncio.run(context_based_sessions())
   ```

## 💡 核心概念

- **Session Isolation**：确保不同对话之间彼此隔离
- **User-Based Sessions**：为每个用户保存独立的对话历史
- **Context Switching**：根据不同业务类型和用途切换 Session
- **Agent Handoffs**：在专业 Agent 之间共享同一个 Session

## 🧪 可用模式

### 多用户 Session
- 分别为 Alice 和 Bob 创建独立 Session
- 对话历史彼此隔离
- 保留每个用户自己的上下文

### 基于上下文的 Session
- 客服工单对话
- 销售咨询对话
- 按功能或业务场景划分 Session

### 多 Agent 共享 Session
- 客服转交场景
- 多个 Agent 使用同一段对话
- Agent 切换时保持上下文连续

### Session 命名与组织
- 按用户命名：`user_123`
- 按功能命名：`chat_feature_user_123`
- 按线程命名：`thread_abc123`
- 按时间戳命名：`user_123_20241215`

## 🔗 后续步骤

- [基础 Session](../7_1_basic_sessions/README.md) —— 学习 Session 基础
- [Memory 操作](../7_2_memory_operations/README.md) —— 学习高级记忆管理
- [教程 8：Handoff 与任务委派](../../8_handoffs_delegation/README.md) —— 学习 Agent 之间的任务转交
