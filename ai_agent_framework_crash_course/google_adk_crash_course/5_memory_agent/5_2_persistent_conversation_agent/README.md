# 🗄️ 教程 5.2：持久化对话 Agent

本教程介绍持久化会话管理，学习如何使用 SQLite 与 `DatabaseSessionService` 创建能够跨多个会话记住对话内容的 AI Agent。

## 🎯 你将学到什么

- **DatabaseSessionService**：使用 SQLite 持久化存储会话
- **跨会话记忆**：程序重启后仍能记住之前的对话
- **数据库管理**：配置和管理会话数据库
- **数据持久化**：长期保存对话历史
- **会话恢复**：检索并继续之前的对话

## 🧠 核心概念：持久化会话

**DatabaseSessionService** 会将会话数据存储到 SQLite 数据库文件中。这意味着：
- ✅ **持久化存储**：程序重启后数据仍然存在
- ✅ **跨会话记忆**：可以在不同会话之间保留对话信息
- ✅ **数据完整性**：利用 SQLite 的 ACID 特性
- ✅ **支持扩展**：可处理多个用户和多个会话
- ❌ **需要配置**：必须初始化数据库
- ❌ **基于本地文件**：默认受限于单机环境

适用于：
- 生产应用原型
- 多用户系统
- 长期对话历史
- 数据分析与洞察

## 🔧 关键组件

### 1. **DatabaseSessionService**
```python
from google.adk.sessions import DatabaseSessionService
```

### 2. **数据库结构**
```text
┌─────────────────────────────────────────────────────────────┐
│                    SQLITE 数据库                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │   SESSIONS  │  │    STATE    │  │   EVENTS    │         │
│  ├─────────────┤  ├─────────────┤  ├─────────────┤         │
│  │ session_id  │  │ session_id  │  │ event_id    │         │
│  │ user_id     │  │ state_data  │  │ session_id  │         │
│  │ app_name    │  │ updated_at  │  │ event_type  │         │
│  │ created_at  │  └─────────────┘  │ content     │         │
│  └─────────────┘                   │ timestamp   │         │
│                                    └─────────────┘         │
└─────────────────────────────────────────────────────────────┘
```

### 3. **持久化会话生命周期**
```text
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│    创建     │───▶│    使用     │───▶│    关闭     │
│    会话     │    │    会话     │    │    会话     │
│   （DB）    │    │   （DB）    │    │   （DB）    │
└─────────────┘    └─────────────┘    └─────────────┘
       │                   │                   │
       ▼                   ▼                   ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  数据库创建 │    │  数据库更新 │    │  数据库归档 │
└─────────────┘    └─────────────┘    └─────────────┘
```

## 🚀 教程概览

本教程将创建一个**简单的持久化 Agent**，它可以：
- 在程序重启后继续记住对话
- 使用 SQLite 数据库进行持久化存储
- 演示基础的跨会话记忆
- 展示其与内存会话的区别

## 📁 项目结构

```text
5_2_persistent_conversation/
├── README.md              # 本文件：概念说明
├── requirements.txt       # 依赖
├── agent.py               # 使用数据库会话管理的主 Agent
├── app.py                 # Streamlit Web 界面
└── sessions.db            # SQLite 数据库（自动创建）
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何使用 SQLite 配置 `DatabaseSessionService`
- ✅ 如何创建持久化会话
- ✅ 如何跨会话检索对话历史
- ✅ 如何管理数据库连接和事务
- ✅ 如何构建具备长期记忆能力的 Agent

## 🚀 快速开始

1. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

2. **配置环境**：
   ```bash
   # 创建 .env 文件并填入 Google AI API Key
   echo "GOOGLE_API_KEY=your_api_key_here" > .env
   ```

3. **运行 Agent**：
   ```bash
   # 启动 Streamlit 应用
   streamlit run app.py
   ```

4. **测试持久化能力**：
   - 与 Agent 进行一段对话
   - 关闭浏览器或应用
   - 重新启动应用
   - 继续对话，Agent 仍能记住之前的内容

## 🔍 代码解析

### 数据库会话管理核心代码

```python
# 1. 创建数据库会话服务
session_service = DatabaseSessionService(
    db_url="sqlite:///sessions.db"
)

# 2. 初始化数据库（创建表）
await session_service.initialize()

# 3. 创建或获取会话
session = await session_service.get_session(
    app_name="demo",
    user_id="user123",
    session_id="session_user123"
)

# 4. 配合 Runner 执行 Agent
async for event in runner.run_async(
    user_id=user_id,
    session_id=session_id,
    new_message=user_content
):
    # 处理响应
```

## 🎯 测试 Agent

可以通过以下测试验证持久化记忆：

### 测试 1：跨会话记忆
```text
会话 1：
用户："My name is Bob"
Agent："Nice to meet you, Bob!"

会话 2（应用重启后）：
用户："What's my name?"
Agent："Your name is Bob!"
```

### 测试 2：兴趣记忆
```text
会话 1：
用户："I love coding"
Agent："That's great! Coding is a wonderful skill."

会话 2（应用重启后）：
用户："What do I love?"
Agent："You love coding!"
```

### 测试 3：数据库验证
```text
1. 与 Agent 进行对话
2. 检查项目目录中是否生成 sessions.db
3. 重启应用
4. 继续对话，确认 Agent 仍保留之前的记忆
```

## 🔗 后续步骤

完成本教程后，可以继续学习：
- **[教程 5.3：云端记忆](../README.md)**：了解基于云服务的会话存储
- **高级数据库模式**：多用户会话管理
- **数据分析**：分析对话模式和历史数据

## 💡 实用建议

- **数据库位置**：SQLite 文件会创建在项目目录中
- **备份策略**：建议定期备份 `sessions.db`
- **性能**：SQLite 适合中小型应用，并具有良好性能
- **扩展性**：大型应用可以考虑 PostgreSQL 或云数据库

## 🚨 重要说明

- **数据库文件**：项目目录中会创建 `sessions.db`
- **数据持久化**：程序重启后对话仍会保留
- **文件权限**：确保项目目录具备写入权限
- **备份**：数据库文件包含全部对话数据，应妥善备份
