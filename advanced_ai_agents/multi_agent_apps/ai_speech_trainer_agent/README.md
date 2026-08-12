# AI 演讲训练 Agent

## 概述
AI Speech Trainer 是一个由 AI 驱动的多 Agent、多模态公众演讲教练。它会聆听你的说话方式、观察你的表达方式，并评估你所说的内容，帮助你逐步成为更自信的公众演讲者。

无论你是在准备 TED 演讲、面试还是学校展示，AI Speech Trainer 都可以提供个性化反馈，指出你的优势和不足，并给出有价值的改进建议，帮助你表达得更好、更清晰、更有自信。

本项目是 **Global Agent Hackathon（2025 年 5 月）** 的参赛作品之一。它利用多 Agent 协作、实时反馈以及多模态分析能力，帮助用户提升演讲表现与沟通效果。

## 功能特性
### 核心功能
- **面部表情分析**：识别情绪并估算眼神交流情况
- **音频分析**：分析语速、音高、清晰度以及填充词
- **内容评估**：基于 GPT 对结构、语气和清晰度提供反馈
- **个性化反馈**：提供平均得分、总体评价、优势、不足以及改进建议

### Agent 组成
- **Facial Agent**：分析面部表情、参与感和眼神交流
- **Vocal Agent**：检测语音问题，例如语速、填充词和音高
- **Content Agent**：使用 LLM 评估并改进内容清晰度
- **Feedback Agent**：根据其他 Agent 的输出，并结合评分标准对演讲者进行评估
- **Coordinator Agent**：负责协调所有 Agent，并汇总分析与反馈结果

## 工作原理
### **用户流程**
1. 用户打开 Streamlit 应用，并上传一段自己练习演讲或展示的视频。

2. 多个 Agent 开始协同工作：

- Facial Agent 分析面部表情和眼神交流。
- Vocal Agent 转录语音并分析声音特征。
- Content Agent 评估语法、结构和连贯性。
- Feedback Agent 对演讲的整体有效性进行评价。
- Coordinator Agent 汇总所有 Agent 的分析结果。

最终，AI Speech Trainer 会生成一份详细反馈报告，其中包含基于评分标准的分数和总结性反馈。

### **核心能力**
- 使用 OpenCV、DeepFace 和 Mediapipe Landmark 进行面部情绪识别。
- 语音转录和语音分析。
- 使用基于 GPT 的反馈进行内容分析。
- 汇总评分和反馈总结。

### **多模态能力**
- **音频**：演讲语音输入与声音质量分析。
- **视频**：面部表情跟踪和反馈。
- **文本**：基于 GPT 对结构、清晰度和语气进行反馈。

## 技术栈
### AI/ML 工具
- **Agno**：用于构建多 Agent 协作和协调机制。
- **Facial Expression Tool**：自定义面部情绪分析工具。
- **Voice Analysis Tool**：自定义语音转录和分析工具。
- **Together API（Llama-3.3-70B-Instruct-Turbo-Free）**：用于内容分析和反馈生成的 LLM。

### 应用框架
- **Streamlit**：用于构建前端用户界面。
- **FastAPI**：用于构建后端 API。

### 编程语言与依赖
- **Python**：后端逻辑和 Agent 实现的核心语言。
- **OpenCV + DeepFace + Mediapipe**：用于面部表情分析。
- **MoviePy + Faster-Whisper + Librosa**：用于语音分析。

## UI 设计
界面基于 Streamlit 构建，包括：

- 首页：包含视频上传区域、操作按钮以及 Transcript 显示区域。
- 反馈页：展示评估分数、详细反馈、优势、不足、改进建议以及性能图表。

## 界面与架构图
### 高层架构
<img src="visuals/ai_speech_trainer.drawio.png">

### 首页
<img src="visuals/home.png">

### 反馈页
<img src="visuals/feedback.png">

## 安装与运行
### 1. 克隆仓库
```sh
git clone https://github.com/aminajavaid30/ai_speech_trainer.git
cd ai_speech_trainer
```

### 2. 安装依赖
```sh
pip install -r requirements.txt
```

### 3. **添加 API Key** —— 创建 `.env` 文件并写入：
```sh
TOGETHER_API_KEY=...
```

### 4. 启动后端
进入 **backend** 目录并运行：
```sh
uvicorn main:app --reload
```

### 5. 启动应用
进入 **frontend** 目录并运行：
```sh
streamlit run Home.py
```

## 团队信息
- **Team Lead**：https://github.com/aminajavaid30 —— Agentic System Designer and Developer
- **Team Members**：https://github.com/aminajavaid30 —— 个人项目
- **背景/经验**：数据科学家，具有软件工程和 Web 开发背景，并具备 AI 产品和 Agentic Workflow 开发经验。

## 演示视频
https://youtu.be/Sb0JPUpJTGQ

## 目录结构
```sh
/backend
  /agents
    /tools
      - facial_expression_tool.py
      - voice_analysis_tool.py
    - content_analysis_agent.py
    - coordinator_agent.py
    - facial_expression_agent.py
    - feedback_agent.py
    - voice_analysis_agent.py
  main.py (FastAPI App)
/frontend
  /pages
    - 1 - Feedback.py
  Home.py
  page_config.py
  sidebar.py
  style.css
.env
LICENSE
README.md
requirements.txt
```

## 补充说明
- 本项目旨在充分利用 **Agno** 作为 AI Agent 开发平台的能力。它展示了多个协作 Agent 如何作为一个团队无缝配合，以解决现实问题——分析用户的演讲展示，并提供综合评估与反馈，从而提升其公众演讲能力。每个 Agent 都拥有明确的目标，并通过团队协作解决复杂的多模态任务。

- 本项目还有较大的扩展空间，可以作为更完整、更实用的 Agentic System 的起点。后续可以加入：
  1. 实时视频录制功能，并通过不同角色场景加入对话式训练能力。
  2. 使用 AI Avatar 重现用户演讲，帮助用户学习和理解更好的表达方式。
  3. 保存用户历史会话记录。
  4. 添加表现仪表盘，用于长期跟踪用户的训练效果。

   这些能力都可以通过增加具有明确目标的专用 Agent 来实现。

## 局限性
- 使用 **Together API** 的 **meta-llama/Llama-3.3-70B-Instruct-Turbo-Free** 模型时，Token 上限较小，因此更适合处理较短的视频片段（约 15-30 秒）。
- 如果需要处理更长的视频，可以改用其他 LLM，并记得在 `.env` 文件中加入相应的 API Key。

## 致谢
本项目使用 Agno、Streamlit、Together API 和 FastAPI 构建，并参加 **#GlobalAgentHackathonMay2025**。