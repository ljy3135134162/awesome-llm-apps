# 🎙️ 教程 11：语音 Agent

本教程介绍如何使用 OpenAI Agents SDK 构建支持语音交互的 AI Agent，涵盖语音转文本、文本转语音以及自然对话所需的 Agent 工作流。

## 🎯 你将学到什么

- **语音流水线架构**：完整的语音 ↔ 文本 ↔ 语音工作流
- **静态语音处理**：基于录音的轮次式语音交互
- **流式语音处理**：实时语音对话与音频流
- **多语言支持**：自动语言识别与 Agent Handoff
- **语音优化工具**：专门针对语音交互设计工具
- **音频管理**：录音、播放以及流式音频处理

## 🧠 核心概念：语音 Agent

语音 Agent 将语言模型与语音处理能力结合起来，构建自然的对话式界面。可以把它理解成一个真正可以“说话”的 AI 助手，它能够：

- 监听用户语音并转换为文本
- 使用 Agent 工作流理解并处理请求
- 像文本 Agent 一样调用工具和执行决策
- 将最终响应转换为自然语音
- 连续处理多轮对话

```text
┌─────────────────────────────────────────────────────────────┐
│                      语音 Agent 系统                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🎤 用户语音                                                │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    1. 语音转文本                           │
│  │   音频流水线 │    ◦ 将语音转换为文本                      │
│  │             │    ◦ 支持多语言                            │
│  └─────────────┘                                            │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    2. Agent 处理                            │
│  │   Agent     │    ◦ 多 Agent 工作流                       │
│  │   生态系统   │    ◦ 工具调用与 Handoff                    │
│  └─────────────┘    ◦ 上下文管理                            │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    3. 文本转语音                           │
│  │   语音合成   │    ◦ 将响应转换为语音                      │
│  │             │    ◦ 自然语音输出                          │
│  └─────────────┘                                            │
│       │                                                     │
│       ▼                                                     │
│  🔊 AI 语音响应                                             │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 教程概览

本教程演示三种核心语音交互模式：

### **1. 静态语音处理**（`static/`）
- **轮次式交互**：录音 → 处理 → 响应
- **完整音频处理**：等待整段语音结束后再处理
- **实现更简单**：更容易理解和调试
- **适用场景**：语音命令、结构化交互

### **2. 流式语音处理**（`streamed/`）
- **实时交互**：持续监听并实时响应
- **音频流处理**：音频到达后立即进行处理
- **语音活动检测**：自动检测说话开始和结束
- **适用场景**：自然对话、语音助手

### **3. Realtime 语音处理**（`realtime/`）
- **超低延迟**：基于 WebSocket 的持久连接
- **打断处理**：支持更自然的对话中断
- **Realtime API**：使用 OpenAI 实时语音能力
- **适用场景**：实时对话、低延迟语音交互

## 📁 项目结构

```text
11_voice/
├── README.md                          # 本文件：语音 Agent 总览
├── static/                            # 静态语音处理示例
│   ├── agent.py                       # 完整静态语音 Agent
│   ├── util.py                        # 录音和播放工具
│   ├── requirements.txt               # 静态示例依赖
│   ├── env.example                    # 环境变量
│   └── README.md                      # 静态语音说明
├── streamed/                          # 流式语音处理示例
│   ├── agent.py                       # 实时流式语音 Agent
│   ├── util.py                        # 流式音频工具
│   ├── requirements.txt               # 流式示例依赖
│   ├── env.example                    # 环境变量
│   └── README.md                      # 流式语音说明
├── realtime/                          # Realtime 语音处理示例
│   ├── agent.py                       # 基础 Realtime 语音 Agent
│   ├── requirements.txt               # Realtime 示例依赖
│   ├── env.example                    # 环境变量
│   └── README.md                      # Realtime 语音说明
└── __init__.py                        # 模块初始化
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何构建完整的语音交互流水线
- ✅ 静态语音处理与流式语音处理的区别
- ✅ 如何通过 Handoff 实现多语言语音 Agent
- ✅ 面向语音交互优化 Agent 的设计方式
- ✅ 实时音频处理与流式传输的基本模式

## 🚀 快速开始

### **前置条件**

1. **安装带语音支持的 OpenAI Agents SDK**：
   ```bash
   pip install 'openai-agents[voice]'
   ```

2. **安装音频依赖**：
   ```bash
   pip install sounddevice numpy soundfile librosa
   ```

3. **配置环境变量**：
   ```bash
   cp static/env.example static/.env
   cp streamed/env.example streamed/.env
   # 编辑 .env 并添加 OpenAI API Key
   ```

### **启动方式**

**方式 1：静态语音（推荐初学者）**
```bash
cd static/
python agent.py
```

**方式 2：流式语音（进阶）**
```bash
cd streamed/
python agent.py
```

**方式 3：Realtime 语音（超低延迟）**
```bash
cd realtime/
python agent.py
```

## 🧪 语音 Agent 能力

### **多语言支持**
示例中包含：
- **English Agent**：主要助手，拥有完整工具能力
- **Spanish Agent**：专门处理西班牙语
- **French Agent**：专门处理法语
- **自动语言检测**：根据用户语言自动 Handoff 到对应 Agent

### **语音优化工具**
- `get_weather(city)`：使用适合语音播报的格式返回天气信息
- `get_time()`：自然语言播报当前时间
- `calculate_tip(bill, percentage)`：处理语音形式的小费计算
- `set_reminder(message, minutes)`：语音创建提醒（仅流式示例）
- `get_news_summary()`：适合语音播报的新闻摘要（仅流式示例）

### **音频处理能力**
- **高质量录音**：24 kHz 音频采集
- **实时播放**：低延迟音频输出
- **语音活动检测**：自动识别语音边界（流式模式）
- **错误恢复**：处理音频流水线中的异常

