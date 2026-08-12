# 🎙️ 静态语音 Agent

这是一个基于 OpenAI Agents SDK 的完整语音交互示例，使用预先录制的音频作为输入，演示语音转文字、Agent 处理以及文字转语音组成的基础 Voice Pipeline。

## 🎯 本示例展示的内容

- **静态音频处理**：先完成录音，再统一处理
- **Voice Pipeline**：完整的语音转文字 → Agent → 文字转语音流程
- **多 Agent 系统**：根据语言检测执行 Agent Handoff
- **工具集成**：通过语音触发天气、时间和计算工具
- **音频管理**：录音、播放以及常用音频工具函数

## 🧠 核心概念：静态语音管线

静态语音管线会先获得一段完整录音，再一次性执行后续流程。它更接近传统的**轮次式语音助手**：

- 先录制完整用户语音
- 将整段音频转写为文本
- 使用 Agent 和工具处理请求
- 将完整响应转换为语音
- 播放最终音频响应

```text
用户录音
   │
   ▼
完整音频缓冲区
   │
   ▼
Speech-to-Text
   │
   ▼
Agent / Tool / Handoff
   │
   ▼
Text-to-Speech
   │
   ▼
播放语音响应
```

## 🚀 快速开始

1. **安装语音相关依赖**：
   ```bash
   pip install 'openai-agents[voice]'
   pip install sounddevice numpy soundfile librosa
   ```

2. **配置环境变量**：
   ```bash
   cp env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行静态语音 Agent**：
   ```bash
   python agent.py
   ```

## 🧪 示例包含的能力

### **多语言支持**
- **English Agent**：主 Agent，可使用全部工具
- **Spanish Agent**：专门处理西班牙语
- **French Agent**：专门处理法语
- **自动语言检测**：根据检测到的语言自动执行 Handoff

### **语音触发工具**
- `get_weather(city)`：查询指定城市天气
- `get_time()`：获取当前时间
- `calculate_tip(bill, percentage)`：计算小费

### **音频工具**
- `AudioPlayer`：通过 `sounddevice` 实时播放音频
- `record_audio()`：按指定时长从麦克风录音
- `create_silence()`：生成静音缓冲区
- `save_audio()` / `load_audio()`：保存和读取音频文件

### **Workflow Callback**
- `WorkflowCallbacks`：监控转写、工具调用和 Handoff
- 输出调试信息以观察 Pipeline 执行过程
- 可用于性能统计和状态跟踪

## 🎯 交互示例

### 英文示例
- `Tell me a joke` → Agent 生成幽默回答
- `What's the weather in Tokyo?` → 调用天气工具
- `What time is it?` → 调用时间工具
- `Calculate a 20% tip on a $50 bill` → 执行小费计算

### 语言 Handoff
- `Hola, ¿cómo estás?` → 转交 Spanish Agent
- `Bonjour, comment allez-vous?` → 转交 French Agent
- Agent 会使用检测到的语言继续回复

### 工具调用
- 天气查询可通过不同语言触发
- 时间和计算工具可以由多个 Agent 使用
- Agent 会根据用户请求自动决定是否调用工具

## 🔧 关键实现模式

### 1. Voice Pipeline 配置
```python
pipeline = VoicePipeline(
    workflow=SingleAgentVoiceWorkflow(agent, callbacks=WorkflowCallbacks())
)
```

### 2. 音频输入处理
```python
audio_buffer = record_audio(duration=5.0)
audio_input = AudioInput(buffer=audio_buffer)
result = await pipeline.run(audio_input)
```

### 3. 流式播放音频输出
```python
with AudioPlayer() as player:
    async for event in result.stream():
        if event.type == "voice_stream_event_audio":
            player.add_audio(event.data)
```

### 4. 多 Agent 配置
```python
agent = Agent(
    name="Assistant",
    handoffs=[spanish_agent, french_agent],
    tools=[get_weather, get_time, calculate_tip]
)
```

## 💡 语音 Agent 最佳实践

- **保证录音清晰**：尽量使用质量稳定的麦克风并降低环境噪声
- **Instructions 保持简洁**：语音交互更适合短而明确的响应逻辑
- **做好错误处理**：录音设备、音频流和网络调用都可能失败
- **明确语言切换逻辑**：让 Handoff 条件简单且可预测
- **工具返回适合口语输出**：避免返回过长或难以朗读的结构化文本

## 📊 静态 Pipeline 的特点

### 优势
- **处理过程可预测**：录音时长固定
- **上下文完整**：整段用户语音在处理前已经获得
- **实现简单**：不需要处理实时输入流
- **适合复杂请求**：可以等待用户完整表达后再处理

### 适用场景
- 传统轮次式语音助手
- 语音命令与自动化
- 多语言学习场景
- 应用无障碍语音界面

## 🚨 依赖与系统要求

### 核心依赖
- `openai-agents[voice]`：支持 Voice Pipeline 的 OpenAI Agents SDK
- `sounddevice`：音频录制和播放
- `numpy`：音频数据处理
- `soundfile`：音频文件操作，可选
- `librosa`：音频重采样，可选

### 系统要求
- 麦克风：用于音频输入
- 扬声器或耳机：用于语音输出
- Python 3.8+：用于异步执行

## 🔗 相关示例

- **[Streaming Voice](../streamed/README.md)**：实时流式语音交互
- **[Voice Pipeline Documentation](https://openai.github.io/openai-agents-python/voice/pipeline/)**：官方 Voice Pipeline 文档
- **[Voice Quickstart](https://openai.github.io/openai-agents-python/voice/quickstart/)**：官方快速入门

## 🛠️ 可扩展方向

### 扩展音频能力
- 添加音频滤波或降噪
- 支持更多音频格式
- 增加音频波形或状态可视化

### 扩展 Agent 能力
- 添加更多语言 Agent
- 集成领域专用工具
- 添加 Session 或长期对话记忆

### 改善语音体验
- 加入 Voice Activity Detection
- 实现自定义唤醒词
- 增加情绪或语气识别

## 💡 实用建议

- 运行前先确认麦克风和扬声器工作正常
- 根据使用场景调整录音时长
- 使用 Callback 观察 Pipeline 内部行为
- 正确处理 `Ctrl+C` 和音频资源释放
- 语音回答应尽量简短、自然、适合直接朗读

## 🔗 后续步骤

完成静态语音示例后，可以继续：
- **[Streaming Voice](../streamed/README.md)**：实现实时语音输入输出
- **[Advanced Voice Pipelines](https://openai.github.io/openai-agents-python/voice/pipeline/)**：了解自定义 Voice Pipeline
- 将语音 Agent 集成到实际应用或生产系统中
