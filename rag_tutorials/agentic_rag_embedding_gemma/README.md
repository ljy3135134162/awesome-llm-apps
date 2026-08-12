## 🔥 基于 EmbeddingGemma 的 Agentic RAG

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-local-agentic-rag-app-with-google-embeddinggemma)，通过详细的代码讲解、说明和最佳实践，从零开始构建这个应用。**

这个 Streamlit 应用演示了一个 Agentic 检索增强生成（RAG）Agent：使用 Google 的 EmbeddingGemma 生成嵌入，使用 Llama 3.2 作为语言模型，并通过 Ollama 完全在本地运行。

### 功能

- **本地 AI 模型**：使用 EmbeddingGemma 生成向量嵌入，使用 Llama 3.2 生成文本
- **PDF 知识库**：动态添加 PDF URL 来构建知识库
- **向量搜索**：使用 LanceDB 高效执行相似度搜索
- **交互式 UI**：提供 Streamlit 界面，用于添加数据源和进行查询
- **流式响应**：实时生成回答，并显示工具调用过程

### 如何开始？

1. 克隆 GitHub 仓库：
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/agentic_rag_embedding_gemma
```

2. 安装所需依赖：
```bash
pip install -r requirements.txt
```

3. 确保已经安装并运行 Ollama，同时准备好所需模型：
   - 拉取模型：`ollama pull embeddinggemma:latest` 和 `ollama pull llama3.2:latest`
   - 如果 Ollama 服务尚未运行，请启动它

4. 运行 Streamlit 应用：
```bash
streamlit run agentic_rag_embeddinggemma.py
```
   （注意：应用文件位于项目根目录。）

5. 在浏览器中打开命令行提供的地址（通常为 `http://localhost:8501`），即可与 RAG Agent 交互。

### 工作原理

1. **知识库设置**：在侧边栏添加 PDF URL，加载文档并建立索引。
2. **生成嵌入**：EmbeddingGemma 为语义搜索生成向量嵌入。
3. **查询处理**：将用户查询转换为嵌入，并在知识库中执行搜索。
4. **生成回答**：Llama 3.2 根据检索到的上下文生成答案。
5. **工具集成**：Agent 使用搜索工具获取相关信息。

### 环境要求

- Python 3.8+
- 已安装并运行 Ollama
- 所需模型：`embeddinggemma:latest`、`llama3.2:latest`

### 使用的技术

- **Agno**：用于构建 AI Agent 的框架
- **Streamlit**：Web 应用框架
- **LanceDB**：向量数据库
- **Ollama**：本地 LLM 服务
- **EmbeddingGemma**：Google 的嵌入模型
- **Llama 3.2**：Meta 的语言模型
