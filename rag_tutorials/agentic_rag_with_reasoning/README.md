# 🧐 带推理能力的 Agentic RAG

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-agentic-rag-app-with-reasoning)，通过详细的代码讲解、说明和最佳实践，从零开始构建该项目。**

这是一个较完整的 RAG 系统示例，使用 Agno、Gemini 和 OpenAI 展示 AI Agent 的分步骤推理过程。用户可以添加 Web 数据源、提出问题，并实时观察 Agent 的推理过程。

## 功能

1. 交互式知识库管理
- 动态添加 URL 作为 Web 内容来源
- 默认知识源：MCP vs A2A Protocol 文章
- 使用 LanceDB 进行持久化向量数据库存储
- 使用 Session State 跟踪已加载 URL，避免重复导入

2. 透明的推理过程
- 实时显示 Agent 的思考步骤
- 并排展示推理过程和最终答案
- 清晰呈现整个 RAG 流程

3. 高级 RAG 能力
- 使用 OpenAI Embedding 进行向量搜索和语义匹配
- 提供来源归因和引用

## Agent 配置

- 使用 Gemini 2.5 Flash 进行语言处理
- 使用 OpenAI Embedding 模型执行向量搜索
- 使用 ReasoningTools 进行分步骤分析
- 支持自定义 Agent 指令
- 默认知识源：MCP vs A2A Protocol 文章

## 环境要求

你需要以下 API Key：

1. Google API Key

- 在 [aistudio.google.com](https://aistudio.google.com/apikey) 注册
- 进入 API Keys 页面
- 创建新的 API Key

2. OpenAI API Key

- 在 [platform.openai.com](https://platform.openai.com/) 注册
- 进入 API Keys 页面
- 创建新的 API Key

## 运行方法

1. **克隆仓库：**
    ```bash
    git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
    cd rag_tutorials/agentic_rag_with_reasoning
    ```

2. **安装依赖：**
    ```bash
    pip install -r requirements.txt
    ```

3. **运行应用：**
    ```bash
    streamlit run rag_reasoning_agent.py
    ```

4. **配置 API Key：**

- 在第一个输入框中填写 Google API Key
- 在第二个输入框中填写 OpenAI API Key
- 应用正常运行需要同时配置两个 Key

5. **使用应用：**

- 默认知识源：应用预先加载 MCP vs A2A Protocol 文章
- 添加知识源：使用侧边栏向知识库添加 URL
- 推荐提示词：点击提示按钮（What is MCP?、MCP vs A2A、Agent Communication）快速提问
- 提出问题：在主输入框中输入查询
- 查看推理：在左侧面板实时查看 Agent 的推理过程
- 获取答案：在右侧面板查看带来源引用的完整响应

## 工作原理

应用基于 Agno v2.0 构建了一套 RAG Pipeline：

### 知识库设置
- 使用 Agno 的 Knowledge 类从 URL 加载文档
- 文本会自动切分，并使用 OpenAI Embedding 模型生成向量
- 向量存储在 LanceDB 中，以便高效检索
- 通过向量搜索实现相关信息的语义匹配
- 使用 Session State 跟踪 URL，避免重复加载

### Agent 处理流程
- 用户查询会触发 Agent 的推理过程
- ReasoningTools 帮助 Agent 进行分步骤思考
- Agent 从知识库中搜索相关信息
- Gemini 2.5 Flash 生成带引用的完整答案
- 流式事件实时更新推理过程和生成内容

### UI 流程
- 输入 API Key → 加载默认的 MCP vs A2A 文章 → 使用推荐提示词或输入自定义问题
- 左侧面板显示推理过程，右侧面板显示答案生成结果
- 通过来源引用提高透明度和可验证性
- 所有事件实时流式展示，以改善交互体验
