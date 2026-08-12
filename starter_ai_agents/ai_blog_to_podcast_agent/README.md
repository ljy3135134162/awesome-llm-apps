## 📰 ➡️ 🎙️ 博客转播客 Agent

这是一个基于 Streamlit 的应用，可将任意博客文章转换成播客。应用使用 OpenAI GPT-4 对内容进行总结，使用 Firecrawl 抓取博客正文，并通过 ElevenLabs API 生成音频。用户只需输入博客 URL，应用即可根据文章内容生成一期播客。

## 功能特性

- **博客抓取**：使用 Firecrawl API 抓取任意公开博客 URL 的完整内容。
- **摘要生成**：使用 OpenAI GPT-4 将博客整理成有吸引力且简洁的摘要（2000 字符以内）。
- **播客生成**：使用 ElevenLabs 语音 API 将摘要转换成音频播客。
- **API Key 集成**：运行需要 OpenAI、Firecrawl 和 ElevenLabs API Key，可通过侧边栏安全输入。

## 配置

### 环境要求

1. **API Keys**：
   - **OpenAI API Key**：注册 OpenAI 并获取 API Key。
   - **ElevenLabs API Key**：从 ElevenLabs 获取 API Key。
   - **Firecrawl API Key**：从 Firecrawl 获取 API Key。

2. **Python 3.8+**：确保已安装 Python 3.8 或更高版本。

### 安装

1. 克隆仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps
cd starter_ai_agents/ai_blog_to_podcast_agent
```

2. 安装所需 Python 依赖：

```bash
pip install -r requirements.txt
```

### 运行应用

1. 启动 Streamlit：

```bash
streamlit run blog_to_podcast_agent.py
```

2. 在应用界面中：
   - 在侧边栏输入 OpenAI、ElevenLabs 和 Firecrawl API Key。
   - 输入需要转换的博客 URL。
   - 点击 `🎙️ Generate Podcast`。
   - 收听或下载生成的播客。
