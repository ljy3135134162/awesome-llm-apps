## 📚 带记忆功能的 AI 研究 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-ai-research-agent-with-memory-to-search-academic-papers)，通过详细代码讲解、说明和最佳实践，从零开始构建这个应用。**

这个 Streamlit 应用实现了一个由 AI 驱动的研究助手，可以帮助用户搜索 arXiv 上的学术论文，同时持续记住用户的研究兴趣和历史交互。它使用 OpenAI 的 GPT-4o-mini 模型处理搜索结果，使用 MultiOn 进行 Web 浏览，并结合 Mem0 与 Qdrant 保存用户上下文。

### 功能

- 提供 arXiv 论文搜索界面
- 使用 AI 处理搜索结果，提高可读性
- 持久保存用户兴趣和历史搜索记录
- 使用 OpenAI GPT-4o-mini 模型进行智能处理
- 通过 Mem0 和 Qdrant 实现记忆存储与检索

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/llm_apps_with_memory_tutorials/ai_arxiv_agent_memory
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 确保 Qdrant 正在运行

应用默认要求 Qdrant 运行在 `localhost:6333`。如果你的环境不同，请在代码中调整对应配置。

```bash
docker pull qdrant/qdrant

docker run -p 6333:6333 -p 6334:6334 \
    -v $(pwd)/qdrant_storage:/qdrant/storage:z \
    qdrant/qdrant
```

4. 运行 Streamlit 应用

```bash
streamlit run ai_arxiv_agent_memory.py
```
