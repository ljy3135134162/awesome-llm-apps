# 🔍 教程 10：Tracing 与可观测性

本教程介绍 OpenAI Agents SDK 内置的 Tracing 系统，学习如何可视化、调试和监控 Agent 工作流，并将这些能力应用到开发和生产环境中。

## 🎯 你将学到什么

- **内置 Tracing**：自动捕获 LLM 生成、工具调用和 Handoff
- **Trace 与 Span**：理解工作流结构与执行过程
- **自定义 Tracing**：为复杂工作流创建自定义 Trace 和 Span
- **生产监控**：用于调试、性能分析和优化

## 🧠 核心概念：什么是 Tracing？

Tracing 提供**完整的工作流可观测性**，可以自动记录 Agent 执行期间的重要事件，包括：

- **LLM Generation**：模型调用、输入、输出和执行表现
- **Tool Call**：函数执行、参数与结果
- **Handoff**：Agent 之间的任务移交和上下文传递
- **Guardrail**：输入与输出验证事件
- **自定义事件**：业务逻辑中的自定义监控点

```text
┌─────────────────────────────────────────────────────────────┐
│                       Tracing 架构                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Agent 工作流                                               │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    自动捕获                                │
│  │    TRACE    │◀─────────────────────────────────────────┐ │
│  │  （工作流） │                                          │ │
│  └─────────────┘                                          │ │
│       │                                                   │ │
│       ▼                                                   │ │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    │ │
│  │    SPAN     │    │    SPAN     │    │    SPAN     │    │ │
│  │  LLM 调用   │    │  工具调用   │    │   Handoff   │    │ │
│  └─────────────┘    └─────────────┘    └─────────────┘    │ │
│       │                    │                    │         │ │
│       ▼                    ▼                    ▼         │ │
│  ┌─────────────────────────────────────────────────────┐  │ │
│  │             OpenAI Traces Dashboard                │  │ │
│  │    • 执行流程可视化                                 │  │ │
│  │    • 性能指标                                       │  │ │
│  │    • 调试信息                                       │  │ │
│  └─────────────────────────────────────────────────────┘  │ │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 教程概览

本教程演示三种主要 Tracing 模式：

### **1. 默认 Tracing**（`default_tracing.py`）
- SDK 默认启用的自动 Tracing
- 理解 Trace 和 Span 的结构
- 基础工作流监控

### **2. 自定义 Tracing**（`custom_tracing.py`）
- 为多阶段工作流创建自定义 Trace
- 添加自定义 Span 作为监控点
- 将多个 Agent Run 归入同一个 Trace

### **3. 高级可观测性**（`advanced_observability.py`）
- 敏感数据处理与配置
- 为外部系统编写自定义 Trace Processor
- 生产环境监控模式

## 📁 项目结构

```text
10_tracing_observability/
├── README.md                    # 本文件：概念说明
├── requirements.txt             # 依赖
├── default_tracing.py           # 内置 Tracing 基础
├── custom_tracing.py            # 自定义 Trace 与 Span
├── advanced_observability.py    # 生产环境 Tracing 模式
├── app.py                       # Streamlit Tracing 演示（可选）
└── env.example                  # 环境变量模板
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 内置 Tracing 如何自动捕获 Agent 工作流事件
- ✅ Trace（工作流）和 Span（操作）之间的区别
- ✅ 如何为复杂多阶段工作流创建自定义 Trace
- ✅ 如何监控和调试生产环境中的 Agent 性能
- ✅ 如何与外部可观测性系统集成

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

4. **测试默认 Tracing**：
   ```bash
   python default_tracing.py
   ```

5. **测试自定义 Tracing**：
   ```bash
   python custom_tracing.py
   ```

6. **探索高级模式**：
   ```bash
   python advanced_observability.py
   ```

## 🧪 示例场景

### 默认 Tracing
- 自动监控基础 Agent 工作流
- 调试工具调用失败和 LLM 输出问题
- 跟踪性能指标并寻找优化点

### 自定义 Tracing
- 将相关 Agent Run 归并到一个复杂工作流中
- 在业务逻辑中添加自定义监控点
- 创建分层 Span 结构用于问题定位

### 高级可观测性
- 配置敏感数据处理以满足合规要求
- 将 Trace 导出到外部监控系统
- 构建生产告警和 Dashboard

## 🔧 常见 Tracing 模式

### 1. **默认 Tracing（自动）**
```python
from agents import Agent, Runner

agent = Agent(name="Assistant")
# 默认会自动记录 Trace，无需额外配置
result = await Runner.run(agent, "Hello")
# 可在 OpenAI Platform 的 Traces 页面查看
```

### 2. **自定义 Trace**
```python
from agents import Agent, Runner, trace

with trace("Multi-step Workflow") as my_trace:
    result1 = await Runner.run(agent, "Step 1")
    result2 = await Runner.run(agent, "Step 2")
    # 两次运行属于同一个 Trace
```

### 3. **自定义 Span**
```python
from agents import custom_span

with custom_span("Data Processing") as span:
    data = process_data()
    span.add_event("Processing complete", {"records": len(data)})
```

## 💡 Tracing 设计最佳实践

- **使用有意义的名称**：为 Trace 和 Span 使用清晰、可检索的命名
- **合理分组**：将属于同一业务流程的操作放入同一个 Trace
- **记录关键业务节点**：使用自定义 Span 补充 SDK 自动记录之外的信息
- **注意敏感数据**：生产环境中应明确配置数据采集边界
- **关注性能指标**：持续跟踪耗时和资源使用趋势

## 🚨 重要说明

- **默认启用**：OpenAI Agents SDK 默认启用 Tracing
- **ZDR 组织限制**：采用 Zero Data Retention 策略的组织无法使用 OpenAI 托管的 Tracing
- **可视化 Dashboard**：可在 OpenAI Platform 中查看 Trace
- **可关闭**：设置 `OPENAI_AGENTS_DISABLE_TRACING=1` 可以禁用 Tracing

## 🔗 后续步骤

完成本教程后，可以继续：
- **[教程 11：Voice](../11_voice/README.md)** —— 学习语音 Agent 与实时交互
- 将 Tracing 与前面的 Handoff、多 Agent 和 Guardrail 工作流组合使用
- 为生产系统接入现有的日志、指标和可观测性平台

## 🚨 故障排查

- **看不到 Trace**：检查 OpenAI API Key、网络连接以及 Tracing 是否被禁用
- **Span 缺失**：确认相关操作位于 Trace 上下文内部
- **性能问题**：检查 Trace 内容和敏感数据采集配置
- **ZDR 策略**：如果组织启用了 ZDR，需要关闭托管 Tracing 或使用自定义 Processor

## 💡 实用建议

- **先使用默认 Tracing**：只有在确有需要时再增加自定义 Trace
- **统一命名规范**：方便搜索、聚合和排障
- **持续观察性能趋势**：不要只在发生故障时查看 Trace
- **外部集成**：大型系统可以通过自定义 Processor 接入现有监控栈
- **区分开发与生产配置**：两个环境通常需要不同的数据采集策略
