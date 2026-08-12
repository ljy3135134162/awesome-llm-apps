# Agents as Tools

本示例展示一种高级多 Agent 编排模式：把专业 Agent 包装成函数工具，再由一个上层 Orchestrator Agent 根据任务需要决定何时调用这些工具。

## 🎯 本示例展示的内容

- **通过 `@function_tool` 使用 Agent**：将专业 Agent 封装成可复用工具
- **内容生产流水线**：Research → Writing → Editing
- **自定义配置**：为不同 Agent 设置独立运行参数
- **智能编排**：由 LLM 决定工作流中工具的调用顺序和方式

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

   # 测试 Agent 编排模式
   asyncio.run(main())
   ```

## 💡 核心概念

- **Agent Tool 创建**：通过 `@function_tool` 和 `Runner.run()` 将 Agent 包装为工具
- **工作流编排**：协调多个专业 Agent 完成一个复杂任务
- **自定义配置**：为每个工具设置不同的 `max_turns`、温度等参数
- **智能协调**：由 Orchestrator Agent 判断什么时候以及如何调用各个专业工具

## 🧪 可用工具

### Research Tool

```python
@function_tool
async def research_tool(topic: str) -> str:
    result = await Runner.run(
        research_agent,
        input=f"Research this topic thoroughly: {topic}",
        max_turns=3
    )
    return str(result.final_output)
```

这个工具负责把研究主题交给 `research_agent`，并将最终研究结果作为字符串返回给上层 Agent。

### Writing Tool

```python
@function_tool
async def writing_tool(content: str, style: str = "professional") -> str:
    prompt = f"Write engaging {style} content based on this research: {content}"
    result = await Runner.run(writing_agent, input=prompt, max_turns=2)
    return str(result.final_output)
```

Writing Tool 接收研究结果和写作风格，再调用专门的写作 Agent 生成正文。

### Editing Tool

```python
@function_tool
async def editing_tool(content: str) -> str:
    result = await Runner.run(
        editing_agent,
        input=f"Edit and improve this content: {content}"
    )
    return str(result.final_output)
```

Editing Tool 负责对生成内容进行润色、校正和质量提升。

## 💻 编排模式

### 基础内容工作流

典型流程如下：

```text
用户需求
   │
   ▼
Orchestrator Agent
   │
   ├──► Research Tool ──► Research Agent
   │
   ├──► Writing Tool ───► Writing Agent
   │
   └──► Editing Tool ───► Editing Agent
                           │
                           ▼
                        最终内容
```

通常包含三个阶段：
1. **Research**：收集并整理较完整的信息
2. **Write**：根据研究结果生成结构化内容
3. **Edit**：对最终内容进行润色和改进

### 高级编排

工具化 Agent 可以进一步支持：
- **条件逻辑**：根据用户要求动态改变工作流
- **风格选择**：为不同任务采用不同写作方式
- **质量控制**：经过多个阶段反复检查和改进

例如，简单问题可能只需要 Research Tool，而文章生成任务则可能完整执行 Research → Writing → Editing。

## 🧠 Agent 作为工具与 Handoff 的区别

虽然两者都能实现多 Agent 协作，但控制方式不同。

### Agent as Tool

调用专业 Agent 后，控制权通常会回到原来的 Orchestrator Agent：

```text
Orchestrator
    │
    ├── 调用 Research Agent
    │      │
    │      └── 返回结果
    │
    ├── 调用 Writing Agent
    │      │
    │      └── 返回结果
    │
    ▼
继续统一协调
```

这种模式适合：
- 一个中心 Agent 需要保持总体控制权
- 专业 Agent 更像可复用能力模块
- 工作流需要在多个工具调用之间持续进行决策

### Handoff

Handoff 更像把当前任务和对话控制权直接移交给另一个 Agent。

因此，如果需要一个“项目经理”Agent 始终掌握流程，Agents as Tools 往往更合适；如果希望用户直接进入某个专业 Agent 的处理流程，则 Handoff 更自然。

## 🔧 手动编排与自动编排

### 直接调用 Agent

应用代码可以显式规定流程：

```text
Research Agent → Writing Agent → Editing Agent
```

优点是执行顺序明确、可预测；缺点是流程相对固定。

### Tool-Based 编排

将专业 Agent 封装成工具后：

```text
用户请求 → Orchestrator Agent → 动态选择需要调用的 Agent Tools
```

优点是工作流更加灵活，由模型根据当前任务决定调用哪些能力。

## 📊 适用场景

Agents as Tools 模式适合：
- 研究与写作流水线
- 软件开发中的规划、编码、审查 Agent
- 数据分析中的检索、计算、解释 Agent
- 企业工作流中的多个专业能力模块
- 需要中心 Orchestrator 持续掌控整个任务的复杂系统

## 💡 设计建议

- 每个专业 Agent 尽量保持单一职责
- 工具名称和描述要明确，让 Orchestrator 能准确判断用途
- 对高成本 Agent 设置合理的 `max_turns`
- 工具返回值尽量保持清晰，便于后续 Agent 继续处理
- 对固定且关键的业务流程，不必完全依赖 LLM 自由决定调用顺序

## 🔗 后续步骤

- [并行执行](../9_1_parallel_execution/README.md) —— 学习并发 Agent 模式
- [教程 10：Tracing 与可观测性](../../10_tracing_observability/README.md) —— 学习如何监控复杂多 Agent 工作流
