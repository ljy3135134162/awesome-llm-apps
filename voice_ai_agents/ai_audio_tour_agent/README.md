# 🎧 自助式 AI 语音导览 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-self-guided-ai-audio-tour-agent)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个对话式语音 Agent 系统，可根据用户的**所在地**、**兴趣领域**和**导览时长**，生成沉浸式的自助语音导览内容。系统基于多 Agent 架构构建，使用 OpenAI Agents SDK、实时信息检索以及富有表现力的 TTS，实现自然的语音输出。

---

## 🚀 功能特性

### 🎙️ 多 Agent 架构

- **Orchestrator Agent（编排 Agent）**  
  协调整体导览流程，管理不同环节之间的切换，并整合各个专家 Agent 提供的内容。

- **History Agent（历史 Agent）**  
  以权威、清晰的方式讲述相关历史背景与故事。

- **Architecture Agent（建筑 Agent）**  
  使用更具描述性和技术性的表达方式，介绍建筑细节、风格与设计元素。

- **Culture Agent（文化 Agent）**  
  以富有感染力的方式介绍当地习俗、传统与艺术遗产。

- **Culinary Agent（美食 Agent）**  
  生动介绍代表性菜肴及当地饮食文化。

---

### 📍 基于位置的内容生成

- 根据用户输入的**地点**动态生成内容
- 集成实时 **Web 搜索**，获取相关且较新的信息
- 根据用户选择的**兴趣类别**对内容进行个性化筛选

---

### ⏱️ 可自定义导览时长

- 可选择 **15、30 或 60 分钟**的导览长度
- 各部分时间分配会根据用户兴趣权重和地点相关性进行调整
- 确保不同部分之间的叙述节奏合理、比例协调

---

### 🔊 富有表现力的语音输出

- 使用 **GPT-4o Mini Audio** 生成高质量语音

### 如何开始？

1. 克隆 GitHub 仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd voice_ai_agents/ai_audio_tour_agent
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key：

- 注册 [OpenAI](https://platform.openai.com/) 账号（或你选择的其他 LLM Provider）并获取 API Key。

4. 运行 Streamlit 应用：

```bash
streamlit run ai_audio_tour_agent.py
```
