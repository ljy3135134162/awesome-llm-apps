# 🎙️ 客户支持语音 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-customer-support-voice-agent)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个基于 OpenAI SDK 构建的客户支持 Agent 应用，可针对知识库中的问题提供语音回答。系统使用 OpenAI GPT-4o 与 TTS 能力，通过 Firecrawl 抓取文档网站，再将内容处理后存入 Qdrant，构建可搜索的知识库，并同时为用户查询生成文本与语音响应。

## 功能特性

- **知识库构建**
  - 使用 Firecrawl 抓取文档网站
  - 使用 Qdrant 向量数据库存储并索引内容
  - 使用 FastEmbed 生成 Embedding，以支持语义搜索

- **AI Agent 团队**
  - **Documentation Processor（文档处理 Agent）**：分析文档内容，并针对用户查询生成清晰、简洁的回答
  - **TTS Agent（语音合成 Agent）**：将文本回答转换为自然语音，并自动调整语速与强调方式
  - **语音自定义**：支持多个 OpenAI TTS 声线：
    - alloy、ash、ballad、coral、echo、fable、onyx、nova、sage、shimmer、verse

- **交互式界面**
  - 简洁的 Streamlit UI，并提供侧边栏配置
  - 实时文档搜索与回答生成
  - 内置音频播放器，并支持下载音频
  - 提供系统初始化和查询处理的进度提示

## 如何运行

1. **配置环境**

   ```bash
   # 克隆仓库
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd awesome-llm-apps/voice_ai_agents/customer_support_voice_agent

   # 安装依赖
   pip install -r requirements.txt
   ```

2. **配置 API Key**
   - 从 [OpenAI Platform](https://platform.openai.com) 获取 OpenAI API Key
   - 从 [Qdrant Cloud](https://cloud.qdrant.io) 获取 Qdrant API Key 和 URL
   - 获取 Firecrawl API Key，用于抓取文档网站

3. **运行应用**

   ```bash
   streamlit run customer_support_voice_agent.py
   ```

4. **使用界面**
   - 在侧边栏输入 API 凭据
   - 输入需要构建知识库的文档网站 URL
   - 从下拉菜单中选择喜欢的语音
   - 点击 `Initialize System` 处理文档内容
   - 提出问题，并同时获得文本和语音回答

## 功能详解

- **知识库构建**
  - 根据文档网站构建可搜索知识库
  - 保留文档结构与元数据
  - 支持抓取多个页面，默认配置最多处理 5 个页面

- **向量搜索**
  - 使用 FastEmbed 生成 Embedding
  - 通过语义搜索查找相关内容
  - 使用 Qdrant 高效检索文档

- **语音生成**
  - 使用 OpenAI TTS 模型生成高质量语音
  - 提供多种声线供用户选择
  - 生成具备自然停顿、语速和重点强调的语音效果
