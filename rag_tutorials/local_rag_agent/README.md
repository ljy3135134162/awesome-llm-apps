## 🦙 使用 Llama 3.2 的本地 RAG Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-a-local-rag-agent)，通过详细的代码讲解、说明和最佳实践，从零开始构建该项目。**

该应用使用 Ollama 运行 Llama 3.2，并以 Qdrant 作为向量数据库，实现了一套检索增强生成（RAG）系统。项目基于 Agno v2.0 构建。

### 功能
- 完全本地运行的 RAG 实现
- 通过 Ollama 使用 Llama 3.2
- 使用 Qdrant 进行向量搜索
- 交互式 AgentOS 界面
- 不依赖外部 API
- 使用 Agno v2.0 的 Knowledge 类进行文档管理

### 如何开始？

1. 克隆 GitHub 仓库
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
```

2. 安装所需依赖：

```bash
cd awesome-llm-apps/rag_tutorials/local_rag_agent
pip install -r requirements.txt
```

3. 在本地安装并启动 [Qdrant](https://qdrant.tech/) 向量数据库

```bash
docker pull qdrant/qdrant
docker run -p 6333:6333 qdrant/qdrant
```

4. 安装 [Ollama](https://ollama.com/download)，并拉取用于 LLM 的 Llama 3.2，以及作为 OllamaEmbedder 嵌入模型的 OpenHermes
```bash
ollama pull llama3.2
ollama pull openhermes
```

5. 运行 AI RAG Agent
```bash
python local_rag_agent.py
```

6. 打开浏览器，访问控制台输出的地址（通常为 `http://localhost:7777`），即可通过 AgentOS 界面与 RAG Agent 交互。

### 注意
- 知识库会在首次运行时加载一份 Thai Recipes PDF。首次运行完成后，可以注释掉 `knowledge_base.add_content()` 这一行，以避免重复加载。
- AgentOS 提供基于 Web 的交互界面，用于与 Agent 进行对话。
