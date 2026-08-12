## 🛫 AI 旅行 Agent

这是一个基于 Streamlit 的 AI 旅行 Agent，使用 OpenAI GPT-4o 生成个性化旅行行程。它可以自动完成目的地研究、行程规划和整理，让用户更轻松地安排旅行。

### 功能特性
- 调研和发现值得前往的旅行目的地、活动与住宿
- 根据旅行天数定制行程
- 使用 GPT-4o 生成智能、个性化的旅行计划
- 将行程下载为日历（`.ics`）文件，并导入 Google Calendar、Apple Calendar 或其他日历应用

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/starter_ai_agents/ai_travel_agent
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册 [OpenAI](https://platform.openai.com/) 账号（或你选择的其他 LLM Provider）并获取 API Key。

4. 获取 SerpAPI Key

- 注册 [SerpAPI](https://serpapi.com/) 账号并获取 API Key。

5. 运行 Streamlit 应用

```bash
streamlit run travel_agent.py
```

如需使用本地 LLM（Ollama）：

```bash
streamlit run local_travel_agent.py
```

### 工作原理

AI Travel Agent 主要包含两个组件：

- **Researcher（研究 Agent）**：根据用户的目的地和旅行时长生成搜索关键词，并通过 SerpAPI 搜索相关活动和住宿信息。
- **Planner（规划 Agent）**：结合研究结果与用户偏好，生成个性化行程草案，其中包括推荐活动、餐饮和住宿安排。

### 使用日历下载功能

生成旅行行程后：

1. 点击 `Generate Itinerary` 按钮旁的 `Download Itinerary as Calendar (.ics)`。
2. 将 `.ics` 文件保存到电脑。
3. 将文件导入常用日历应用，例如 Google Calendar、Apple Calendar 或 Outlook。
4. 行程中的每一天都会作为全天事件显示在日历中。
5. 每天的完整活动详情会写入对应事件的描述中。

借助该功能，可以更方便地在多个设备上查看旅行计划，即使离线也能访问行程。

### 本地版与云端版

- **`travel_agent.py`**：使用 OpenAI GPT-4o 生成高质量行程，需要 OpenAI API Key。
- **`local_travel_agent.py`**：使用 Ollama 在本地运行 LLM，不需要将数据发送到外部模型 API；需要提前安装并启动 Ollama。
