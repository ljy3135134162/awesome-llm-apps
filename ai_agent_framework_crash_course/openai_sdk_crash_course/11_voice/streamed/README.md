# 🌊 流式语音 Agent

本示例使用 OpenAI Agents SDK 构建实时流式语音交互，展示持续音频流、语音活动检测、实时处理以及多轮对话管理等高级语音能力。

## 🎯 本示例展示的内容

- **实时音频处理**：持续进行音频输入和输出流传输
- **语音活动检测**：自动识别用户开始和停止说话
- **对话轮次管理**：智能管理多轮语音交互
- **实时 Agent 处理**：在对话过程中实时生成 Agent 响应
- **中断处理**：利用生命周期事件管理对话流程
- **流式回调**：实时监控和调试语音工作流

## 🧠 核心概念：流式语音管线

流式语音管线会持续实时处理音频。可以将它理解为一个**实时对话助手**：

- 持续监听音频输入
- 自动检测用户何时开始和结束说话
- 使用 AI Agent 实时处理语音内容
- 在响应生成过程中持续输出语音
- 自动管理多轮对话

```text
┌─────────────────────────────────────────────────────────────┐
│                     流式语音工作流                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🎤 持续音频输入                                            │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    1. 实时采集                             │
│  │  流式音频   │    ◦ 持续获取麦克风输入                    │
│  │   录音器    │    ◦ 按音频块处理                          │
│  │             │    ◦ 语音活动检测                          │
│  └─────────────┘                                            │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    2. 实时转写                             │
│  │  流式语音   │    ◦ 实时 Speech-to-Text                   │
│  │    转写     │    ◦ 检测对话轮次边界                      │
│  └─────────────┘                                            │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    3. 并发处理                             │
│  │  Agent 并发 │    ◦ 执行 Agent 工作流                     │
│  │    执行     │    ◦ 工具调用与 Handoff                    │
│  └─────────────┘    ◦ Session 内支持多个轮次                │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    4. 流式响应                             │
│  │  实时 TTS   │    ◦ 实时 Text-to-Speech                   │
│  │    播放     │    ◦ 分块输出音频                          │
│  └─────────────┘    ◦ 尽快开始播放响应                      │
│       │                                                     │
│       ▼                                                     │
│  🔊 持续音频输出                                            │
│                                                             │
│  ↺ 持续循环，支持多轮对话                                   │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 快速开始

1. **安装语音依赖**：
   ```bash
   pip install 'openai-agents[voice]'
   pip install sounddevice numpy soundfile librosa
   ```

2. **配置环境变量**：
   ```bash
   cp env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行流式语音 Agent**：
   ```bash
   python agent.py
   ```

4. **开始说话**：Agent 会自动检测语音并实时响应。

## 🧪 示例包含的能力

### **实时音频管理**
- **StreamedAudioRecorder**：使用线程持续获取麦克风输入
- **AudioPlayer**：管理实时音频播放
- **Activity Detection**：自动检测语音开始与结束
- **Turn-Based Processing**：管理连续的对话轮次

### **高级 Agent 能力**
- **多语言支持**：英语、西班牙语和法语 Agent
- **增强工具集**：天气、时间、提醒和新闻工具
- **实时 Handoff**：流式交互期间根据语言进行 Agent 切换
- **Session 管理**：跟踪多轮对话

### **流式工具**
- `get_weather(city)`：获取天气信息
- `get_time()`：获取当前时间
- `set_reminder(message, minutes)`：演示提醒功能
- `get_news_summary()`：模拟新闻摘要

### **高级监控**
- **StreamingWorkflowCallbacks**：实时监控工作流事件
- **VoiceSessionManager**：管理 Session 生命周期
- **Turn Tracking**：统计和分析对话轮次
- **Lifecycle Events**：处理轮次开始与结束事件

## 🎯 交互示例

### **自然对话流程**
- 开始说话 → Agent 自动检测到语音
- 停顿 → Agent 开始处理并响应
- 继续说话 → 自动进入下一轮
- 同一个 Session 中可以持续进行多轮对话

### **实时工具调用**
- `What's the weather in New York?` → 调用天气工具并响应
- `What time is it?` → 返回当前时间
- `Set a reminder to call Sarah in 15 minutes` → 创建提醒示例
- `Give me a news summary` → 返回新闻摘要示例

### **实时语言切换**
- 使用英语 → English Agent 响应
- 切换到 `¿Qué tiempo hace en Madrid?` → Spanish Agent 接管
- 切换到 `Quelle heure est-il?` → French Agent 响应
- 通过语言检测和 Handoff 实现自然切换

## 🔧 关键实现模式

### **1. 配置流式语音管线**
```python
pipeline = VoicePipeline(
    workflow=SingleAgentVoiceWorkflow(agent, callbacks=StreamingWorkflowCallbacks())
)
```

### **2. 持续音频输入**
```python
with StreamedAudioRecorder() as recorder:
    streamed_input = StreamedAudioInput()

    while session_active:
        if recorder.has_audio():
            audio_chunk = recorder.get_audio_chunk()
            streamed_input.push_audio(audio_chunk)
```

### **3. 实时音频输出**
```python
with AudioPlayer() as player:
    async for event in result.stream():
        if event.type == "voice_stream_event_audio":
            player.add_audio(event.data)
        elif event.type == "voice_stream_event_lifecycle":
            handle_turn_events(event)
```

