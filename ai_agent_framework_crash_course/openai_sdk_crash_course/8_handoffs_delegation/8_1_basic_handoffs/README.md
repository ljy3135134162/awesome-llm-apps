# 基础 Handoff

本示例演示如何使用 OpenAI Agents SDK 的 Handoff 机制，在不同 Agent 之间进行最基础的任务委派。

## 🎯 本示例展示的内容

- **Agent Handoff**：在不同专业 Agent 之间进行简单任务转交
- **自动创建工具**：配置 Handoff 后，SDK 会自动生成对应的转交工具
- **Triage 路由模式**：根据用户请求类型智能选择处理 Agent
- **专业 Agent 分工**：分别处理账单问题和技术支持问题

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

3. **运行 Agent**：
   ```python
   import asyncio
   from agent import main

   # 测试基础 Handoff
   asyncio.run(main())
   ```

## 💡 核心概念

- **Handoff Definition**：通过 `handoffs` 参数注册可以接管任务的 Agent
- **Tool Generation**：SDK 自动生成 `transfer_to_*` 类型的转交工具
- **Agent Specialization**：为不同业务领域配置不同专业 Agent
- **Intelligent Routing**：由 LLM 判断应该把任务转交给哪个 Agent

## 🧪 可尝试的示例

### 转交给 Billing Agent
- `I was charged twice for my subscription`
- `Can you help me get a refund?`
- `What are my payment options?`

### 转交给 Technical Support Agent
- `My app keeps crashing`
- `I can't upload photos`
- `The app won't load`

### 不需要 Handoff
- `What are your customer service hours?`
- `How do I contact support?`
- `What services do you offer?`

## 💻 Agent 架构

```python
# 专业 Agent
billing_agent = Agent(name="Billing Agent", instructions="Handle billing issues")
technical_agent = Agent(name="Technical Support Agent", instructions="Handle technical issues")

# 带 Handoff 的 Triage Agent
triage_agent = Agent(
    name="Customer Service Triage Agent",
    handoffs=[billing_agent, technical_agent]  # SDK 自动创建转交工具
)
```

这个结构中，`triage_agent` 负责理解用户意图。当请求属于账单或技术问题时，它会调用 SDK 自动生成的 Handoff 工具，把当前任务交给对应的专业 Agent 继续处理。

## 🔗 后续步骤

- [高级 Handoff](../8_2_advanced_handoffs/README.md) —— 学习自定义配置、回调和结构化输入
- [教程 9：多 Agent 编排](../../9_multi_agent_orchestration/README.md) —— 构建更复杂的多 Agent 工作流
