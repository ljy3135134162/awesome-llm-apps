## ModelsLab 音乐生成器

这是一个基于 Streamlit 的应用，使用 ModelsLab API 和 OpenAI GPT-4 模型生成音乐。用户只需输入描述目标音乐风格的提示词，应用就会根据提示生成 MP3 格式的音乐作品。

## 功能特性

- **生成音乐**：输入详细的音乐生成提示词，例如流派、乐器、情绪等，应用会据此生成音乐。
- **MP3 输出**：生成结果为 MP3 格式，可直接播放或下载。
- **易用界面**：使用简洁直观的 Streamlit UI。
- **API Key 集成**：运行需要 OpenAI 和 ModelsLab API Key，并通过侧边栏输入进行认证。

## 配置

### 环境要求

1. **API Keys**：
   - **OpenAI API Key**：在 [OpenAI](https://platform.openai.com/api-keys) 注册并获取 API Key。
   - **ModelsLab API Key**：在 [ModelsLab](https://modelslab.com/dashboard/api-keys) 注册并获取 API Key。

2. **Python 3.8+**：确保已安装 Python 3.8 或更高版本。

### 安装

1. 克隆仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps
cd starter_ai_agents/ai_music_generator_agent
```

2. 安装所需 Python 依赖：

```bash
pip install -r requirements.txt
```

### 运行应用

1. 启动 Streamlit 应用：

```bash
streamlit run music_generator_agent.py
```

2. 在应用界面中：
   - 输入音乐生成提示词。
   - 点击 `Generate Music`。
   - 播放或下载生成的音乐。
