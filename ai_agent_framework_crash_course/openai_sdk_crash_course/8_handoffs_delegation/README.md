# 🤝 教程 8：Handoff 与任务委派

本教程介绍 Agent 之间的任务委派机制。你将学习如何使用 OpenAI Agents SDK 的 Handoff 系统，让不同专业 Agent 根据任务类型智能地互相转交工作，从而构建多 Agent 工作流。

## 🎯 你将学到什么

- **Agent Handoff**：在专业 Agent 之间委派任务
- **Handoff 配置**：自定义工具名称、描述和回调
- **输入过滤**：控制 Agent 之间传递哪些上下文
- **Triage 模式**：构建智能路由和分诊系统

## 🧠 核心概念：什么是 Handoff？

Handoff 用于实现**Agent 专业化与任务转交**。一个 Agent 可以根据当前请求，将任务交给更擅长该领域的另一个 Agent。

可以把它理解为一个**智能路由系统**：

- 为不同领域创建专门 Agent，例如客服、账单、技术支持
- 根据用户需求自动决定应将任务交给哪个 Agent
- 在 Agent 切换过程中保留必要的对话上下文
- 支持自定义路由逻辑和输入过滤

```text
┌─────────────────────────────────────────────────────────────┐
│                     Handoff 工作流                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  用户请求                                                   │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    1. 分析请求                             │
│  │  Triage     │                                            │
│  │   Agent     │    2. 决定是否委派                         │
│  └─────────────┘                                            │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    3. 调用 Handoff 工具                    │
│  │  Handoff    │    "transfer_to_billing_agent"            │
│  │   Tool      │                                            │
│  └─────────────┘                                            │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    4. 转交上下文                           │
│  │  Billing    │    （可选过滤）                            │
│  │   Agent     │                                            │
│  └─────────────┘    5. 处理请求                             │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    6. 返回结果                             │
│  │    响应     │                                            │
│  │   给用户    │                                            │
│  └─────────────┘                                            │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 教程概览

本教程主要演示两类 Handoff 模式：

### **1. 基础 Handoff**（`basic_handoffs.py`）
- 简单的 Agent 到 Agent 委派
- 客服分诊场景
- 根据 Handoff 定义自动生成工具

### **2. 高级 Handoff**（`advanced_handoffs.py`）
- 使用回调进行自定义 Handoff 配置
- 输入过滤和上下文管理
- 使用结构化输入执行 Handoff

## 📁 项目结构

```text
8_handoffs_delegation/
├── README.md                # 本文件：概念说明
├── requirements.txt         # 依赖
├── basic_handoffs.py        # 简单 Agent Handoff
├── advanced_handoffs.py     # 高级 Handoff 模式
├── app.py                   # Streamlit Handoff 演示（可选）
└── env.example              # 环境变量模板
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何创建 Agent Handoff 实现任务委派
- ✅ 如何自定义 Handoff 工具名称和描述
- ✅ 如何通过 Input Filter 控制上下文传递
- ✅ 如何构建由多个专业 Agent 组成的智能分诊系统
- ✅ 何时使用 Handoff，何时使用显式 Agent 编排

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

4. **测试基础 Handoff**：
   ```bash
   python basic_handoffs.py
   ```

5. **测试高级模式**：
   ```bash
   python advanced_handoffs.py
   ```

## 🔧 常见 Handoff 模式

### 1. **基础 Handoff 配置**
```python
from agents import Agent, handoff

billing_agent = Agent(name="Billing Agent")
support_agent = Agent(name="Support Agent")

triage_agent = Agent(
    name="Triage Agent",
    handoffs=[billing_agent, support_agent]
)
```

这里 SDK 会根据 Handoff 自动创建对应工具。

### 2. **自定义 Handoff 配置**
```python
from agents import Agent, handoff

def on_handoff_callback(ctx):
    print(f"Handoff to {ctx.agent.name} initiated")

custom_handoff = handoff(
    agent=billing_agent,
    tool_name_override="escalate_to_billing",
    tool_description_override="Transfer complex billing issues",
    on_handoff=on_handoff_callback
)
```

### 3. **输入过滤**
```python
from agents.extensions import handoff_filters

filtered_handoff = handoff(
    agent=support_agent,
    input_filter=handoff_filters.remove_all_tools
)
```

这类过滤器可以避免将不必要的工具上下文继续传给下一个 Agent。

## 💡 Handoff 设计最佳实践

- **职责清晰**：每个 Agent 应有明确且不重叠的专业范围
- **路由信息明确**：为 Handoff 工具设置清晰的名称和描述，帮助模型正确选择
- **控制上下文**：只传递后续 Agent 真正需要的信息
- **使用回调**：通过 Callback 实现日志、指标采集和业务流程联动
- **输入结构化**：需要传递特定字段时，应使用结构化数据模型

## 🔗 后续步骤

完成本教程后，可以继续：
- **[教程 9：多 Agent 编排](../9_multi_agent_orchestration/README.md)** —— 学习并行执行和复杂多 Agent 工作流
- **[教程 10：Tracing 与可观测性](../10_tracing_observability/README.md)** —— 监控和调试 Agent 执行过程
- **[教程 11：语音 Agent](../11_voice/README.md)** —— 学习语音与实时交互能力

## 💡 实用建议

- **先从简单 Handoff 开始**：确认路由稳定后再增加过滤、回调等逻辑
- **明确 Agent Instructions**：让各 Agent 的角色和 Handoff 条件清晰可判断
- **测试路由结果**：针对不同类型请求验证模型是否转交给正确 Agent
- **监控 Handoff**：结合 Callback 和 Tracing 记录实际委派路径
- **提前设计上下文策略**：明确哪些信息需要跨 Agent 保留，哪些应被过滤
