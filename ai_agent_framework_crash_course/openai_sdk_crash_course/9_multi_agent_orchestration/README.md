# 🎼 教程 9：多 Agent 编排

本教程介绍复杂的多 Agent 工作流，学习如何通过并行执行、Agents as Tools 模式以及更高级的编排技术协调多个 Agent，构建复杂的 AI 系统。

## 🎯 你将学到什么

- **并行执行**：使用 `asyncio.gather()` 同时运行多个 Agent
- **Agents as Tools**：将 Agent 作为函数工具参与复杂编排
- **工作流协调**：组合顺序执行与并行执行模式
- **结果综合**：智能合并多个 Agent 的输出

## 🧠 核心概念：什么是多 Agent 编排？

多 Agent 编排用于构建**协同式 AI 工作流**，让多个专业化 Agent 共同完成复杂任务。可以把编排系统理解为一个“指挥者”：

- 不同 Agent 负责不同领域与能力
- 根据任务需要并行或顺序执行
- 综合多个 Agent 的结果形成最终输出
- 将复杂任务拆分给不同 AI 能力处理

```text
┌─────────────────────────────────────────────────────────────┐
│                     多 AGENT 编排                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  复杂任务                                                   │
│      │                                                      │
│      ▼                                                      │
│  ┌─────────────┐    1. 任务拆解                             │
│  │ 编排 Agent  │    2. Agent 协调                           │
│  └─────────────┘                                            │
│      │                                                      │
│      ▼                                                      │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                  并行执行                             │  │
│  │  Research   Writing   Analysis   Review               │  │
│  │   Agent      Agent      Agent      Agent               │  │
│  └───────────────────────────────────────────────────────┘  │
│      │          │          │          │                      │
│      ▼          ▼          ▼          ▼                      │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                  结果综合                             │  │
│  │  • 智能合并多个输出                                   │  │
│  │  • 质量评估与结果选择                                 │  │
│  │  • 生成最终协调后的响应                               │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 教程概览

本教程演示三种核心编排模式：

### **1. Parallel Agent Execution**（`parallel_execution.py`）
- 使用 `asyncio.gather()` 并行运行多个 Agent
- 对结果执行质量评估并选出最佳答案
- 通过多次翻译结果演示并行模式

### **2. Agents as Tools Orchestration**（`agents_as_tools.py`）
- 将专业 Agent 封装为函数工具
- 构建 Research + Writing 内容生产工作流
- 自定义 Agent 工具配置与协调逻辑

### **3. Complex Workflow Orchestration**（`complex_orchestration.py`）
- 混合并行与顺序执行的多阶段工作流
- Research、Writing、Review、Optimization 内容流水线
- 高级结果综合与质量控制

## 📁 项目结构

```text
9_multi_agent_orchestration/
├── README.md                    # 本文件：概念说明
├── requirements.txt             # 依赖
├── parallel_execution.py        # 并行 Agent 模式
├── agents_as_tools.py           # Agents as Tools 编排
├── complex_orchestration.py     # 高级工作流模式
├── app.py                       # Streamlit 编排演示（可选）
└── env.example                  # 环境变量模板
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何并行运行多个 Agent 以提升执行效率
- ✅ 如何将 Agent 封装为函数工具进行复杂编排
- ✅ 如何组合顺序执行与并行执行模式
- ✅ 如何智能综合多个 Agent 的结果
- ✅ 不同场景下应该选择哪种编排方式

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

4. **测试并行执行**：
   ```bash
   python parallel_execution.py
   ```

5. **测试 Agents as Tools**：
   ```bash
   python agents_as_tools.py
   ```

6. **测试复杂工作流**：
   ```bash
   python complex_orchestration.py
   ```

## 🧪 示例场景

### 并行执行
- 同时生成多份翻译结果并选择最佳版本
- 并行生成多个内容候选，再进行筛选
- 同时从多个视角进行研究

### Agents as Tools
- 内容生产：研究 → 写作 → 编辑
- 分析流程：数据处理 → 洞察 → 建议
- 客户服务：分诊 → 专业 Agent → 质量检查

### 复杂编排
- 带反馈环的多阶段内容生产
- 带验证阶段的研发工作流
- 包含多轮审查的教育内容生成

## 🔧 核心编排模式

### 1. **并行执行 + 质量选择**
```python
import asyncio
from agents import Agent, Runner, trace

with trace("Parallel translation"):
    results = await asyncio.gather(
        Runner.run(translator_agent, message),
        Runner.run(translator_agent, message),
        Runner.run(translator_agent, message)
    )

    best = await Runner.run(selector_agent, combined_results)
```

### 2. **将 Agent 封装为函数工具**
```python
from agents import Agent, function_tool

@function_tool
async def research_tool(topic: str) -> str:
    result = await Runner.run(research_agent, f"Research: {topic}")
    return str(result.final_output)

orchestrator = Agent(
    name="Content Orchestrator",
    tools=[research_tool, writing_tool]
)
```

### 3. **顺序 + 并行混合模式**
```python
with trace("Content Creation Pipeline"):
    # 阶段 1：并行研究
    research_results = await asyncio.gather(
        research_agent_1.run(topic),
        research_agent_2.run(topic)
    )

    # 阶段 2：顺序写作
    content = await writing_agent.run(combined_research)

    # 阶段 3：并行审查
    reviews = await asyncio.gather(
        quality_agent.run(content),
        style_agent.run(content)
    )
```

## 💡 编排设计最佳实践

- **任务拆解**：将复杂任务拆成适合单个 Agent 处理的子任务
- **并行优化**：彼此独立的任务优先考虑并行执行
- **质量控制**：加入 Review、Scoring、Selection 等机制
- **错误处理**：为 Agent 失败准备回退和恢复策略
- **结果综合**：明确多个输出如何组合为最终结果

## 🚨 重要说明

- **Tracing 集成**：建议使用 `trace()` 对完整多 Agent 工作流分组
- **资源管理**：并行调用时注意 API Rate Limit
- **质量与速度**：并行化并不一定始终意味着更高质量，需要合理权衡
- **错误传播**：复杂工作流中应明确处理局部 Agent 失败

## 🔗 后续步骤

完成本教程后，可以继续：
- **[教程 10：Tracing 与可观测性](../10_tracing_observability/README.md)** —— 监控复杂工作流
- **[教程 11：语音 Agent](../11_voice/README.md)** —— 进一步学习实时与语音能力

## 🚨 故障排查

- **性能问题**：检查是否存在不必要的顺序执行
- **结果质量问题**：改进结果综合和筛选逻辑
- **Rate Limit**：对并行请求增加退避和重试机制
- **内存占用**：大量并行 Agent 时监控资源消耗

## 💡 实用建议

- **从简单模式开始**：先掌握基本并行执行，再逐步增加复杂度
- **实际测量性能**：比较并行与顺序执行的总耗时
- **建立质量指标**：明确如何评价并筛选多个结果
- **使用 Tracing 可视化**：通过 Trace 理解复杂工作流的执行路径
- **保持 Agent 专业化**：让每个 Agent 的职责清晰且聚焦