### **4. Session 管理**
```python
class VoiceSessionManager:
    async def start_session(self):
        # 并发处理输入和输出
        input_task = asyncio.create_task(self._process_audio_input())
        output_task = asyncio.create_task(self._process_audio_output())
        await asyncio.gather(input_task, output_task)
```

## 💡 流式语音最佳实践

1. **语音活动检测**：尽量让语音管线自动处理 Speech Activity Detection
2. **轮次管理**：利用生命周期事件控制对话流程
3. **并发处理**：同时处理输入和输出流
4. **缓冲区管理**：在不同轮次之间合理加入静音缓冲，使交互更自然
5. **错误恢复**：为流式处理失败设计可靠的异常恢复机制
6. **资源管理**：正确释放音频流和相关资源

## 📊 性能特征

### **流式管线优势**
- **实时交互**：用户说话后能够快速开始处理
- **自然对话**：支持持续、连续的语音交互
- **活动检测**：自动识别轮次边界
- **并发处理**：输入和输出可并行运行
- **多轮扩展**：能够高效处理连续对话

### **技术优势**
- **低延迟**：降低语音输入到响应之间的等待时间
- **自适应**：能够处理不同的说话节奏
- **健壮性**：可结合错误恢复机制持续运行
- **高效**：通过分块音频处理降低实时处理压力

## 🌊 流式处理特性

### **自动轮次检测**
- **Speech Activity Detection**：自动检测用户开始说话
- **Silence Detection**：检测用户结束说话
- **Turn Boundaries**：确定对话轮次边界
- **Continuous Listening**：持续等待下一次输入

### **实时处理**
- **Live Transcription**：实时进行语音转文本
- **Streaming Agent Response**：以流式方式处理 Agent 响应
- **Immediate Audio Output**：响应生成时即可开始语音输出
- **Parallel Operations**：多个处理任务并行执行

### **生命周期管理**
- **Turn Events**：监听 `turn_started` 和 `turn_ended`
- **Session Tracking**：统计多轮对话
- **State Management**：管理资源和状态
- **Interruption Handling**：处理用户中断等情况

## 🚨 依赖与系统要求

### **核心依赖**
- `openai-agents[voice]`：带语音支持的 OpenAI Agents SDK
- `sounddevice`：实时音频输入/输出
- `numpy`：音频数据处理
- `threading`：并发音频处理
- `asyncio`：异步管线管理

### **系统要求**
- **低延迟音频设备**：用于实时音频输入和播放
- **麦克风**：建议使用质量较好的麦克风以提高语音检测效果
- **处理能力**：CPU 需要能够承担实时音频处理负载
- **网络**：需要稳定网络连接进行流式 API 调用

## 🔗 相关示例

- **[静态语音](../static/README.md)**：基于轮次的语音交互
- **[Voice Pipeline 官方文档](https://openai.github.io/openai-agents-python/voice/pipeline/)**：语音管线配置
- **[Voice Quickstart](https://openai.github.io/openai-agents-python/voice/quickstart/)**：语音事件处理与快速入门

## 🛠️ 高级自定义

### **自定义语音活动检测**
- 实现自定义语音检测算法
- 添加语音活动阈值
- 调整静音检测参数

### **增强 Session 管理**
- 在不同 Session 之间加入对话记忆
- 实现用户认证
- 添加对话日志和分析

### **实时功能扩展**
- 显示实时转写文本
- 添加实时情感分析
- 加入语音情绪检测

## 🚨 流式语音注意事项

### **中断处理**

如果当前 SDK/示例没有直接提供完整的内置打断机制，可以利用生命周期事件协调输入输出，例如：
- AI 响应期间（`turn_started`）暂时静音麦克风
- 响应结束后（`turn_ended`）重新启用麦克风
- 根据应用需求实现更完整的用户打断逻辑

### **性能优化方向**
- **Buffer Size**：在延迟和音质之间调整音频块大小
- **Concurrency**：合理控制并发任务和线程数量
- **Memory Management**：及时清理不再需要的音频缓冲区
- **Network Handling**：为 API 调用失败和网络波动设计恢复机制

## 💡 实用建议

- **先从基础流式功能开始**：确认稳定后再逐步加入复杂功能
- **监控生命周期事件**：利用 Callback 理解轮次切换过程
- **测试音频硬件**：实时语音对硬件延迟较敏感
- **处理边界情况**：考虑断网、音频设备异常等情况
- **针对语音设计 Agent**：响应应尽量自然、简洁、适合朗读

## 🔗 后续步骤

掌握流式语音 Agent 后，可以继续探索：
- **生产部署**：将实时语音能力部署到实际应用
- **自定义语音模型**：集成专用语音识别或语音合成模型
- **多模态 Agent**：结合语音、视觉和文本能力
- **企业语音解决方案**：构建更可靠的业务级语音应用

## 🎯 故障排查

- **音频延迟较高**：检查音频设备以及 Buffer 配置
- **语音检测不稳定**：调整麦克风输入音量和环境噪声
- **轮次管理异常**：通过生命周期事件定位问题
- **资源占用过高**：监控 CPU 和内存使用情况
- **网络问题**：为 API 调用添加适当的失败恢复逻辑
