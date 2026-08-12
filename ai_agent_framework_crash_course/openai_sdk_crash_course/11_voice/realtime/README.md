# ⚡ Realtime 实时语音 Agent

本示例使用 OpenAI Realtime API 构建一个基础实时语音 Agent，展示实现超低延迟语音对话所需的核心组件和最小配置。

## 🎯 本示例展示的内容

- **Realtime 核心组件**：`RealtimeAgent`、`RealtimeRunner` 与 `RealtimeSession`
- **基础实时语音对话**：实现超低延迟语音交互
- **函数工具**：在语音对话过程中调用工具
- **Agent Handoff**：将任务转交给专门 Agent
- **事件处理**：处理实时 Session 中的关键事件

## 🧠 核心概念：Realtime 实时语音处理

Realtime Agent 通过 OpenAI Realtime API 提供**超低延迟语音对话**。与传统语音管线不同，它会维持持久 WebSocket 连接，从而持续处理音频并快速响应。可以把 Realtime Agent 理解为一个**实时对话伙伴**：

- 以极低延迟处理音频并生成响应
- 在对话过程中支持自然打断
- 使用持久连接维持连续对话
- 支持实时工具调用与 Agent Handoff
- 在实时生成过程中应用安全 Guardrail

根据 [官方文档](https://openai.github.io/openai-agents-python/realtime/quickstart/)，Realtime Agent 面向自然、低延迟的实时语音交互场景。

```text
┌─────────────────────────────────────────────────────────────┐
│                    REALTIME 语音工作流                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🎤 实时音频输入                                            │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    1. WebSocket 持久连接                   │
│  │  持久连接   │    ◦ 持续传输音频                          │
│  │             │    ◦ 超低延迟处理                          │
│  └─────────────┘    ◦ 实时数据流                            │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    2. 即时处理                             │
│  │ Realtime    │    ◦ 即时语音识别                          │
│  │   Agent     │    ◦ 实时 Agent 推理                       │
│  └─────────────┘    ◦ 实时工具调用                          │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    3. 即时响应                             │
│  │  实时响应   │    ◦ 实时生成语音                          │
│  │             │    ◦ 流式音频输出                          │
│  └─────────────┘    ◦ 支持打断                              │
│       │                                                     │
│       ▼                                                     │
│  🔊 即时音频输出                                            │
│                                                             │
│  ↺ 持续对话循环                                             │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **配置环境变量**：
   ```bash
   cp env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行基础 Realtime Agent**：
   ```bash
   python agent.py
   ```

4. **开始说话**。例如：
   - `What's the weather in Paris?`
   - `Book appointment tomorrow at 2pm`

## 🧪 示例包含的能力

### **Realtime 核心组件**

根据 [官方指南](https://openai.github.io/openai-agents-python/realtime/guide/)：
- **RealtimeAgent**：包含 Instructions、Tools 与 Handoffs 的实时 Agent
- **RealtimeRunner**：负责配置并创建 Realtime Session
- **RealtimeSession**：单次实时会话，并以事件流形式输出状态和数据

### **基础函数工具**
- `get_weather(city)`：返回简单天气信息
- `book_appointment(date, time, service)`：模拟预约服务

### **基础 Agent Handoff**
- **Main Assistant**：通用语音助手
- **Billing Agent**：专门处理账单问题，用于演示 Handoff

### **关键事件处理**
- **Audio Transcript**：用户和 Assistant 的语音转写
- **Tool Call**：工具调用事件
- **Error Event**：基础错误处理

## 🎯 语音交互示例

### **基础对话**
- `What's the weather in Paris?` → 调用天气工具并即时响应
- `Book appointment tomorrow at 2pm` → 调用预约工具

### **Agent Handoff**
- `I need help with billing` → 转交 Billing Agent 处理

## 🔧 关键实现模式

根据 [官方指南](https://openai.github.io/openai-agents-python/realtime/guide/)：

### **1. 创建 RealtimeAgent**
```python
from agents.realtime import RealtimeAgent

agent = RealtimeAgent(
    name="Assistant",
    instructions="You are a helpful voice assistant...",
    tools=[get_weather, book_appointment],
    handoffs=[realtime_handoff(billing_agent)]
)
```

### **2. 配置 RealtimeRunner**
```python
from agents.realtime import RealtimeRunner

runner = RealtimeRunner(
    starting_agent=agent,
    config={
        "model_settings": {
            "model_name": "gpt-4o-realtime-preview",
            "voice": "alloy",
            "modalities": ["text", "audio"]
        }
    }
)
```

### **3. 启动 Session 并处理事件**
```python
session = await runner.run()

async with session:
    async for event in session:
        if event.type == "response.audio_transcript.done":
            print(f"Assistant: {event.transcript}")
```

## 💡 Realtime 基础概念

来自 [官方指南](https://openai.github.io/openai-agents-python/realtime/guide/) 的核心流程包括：

1. **Session 流程**：创建 Agent → 配置 Runner → 启动 Session → 处理事件
2. **事件处理**：监听音频转写、工具调用和错误事件
3. **语音配置**：选择可用 Voice
4. **轮次检测**：通过服务器端 Voice Activity Detection 实现自然对话

## 📊 Realtime 与传统语音管线对比

| 特性 | 传统语音 | Realtime 语音 |
|---|---|---|
| **延迟** | 通常更高 | 更低 |
| **连接方式** | Request / Response | 持久 WebSocket |
| **打断能力** | 较受限 | 更自然 |
| **音频处理** | 批处理 | 流式处理 |
| **工具调用** | 以轮次为主 | 实时事件驱动 |
| **对话流程** | 结构化 | 更自然连续 |
| **API 模式** | REST 请求 | WebSocket 事件 |

## 🌟 高级 Realtime 能力

### **Voice Activity Detection（VAD）**
- **Server VAD**：由服务端完成语音活动检测
- **Threshold**：可调整检测灵敏度
- **Silence Detection**：识别用户是否结束当前轮次
- **Prefix Padding**：保留语音起始位置之前的少量音频，避免截断开头

### **音频配置选项**
- **Voice Selection**：选择不同 Voice
- **Audio Format**：支持 Realtime API 所提供的音频格式
- **Transcription Model**：配置语音转写模型
- **Multi-Modal Support**：支持文本和音频模态

### **Realtime Guardrails**

根据 [官方指南](https://openai.github.io/openai-agents-python/realtime/guide/)，Realtime Guardrail 具有以下特性：
- **Debounced**：不会针对每个字词运行，而是按一定节奏触发
- **Configurable**：可以调整触发频率和阈值
- **Event-Based**：违规通常通过事件暴露，而不是传统同步异常流程
- **Real-Time**：触发后可以对实时响应进行干预

### **Session 事件类型**
- **Audio Events**：例如 `response.audio.delta`、`response.audio.done`
- **Transcription Events**：例如 `response.audio_transcript.done`
- **Tool Events**：函数调用相关事件
- **Lifecycle Events**：Session 与 Response 生命周期事件
- **Error / Guardrail Events**：错误和 Guardrail 触发事件

## 🚨 依赖与系统要求

### **核心依赖**
- `openai-agents`：OpenAI Agents SDK
- `python-dotenv`：环境变量管理
- 使用支持当前 Realtime 功能的 Python 版本

### **API 要求**
- **OpenAI API Key**：调用 Realtime API 所必需
- **Realtime Model Access**：账号需要能够访问对应 Realtime 模型
- **WebSocket**：需要稳定网络支持持久连接

### **系统要求**
- **低延迟网络**：Realtime 场景对网络延迟更加敏感
- **音频硬件**：麦克风与扬声器/耳机
- **处理能力**：需要足够 CPU 处理实时音频输入输出

## 🔧 配置示例

### **Model Settings**
```python
"model_settings": {
    "model_name": "gpt-4o-realtime-preview",
    "voice": "alloy",
    "modalities": ["text", "audio"],
    "input_audio_format": "pcm16",
    "output_audio_format": "pcm16"
}
```

### **Turn Detection**
```python
"turn_detection": {
    "type": "server_vad",
    "threshold": 0.5,
    "prefix_padding_ms": 300,
    "silence_duration_ms": 200
}
```

### **Transcription**
```python
"input_audio_transcription": {
    "model": "whisper-1",
    "language": "en",
    "prompt": "Custom prompt..."
}
```

> 这些模型名和配置字段可能随 SDK 与 Realtime API 版本变化，应以当前官方文档和项目代码为准。

## 🛡️ 安全与 Guardrails

### **实时安全能力**
- **Debounced Processing**：通过节流方式降低 Guardrail 对延迟的影响
- **Immediate Intervention**：需要时可中断或修改实时输出流程
- **Event-Based Alerts**：通过 Guardrail 事件反馈检测结果
- **Configurable Sensitivity**：根据业务需求调整检测策略

### **Guardrail 示例**
```python
@output_guardrail
def sensitive_data_guardrail(ctx, agent, output: str) -> GuardrailFunctionOutput:
    if contains_sensitive_data(output):
        return GuardrailFunctionOutput(
            tripwire_triggered=True,
            output_info="Blocked sensitive data"
        )
    return GuardrailFunctionOutput(tripwire_triggered=False)
```

## 🎯 生产环境注意事项

### **性能优化**
- **Connection Management**：正确维护持久 WebSocket 连接
- **Error Recovery**：实现断线重连与故障恢复
- **Resource Monitoring**：监控 Session 的 CPU、内存与连接资源
- **Event Processing**：优化高频事件处理逻辑

### **扩展模式**
- **Session Isolation**：不同用户使用独立 Realtime Session
- **Load Balancing**：将 Session 分布到多个服务实例
- **Connection Management**：合理管理大量 WebSocket 连接
- **Graceful Shutdown**：关闭服务时正确清理 Session

### **监控与分析**
- **Event Tracking**：记录关键实时事件
- **Performance Metrics**：关注延迟、吞吐量与错误率
- **Conversation Analytics**：分析对话成功率与使用模式
- **Safety Metrics**：跟踪 Guardrail 触发情况

## 🚨 版本变化注意事项

Realtime API 与 Agents SDK 会持续演进，因此生产使用时应重点关注：

- **API Stability**：接口和事件模型可能发生变化
- **Feature Evolution**：新能力可能持续加入
- **Compatibility Testing**：升级 SDK 前应进行回归测试
- **Official Docs**：始终以当前官方文档为准

## 💡 实用建议

- **从简单场景开始**：先验证基础实时语音，再加入复杂工具和多 Agent
- **完整记录事件**：Realtime 问题通常需要通过事件日志定位
- **平衡 Guardrail 与延迟**：安全检查应兼顾实时体验
- **重点测试打断**：验证用户插话和对话切换行为
- **提前考虑扩展性**：生产环境应设计 Session 与连接管理策略

## 🔗 相关文档

- **[Realtime Quickstart](https://openai.github.io/openai-agents-python/realtime/quickstart/)**：官方快速入门
- **[Realtime Guide](https://openai.github.io/openai-agents-python/realtime/guide/)**：Realtime 完整指南
- **[Voice Agents](../README.md)**：语音 Agent 总览
- **[Agent Fundamentals](../../1_starter_agent/README.md)**：基础 Agent 概念

## 🎯 故障排查

### **常见问题**
- **延迟较高**：检查网络延迟和 WebSocket 稳定性
- **音频质量较差**：检查麦克风、采样率与音频格式
- **事件处理异常**：记录并检查事件流
- **Guardrail 影响性能**：调整检测策略和触发频率
- **模型不可用**：确认账号是否拥有对应 Realtime 模型访问权限

### **调试方向**
- **Event Logging**：记录完整事件流
- **Connection Monitoring**：监控 WebSocket 连接状态
- **Performance Profiling**：监控 CPU、内存和事件处理延迟
- **Audio Pipeline**：单独验证音频输入和输出链路

## 🚀 后续步骤

掌握 Realtime Voice Agent 后，可以继续探索：
- **生产部署**：构建可扩展的 Realtime Voice 服务
- **自定义集成**：将实时语音接入现有业务系统
- **高级 Realtime 功能**：探索更复杂的实时 Agent 工作流
- **多模态体验**：将实时语音与文本、视觉等模态结合