## 🔧 关键语音 Agent 模式

### **1. 基础语音流水线**
```python
from agents.voice import VoicePipeline, SingleAgentVoiceWorkflow

pipeline = VoicePipeline(
    workflow=SingleAgentVoiceWorkflow(agent)
)
```

### **2. 静态音频处理**
```python
from agents.voice import AudioInput

audio_buffer = record_audio(duration=5.0)
audio_input = AudioInput(buffer=audio_buffer)
result = await pipeline.run(audio_input)
```

### **3. 流式音频处理**
```python
from agents.voice import StreamedAudioInput

streamed_input = StreamedAudioInput()
result = await pipeline.run(streamed_input)

# 实时推送音频块
streamed_input.push_audio(audio_chunk)
```

### **4. 多语言 Agent 配置**
```python
spanish_agent = Agent(
    name="Spanish",
    handoff_description="A spanish speaking agent.",
    instructions="Speak in Spanish only..."
)

main_agent = Agent(
    name="Assistant",
    handoffs=[spanish_agent, french_agent],
    instructions="If user speaks Spanish, handoff to Spanish agent..."
)
```

## 💡 语音 Agent 最佳实践

### **面向语音的 Agent 设计**
- **指令简洁**：语音场景下应避免过长 Instructions
- **响应自然**：输出应适合口语，而不是文档式表达
- **工具描述清晰**：工具名称和描述应便于模型在语音上下文中理解
- **明确语言路由**：多语言场景下应清晰定义 Handoff 条件

### **音频质量**
- **使用质量较好的硬件**：麦克风和扬声器会直接影响体验
- **降低背景噪声**：录音环境越安静越好
- **控制输入输出音量**：避免过小或削波
- **优化延迟**：通过合理的音频 Buffer 设置减少交互等待

### **错误处理**
- **优雅处理设备异常**：麦克风或播放设备不可用时不要直接崩溃
- **网络异常处理**：API 调用应具备重试逻辑
- **允许用户中断**：语音会话应支持安全退出和打断
- **释放资源**：及时关闭音频流和相关句柄

## 🧪 示例语音交互

### **英语对话**
- `Tell me a joke` → 返回幽默内容
- `What's the weather in London?` → 调用天气工具
- `What time is it?` → 返回当前时间
- `Calculate a 18% tip on a $75 bill` → 计算小费

### **语言切换**
- `Hola, ¿qué tiempo hace en Madrid?` → 切换到 Spanish Agent
- `Bonjour, quelle heure est-il?` → 切换到 French Agent
- 根据用户语言自动完成 Handoff

### **多轮对话（流式模式）**
- 自然连续对话
- 跨轮次保留上下文
- 在对话过程中调用工具

## 📊 静态语音与流式语音对比

| 特性 | 静态语音 | 流式语音 |
|---|---|---|
| **处理方式** | 轮次式 | 实时 |
| **复杂度** | 较低 | 较高 |
| **延迟** | 较高 | 较低 |
| **典型用途** | 命令、查询 | 连续对话 |
| **语音活动检测** | 手动 | 自动 |
| **资源占用** | 较低 | 较高 |
| **用户体验** | 结构化 | 更自然 |

## 🚨 依赖与系统要求

### **核心依赖**
- `openai-agents[voice]`：带语音支持的 Agents SDK
- `sounddevice`：实时音频输入输出
- `numpy`：音频数据处理
- `soundfile`：音频文件处理（可选）
- `librosa`：音频重采样（可选）

### **系统要求**
- **Python 3.8+**：支持异步执行
- **音频硬件**：麦克风与扬声器/耳机
- **处理能力**：能够承担实时音频处理负载
- **网络**：稳定连接 OpenAI API

## 🔗 相关文档

- **[Voice Quickstart](https://openai.github.io/openai-agents-python/voice/quickstart/)**：官方语音快速开始
- **[Voice Pipelines](https://openai.github.io/openai-agents-python/voice/pipeline/)**：语音流水线配置
- **[Agent 基础](../1_starter_agent/README.md)**：基础 Agent 概念
- **[多 Agent 系统](../9_multi_agent_orchestration/README.md)**：Agent 编排与协作

## 🚨 故障排查

### **音频问题**
- **没有麦克风输入**：检查系统权限和音频设备设置
- **音质较差**：检查麦克风增益和背景噪声
- **无法播放**：确认扬声器或耳机设备配置
- **延迟过高**：优化 Audio Buffer 大小

### **语音流水线问题**
- **转写错误**：确保语音清晰、录音质量足够
- **Agent 无响应**：检查 API Key 和网络连接
- **语言识别异常**：使用明确的语言样例测试
- **Handoff 失败**：检查 Agent Instructions 与 Handoff 逻辑

### **性能问题**
- **CPU 使用率过高**：检查实时音频处理负载
- **内存持续增长**：确认音频流是否正确释放
- **网络超时**：为 API 调用增加合理的重试机制
- **设备冲突**：检查是否有其他程序独占音频设备

## 💡 实用建议

- **先学习静态模式**：理解完整语音流水线后，再进入流式和 Realtime
- **先验证音频环境**：开发前确认麦克风和播放设备工作正常
- **观察调试输出**：使用回调和日志理解流水线行为
- **针对语音优化 Agent**：不要直接照搬文本 Agent 的响应风格
- **考虑异常场景**：提前处理网络异常、音频故障和用户打断

## 🔗 后续方向

完成语音 Agent 学习后，可以继续探索：
- **生产部署**：将语音 Agent 扩展到真实应用
- **自定义语音模型**：集成专用语音识别或语音合成方案
- **多模态 Agent**：结合语音、视觉和文本能力
- **企业语音应用**：构建客服、语音助手等业务系统
