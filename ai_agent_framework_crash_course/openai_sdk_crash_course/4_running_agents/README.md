# 🚀 教程 4：运行 Agent

本教程系统讲解 OpenAI Agents SDK 的执行机制，包括同步、异步、流式执行、Agent Loop、异常处理以及高级运行配置。

## 🎯 你将学到什么

- **三种执行方式**：`Runner.run()`、`Runner.run_sync()`、`Runner.run_streamed()`
- **Agent Loop**：理解 LLM 调用、工具执行与 Handoff
- **流式事件**：实时处理模型输出和运行事件
- **异常处理**：正确处理 SDK 中的常见异常
- **高级运行配置**：Guardrail、Tracing 与工作流控制

## 🧠 核心概念：Agent Loop

调用任意 Runner 方法后，SDK 都会进入一个完整的 Agent 执行循环：

```text
┌─────────────────────────────────────────────────────────────┐
│                       Agent Loop                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  开始：Runner.run(agent, input)                             │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    1. 调用 LLM                             │
│  │     LLM     │    ◦ 当前 Agent + 输入                     │
│  │    调用     │    ◦ 生成响应                              │
│  └─────────────┘                                            │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    2. 分析输出                              │
│  │    输出     │    ◦ 最终结果？→ 结束                      │
│  │    分析     │    ◦ 工具调用？→ 执行工具                  │
│  └─────────────┘    ◦ Handoff？→ 切换 Agent                 │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    3. 继续循环                              │
│  │    重复     │    ◦ 将工具结果追加到上下文                │
│  │    执行     │    ◦ 检查 max_turns                        │
│  └─────────────┘    ◦ 直到获得最终输出                      │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 教程概览

本教程包含五种关键运行模式：

### **1. 执行方式**（`4_1_execution_methods/`）
- 同步、异步和流式执行对比
- 性能与适用场景分析
- 基础 Agent Loop

### **2. 对话管理**（`4_2_conversation_management/`）
- 使用 `to_input_list()` 手动维护对话
- 使用 Sessions 自动管理会话
- Thread ID 与分组管理

### **3. Run 配置**（`4_3_run_configuration/`）
- 模型覆盖与模型设置
- Tracing 配置和元数据
- 工作流命名与组织

### **4. 流式事件**（`4_4_streaming_events/`）
- 详细的流式事件处理
- `RunResultStreaming` 用法
- 实时响应处理模式

### **5. 异常处理**（`4_5_exception_handling/`）
- `MaxTurnsExceeded`、`ModelBehaviorError` 等 SDK 异常
- 正确的错误处理模式
- 恢复与重试策略

## 📁 项目结构

```text
4_running_agents/
├── README.md                           # 本文件：完整指南
├── requirements.txt                    # 依赖
├── 4_1_execution_methods/
│   ├── __init__.py
│   └── agent.py                        # 三种执行方式
├── 4_2_conversation_management/
│   ├── __init__.py
│   └── agent.py                        # 手动与自动对话管理
├── 4_3_run_configuration/
│   ├── __init__.py
│   └── agent.py                        # RunConfig 示例
├── 4_4_streaming_events/
│   ├── __init__.py
│   └── agent.py                        # 流式事件处理
├── 4_5_exception_handling/
│   ├── __init__.py
│   └── agent.py                        # 异常处理示例
├── agent_runner.py                     # Streamlit 演示界面
└── env.example                         # 环境变量模板
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ Agent 完整执行循环以及每一步发生的时机
- ✅ 如何在同步、异步和流式执行之间做选择
- ✅ 如何处理实时流式事件
- ✅ 如何为生产应用正确处理异常
- ✅ 如何配置复杂工作流的运行参数

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

4. **测试执行方式**：
   ```bash
   python -m 4_1_execution_methods.agent
   ```

5. **测试对话管理**：
   ```bash
   python -m 4_2_conversation_management.agent
   ```

6. **查看 Run 配置**：
   ```bash
   python -m 4_3_run_configuration.agent
   ```

7. **测试流式事件**：
   ```bash
   python -m 4_4_streaming_events.agent
   ```

8. **测试异常处理**：
   ```bash
   python -m 4_5_exception_handling.agent
   ```

## 🔧 关键运行概念

### 1. **Agent Loop 流程**
- **LLM 调用**：Agent 处理输入并生成下一步结果
- **输出分析**：判断结果是最终输出、工具调用还是 Handoff
- **工具执行**：执行工具并将结果追加到上下文
- **Handoff 处理**：必要时切换到新的 Agent
- **继续循环**：直到得到最终结果或达到 `max_turns`

### 2. **三种执行方式**
```python
# 1. 异步执行：非阻塞，返回 RunResult
result = await Runner.run(agent, "message")

# 2. 同步执行：阻塞式，内部封装异步运行
result = Runner.run_sync(agent, "message")

# 3. 流式执行：返回 RunResultStreaming
result = Runner.run_streamed(agent, "message")
async for event in result.stream_events():
    # 实时处理事件
    pass
```

### 3. **流式事件**
流式运行可以实时获得模型生成过程中的事件，例如文本增量、工具调用以及最终完成事件。

### 4. **异常体系**
- **AgentsException**：SDK 基础异常类
- **MaxTurnsExceeded**：超过最大循环次数
- **ModelBehaviorError**：模型输出不符合预期，例如格式错误
- **UserError**：SDK 使用方式错误
- **InputGuardrailTripwireTriggered**：输入 Guardrail 触发
- **OutputGuardrailTripwireTriggered**：输出 Guardrail 触发

## 🧪 常见应用场景

### 执行方式
- **同步**：脚本、批处理、简单请求
- **异步**：Web 服务、并发请求、非阻塞应用
- **流式**：实时聊天、长文本生成、需要即时反馈的界面

### 对话管理
- **手动管理**：需要自定义对话拼接和线程逻辑的应用
- **Sessions**：标准聊天应用和自动历史管理

### 异常处理
- **生产环境**：优雅恢复错误并向用户返回可理解的信息
- **开发环境**：调试 Agent 行为并定位失败原因

## 💡 最佳实践

- **按场景选择执行方式**：脚本用 Sync，应用用 Async，实时交互用 Streaming
- **处理明确的异常类型**：不要只捕获通用 Exception
- **统一 RunConfig**：生产环境中集中管理模型、Tracing 和运行参数
- **监控性能**：关注调用时间、循环次数以及资源消耗
- **合理管理对话**：根据需求选择手动历史管理或 Sessions

## 🔗 后续步骤

完成本教程后，可以继续：
- **[教程 5：上下文管理](../5_context_management/README.md)** —— 高级状态管理
- **[教程 6：Guardrail 与验证](../6_guardrails_validation/README.md)** —— 输入/输出安全控制
- **[教程 7：Sessions](../7_sessions/README.md)** —— 记忆与对话管理

## 🚨 故障排查

- **Async 问题**：`Runner.run()` 需要配合 `await` 使用
- **Streaming 问题**：正确处理增量事件和连接中断
- **异常处理**：优先捕获明确的 SDK 异常类型
- **性能问题**：检查 `max_turns`，避免意外的无限循环
- **配置问题**：确认 RunConfig 与当前工作流需求一致

## 💡 实用建议

- **从 `run_sync` 开始**：理解 Agent 行为后再引入异步并发
- **需要实时输出时使用 Streaming**：尤其适合聊天界面和长响应
- **提前设计异常策略**：生产环境中应明确各类失败的处理方式
- **保持配置一致**：使用 RunConfig 管理可重复的执行策略
- **使用 Tracing 观察 Agent Loop**：复杂 Agent 工作流尤其有用
