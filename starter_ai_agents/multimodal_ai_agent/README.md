## 🧬 多模态 AI Agent

这是一个基于 Streamlit 的应用，使用 Google Gemini 2.5 模型结合视频分析与 Web 搜索能力。该 Agent 可以分析用户上传的视频，并将视觉理解与 Web 搜索结果结合起来回答问题。

### 功能特性

- 使用 Gemini 2.5 Flash / Pro 进行视频分析
- 通过 DuckDuckGo 集成 Web 研究能力
- 支持多种视频格式（MP4、MOV、AVI）
- 实时处理视频内容
- 将视觉信息与文本信息结合分析

### 如何开始？

1. 克隆 GitHub 仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd starter_ai_agents/multimodal_ai_agent
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 Google Gemini API Key：

- 注册 [Google AI Studio](https://aistudio.google.com/apikey) 并获取 API Key。

4. 将 Gemini API Key 配置为环境变量：

```bash
GOOGLE_API_KEY=your_api_key_here
```

5. 运行 Streamlit 应用：

```bash
streamlit run multimodal_agent.py
```
