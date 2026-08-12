# 🧠 教程 5：上下文管理

本教程介绍如何使用 OpenAI Agents SDK 构建具备上下文感知能力的 Agent，重点学习通过 `RunContextWrapper` 传递自定义上下文对象，让 Agent 在执行过程中访问用户数据、Session 信息和共享状态。

## 🎯 你将学到什么

- **RunContextWrapper**：向 Agent 传递自定义上下文对象
- **上下文感知工具**：构建能够访问用户状态与偏好的工具
- **类型安全上下文**：使用泛型获得类型检查能力
- **上下文修改**：在 Agent 执行期间读取和更新上下文
- **生产模式**：真实应用中的上下文管理策略

## 🧠 核心概念：什么是上下文管理？

上下文管理允许你向 Agent 传入一个**自定义数据对象**，并让该对象在整个 Agent 执行过程中持续可用。可以把 Context 理解为一个**共享状态容器**，用于：

- 保存用户信息、偏好和 Session 数据
- 提供数据库和外部系统访问对象
- 在多次 Tool Call 之间维护状态
- 实现个性化 Agent 行为
- 提供类型安全的数据访问方式

```text
┌─────────────────────────────────────────────────────────────┐
│                       Context 工作流                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  用户 Context                                               │
│  ┌─────────────┐                                            │
│  │  UserInfo   │    1. 传给 Runner                         │
│  │  - name     │ ────────────────────────────────────────┐  │
│  │  - uid      │                                         │  │
│  │  - prefs    │                                         ▼  │
│  └─────────────┘                                            │
│                   ┌─────────────┐                           │
│                   │   Agent     │    2. 所有工具都可以      │
│                   │   Runner    │       访问 Context        │
│                   └─────────────┘                           │
│                         │                                   │
│                         ▼                                   │
│                   ┌─────────────┐    3. 工具通过            │
│                   │ Tool Call   │       RunContextWrapper   │
│                   │ + Context   │       访问 Context        │
│                   └─────────────┘                           │
│                         │                                   │
│                         ▼                                   │
│                   ┌─────────────┐    4. Context 可修改，     │
│                   │ Context     │       且修改结果持续存在   │
│                   │ 更新        │                           │
│                   └─────────────┘                           │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 核心上下文管理概念

### **Context 对象**
使用自定义数据类保存状态和用户信息：

```python
@dataclass
class UserInfo:
    name: str
    uid: int
    preferences: dict = None
```

### **RunContextWrapper**
工具通过类型安全的 Wrapper 访问 Context：

```python
@function_tool
async def my_tool(wrapper: RunContextWrapper[UserInfo]) -> str:
    user = wrapper.context
    return f"Hello {user.name}"
```

### **上下文感知 Agent**
通过泛型声明 Agent 使用的 Context 类型：

```python
agent = Agent[UserInfo](
    name="Context Agent",
    tools=[context_aware_tool]
)
```

## 🧪 本教程演示的内容

### **1. 用户信息 Context**
- 保存用户资料，例如姓名、ID 和偏好
- 根据用户 Context 个性化 Agent 响应
- 在对话过程中更新用户偏好

### **2. 上下文感知工具**
- `fetch_user_profile()`：从 Context 获取用户资料
- `update_user_preference()`：修改 Context 中的用户设置
- `get_personalized_greeting()`：根据 Context 生成个性化问候

### **3. 类型安全**
- 使用 `Agent[UserInfo]` 泛型提供类型检查
- 使用 `RunContextWrapper[UserInfo]` 获取类型明确的 Context
- IDE 可以提供更完整的自动补全和类型提示

### **4. Context 持久性**
- 对 Context 的修改会在后续 Tool Call 中继续存在
- 状态更新可以贯穿一次完整 Agent 执行
- 后续操作可以读取已经更新后的 Context

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何使用 dataclass 创建自定义 Context 对象
- ✅ 如何通过 RunContextWrapper 类型安全地访问 Context
- ✅ 如何构建能够读取和修改状态的上下文感知工具
- ✅ 如何利用 Context 实现个性化 Agent 行为
- ✅ 大规模应用中的 Context 管理模式

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **配置环境**：
   ```bash
   cp env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行 Context 示例**：
   ```python
   import asyncio
   from agent import context_example

   asyncio.run(context_example())
   ```

## 🧪 示例场景

### 基础 Context 使用
- `Hello! I'd like to know about my profile and prefer casual greetings.`
- `Update my greeting style to friendly`
- `What are my current preferences?`

### 个性化应用
- 根据用户偏好生成不同问候方式
- 根据用户资料调整 Agent 回复
- 通过 Context 更新动态修改 Agent 行为

### 生产应用
- Web 应用中的用户 Session 管理
- 携带账户信息的客户支持 Agent
- 基于购物偏好和历史记录的电商 Agent

## 🔧 常见 Context 模式

### 1. **基础 Context 创建**
```python
from dataclasses import dataclass

@dataclass
class UserInfo:
    name: str
    uid: int
    preferences: dict = None
```

### 2. **上下文感知工具**
```python
@function_tool
async def my_tool(wrapper: RunContextWrapper[UserInfo]) -> str:
    user = wrapper.context
    return f"Processing for {user.name}"
```

### 3. **携带 Context 运行 Agent**
```python
user_context = UserInfo(name="Alice", uid=123)
result = await Runner.run(agent, "message", context=user_context)
```

### 4. **更新 Context**
```python
@function_tool
async def update_preference(
    wrapper: RunContextWrapper[UserInfo],
    key: str,
    value: str
) -> str:
    wrapper.context.preferences[key] = value
    return f"Updated {key} to {value}"
```

## 💡 Context 管理最佳实践

- **使用 Dataclass**：通过 Python dataclass 定义结构清晰的 Context
- **保持类型安全**：始终使用泛型声明 Context 类型
- **尽可能不可变**：只读 Context 可考虑使用 frozen dataclass
- **进行数据验证**：在 Context 初始化阶段验证关键字段
- **清晰文档**：明确说明每个 Context 字段的含义和用途

## 🔗 相关概念

- **Sessions**：Context 可以与 Session Memory 配合使用，形成完整状态体系
- **Guardrails**：Guardrail 可以读取 Context 参与输入/输出验证
- **Tool Calling**：Context 让工具可以根据当前用户和状态执行更复杂的逻辑

## 🚨 常见问题

- **缺少泛型类型**：建议明确写成 `Agent[YourContextType]`
- **意外修改 Context**：共享状态需要控制写入范围，避免非预期副作用
- **内存占用**：大型 Context 对象使用结束后应及时释放
- **并发安全**：多线程或并发应用中要注意 Context 的并发访问问题

## 💡 实用建议

- **从简单开始**：先保存基础用户信息，再逐步扩展复杂状态
- **尽早验证**：在 Context 对象创建阶段处理无效数据
- **充分使用 Type Hint**：利用 Python 类型系统提升 IDE 与静态检查体验
- **考虑不可变设计**：只读场景优先使用不可变 Context
- **维护 Context 文档**：复杂项目中应明确约定 Context 结构

## 🔗 后续步骤

掌握 Context 管理后，可以继续：
- **[教程 6：Guardrail 与验证](../6_guardrails_validation/README.md)** —— 使用 Context 参与输入与输出安全控制
- **[教程 7：Sessions](../7_sessions/README.md)** —— 将 Context 与对话记忆结合
- **[教程 8：Handoff 与任务委派](../8_handoffs_delegation/README.md)** —— 在多 Agent 场景中管理 Context
