# 🤖 AutoRAG：基于 GPT-4o 和向量数据库的自主 RAG

**🎓 免费分步教程**

**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-autonomous-rag-app-using-gpt-4o-and-vector-database)，通过详细的代码讲解、说明和最佳实践，从零开始构建该项目。**

这个 Streamlit 应用实现了一个自主检索增强生成（RAG）系统，使用 OpenAI 的 GPT-4o 模型和 PgVector 数据库。用户可以上传 PDF 文档、将其加入知识库，并让 AI 助手结合知识库内容和 Web 搜索结果回答问题。

### 功能
- 提供与 AI 助手交互的聊天界面
- 支持 PDF 文档上传和处理
- 使用 PostgreSQL 和 PgVector 集成知识库
- 使用 DuckDuckGo 实现 Web 搜索
- 持久化保存助手数据和对话记录

### 如何开始？

1. 克隆 GitHub 仓库
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/autonomous_rag
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 确保 PgVector 数据库正在运行：
应用默认要求 PgVector 运行在 [localhost:5532](http://localhost:5532/)。如果你的环境配置不同，请相应修改代码中的配置。

```bash
docker run -d \
  -e POSTGRES_DB=ai \
  -e POSTGRES_USER=ai \
  -e POSTGRES_PASSWORD=ai \
  -e PGDATA=/var/lib/postgresql/data/pgdata \
  -v pgvolume:/var/lib/postgresql/data \
  -p 5532:5432 \
  --name pgvector \
  phidata/pgvector:16
```

4. 运行 Streamlit 应用
```bash
streamlit run autorag.py
```
