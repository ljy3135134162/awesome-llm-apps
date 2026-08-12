## 🎙️ 使用 OpenAI SDK 构建 Voice RAG

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-voice-rag-agent)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

该示例展示如何使用 OpenAI SDK 和 Streamlit 构建支持语音的检索增强生成（RAG）系统。用户可以上传 PDF 文档、提出问题，并通过 OpenAI 的文本转语音能力同时获得文本和语音回答。

### 功能特性

- 使用 OpenAI SDK 构建支持语音的 RAG 系统
- 支持 PDF 文档处理与文本分块
- 使用 Qdrant 作为向量数据库，实现高效相似度搜索
- 支持多种声音选项的实时文本转语音
- 提供易于使用的 Streamlit 界面
- 支持下载生成的语音回答
- 支持上传多个文档并跟踪其处理状态

### 如何开始？

1. 克隆 GitHub 仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/voice_ai_agents/voice_rag_openaisdk
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 配置 API Key：
- 获取 [OpenAI API Key](https://platform.openai.com/)
- 注册 [Qdrant Cloud](https://cloud.qdrant.io/) 并获取 API Key 和 URL
- 创建 `.env` 文件并填写凭据：

```bash
OPENAI_API_KEY='your-openai-api-key'
QDRANT_URL='your-qdrant-url'
QDRANT_API_KEY='your-qdrant-api-key'
```

4. 运行 Voice RAG 应用：

```bash
streamlit run rag_voice.py
```

5. 打开浏览器并访问控制台输出中提供的 URL，即可与 Voice RAG 系统交互。

### 工作原理

1. **文档处理：**
   - 通过 Streamlit 界面上传 PDF 文档
   - 使用 LangChain 的 `RecursiveCharacterTextSplitter` 将文档切分为多个文本块
   - 使用 FastEmbed 为每个文本块生成 Embedding，并存储到 Qdrant

2. **查询处理：**
   - 将用户问题转换为 Embedding
   - 从 Qdrant 检索相似文档
   - 由处理 Agent 生成清晰、适合口语表达的回答
   - TTS Agent 进一步优化回答，使其更适合语音合成

3. **语音生成：**
   - 使用 OpenAI TTS 将文本回答转换为语音
   - 用户可以从多种声音中进行选择
   - 音频可以直接播放，也可以下载为 MP3

4. **其他功能：**
   - 实时音频流
   - 多种声音风格选项
   - 文档来源跟踪
   - 语音回答下载
   - 文档处理进度跟踪
