# 自定义 Tracing

本示例展示 OpenAI Agents SDK 中更高级的追踪模式，包括自定义 Trace、Span，以及如何为复杂的多 Agent 系统组织完整工作流。

## 🎯 本示例展示的内容

- **自定义 Trace**：将多个 Agent 运行归入同一个工作流
- **自定义 Span**：为业务逻辑添加可观测节点
- **层级追踪**：通过嵌套 Span 表达复杂操作关系
- **Trace 元数据**：使用分组和 Metadata 组织追踪记录

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **配置环境**：
   ```bash
   cp ../env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行 Agent**：
   ```python
   import asyncio
   from agent import main

   # 测试自定义 Tracing 模式
   asyncio.run(main())
   ```

## 💡 核心概念

- **`trace()` Context Manager**：创建自定义工作流分组
- **`custom_span()`**：为业务逻辑增加独立监控节点
- **Trace Metadata**：为工作流添加名称、分组和业务信息
- **层级结构**：通过嵌套 Span 表达复杂操作之间的父子关系

## 🧪 自定义 Tracing 模式

### 多步骤工作流 Trace

```python
with trace("Research and Analysis Workflow") as workflow_trace:
    # Step 1: Research
    research_result = await Runner.run(research_agent, "Research AI in healthcare")

    # Step 2: Analysis
    analysis_result = await Runner.run(
        analysis_agent,
        f"Analyze: {research_result.final_output}"
    )

    # Step 3: Summary
    summary_result = await Runner.run(
        analysis_agent,
        f"Summarize: {analysis_result.final_output}"
    )
```

这种方式可以将多个 `Runner.run()` 归入同一条 Trace，便于查看整个业务工作流，而不是把每一步拆成彼此独立的运行记录。

### 自定义业务逻辑 Span

```python
with trace("Document Processing Workflow") as doc_trace:

    with custom_span("Data Preparation") as prep_span:
        # 业务逻辑
        data = prepare_data()
        prep_span.add_event("Data loaded", {"records": 100})
        prep_span.add_event("Data validated", {"errors": 0})

    with custom_span("AI Processing") as ai_span:
        result = await Runner.run(agent, "Process the data")
        ai_span.add_event("Processing complete", {
            "output_length": len(result.final_output)
        })
```

通过 `custom_span()`，可以把传统业务代码和 Agent 调用放在同一个可观测工作流中。

### 层级 Span

```python
with trace("E-commerce Order Processing") as order_trace:

    with custom_span("Order Validation") as validation_span:

        # 库存检查子 Span
        with custom_span("Inventory Check") as inventory_span:
            inventory_span.add_event("Stock verified", {"available": True})

        # 支付验证子 Span
        with custom_span("Payment Validation") as payment_span:
            payment_span.add_event("Payment authorized", {"amount": 99.99})
```

嵌套 Span 可以表达复杂业务流程中的层级关系，例如“订单验证”下面再包含“库存检查”和“支付验证”。

## 💻 高级功能

### Trace 元数据与分组

```python
conversation_id = "conv_12345"

# 同一会话中的第一次交互
with trace(
    "Customer Support - Initial Inquiry",
    group_id=conversation_id,
    metadata={"customer_id": "cust_789", "priority": "high"}
) as trace1:
    result1 = await Runner.run(
        support_agent,
        "How do I reset my password?"
    )

# 同一会话中的后续交互
with trace(
    "Customer Support - Follow-up",
    group_id=conversation_id,
    metadata={"customer_id": "cust_789", "interaction": 2}
) as trace2:
    result2 = await Runner.run(
        support_agent,
        f"Based on this context: {result1.final_output}"
    )
```

`group_id` 可以把多条相关 Trace 归到同一个业务会话中，而 `metadata` 可用于记录客户 ID、优先级、交互次数等信息。

### 事件记录

```python
with custom_span("Business Process") as span:
    span.add_event("Process started", {"timestamp": datetime.now()})
    # 业务逻辑
    span.add_event("Milestone reached", {"progress": "50%"})
    # 更多业务逻辑
    span.add_event("Process completed", {"status": "success"})
```

Event 适合记录阶段性状态变化，例如处理开始、关键节点、完成状态或异常信息。

## 🔍 自定义 Tracing 的价值

### 工作流组织

- **关联相关操作**：把多个 Agent 调用归入同一条 Trace
- **业务逻辑可见性**：同时观察 AI 调用与普通业务代码
- **端到端性能分析**：追踪整个工作流的执行时间和瓶颈

### 生产监控

- **错误关联**：把跨组件失败放到同一上下文中分析
- **性能优化**：定位复杂工作流中的慢节点
- **用户旅程追踪**：跨多轮交互观察同一个用户或会话

### 调试与分析

- **理解复杂工作流**：可视化多步骤流程和 Agent 之间的调用关系
- **保持上下文关联**：保留多个相关操作之间的关系
- **元数据组织**：根据业务字段筛选和定位 Trace

## 🧠 推荐使用方式

默认 Tracing 已足以观察单个 Agent 运行，但当系统开始出现多阶段业务流程时，自定义 Trace 和 Span 会更有价值：

```text
业务请求
   │
   ▼
Trace：完整工作流
   │
   ├── Span：数据准备
   ├── Span：Agent A
   ├── Span：Agent B
   └── Span：结果处理
```

这种结构能把“模型调用”提升为完整的端到端业务可观测性，而不仅仅是查看某一次 LLM 请求。

## 🔗 后续步骤

- [默认 Tracing](../10_1_default_tracing/README.md) —— Tracing 基础
- [教程 11：Voice](../../11_voice/README.md) —— 继续学习语音 Agent 与实时交互能力
