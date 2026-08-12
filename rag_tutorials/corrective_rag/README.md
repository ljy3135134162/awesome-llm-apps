# 🔄 Corrective RAG Agent

一个高级的检索增强生成（RAG）系统，使用 LangGraph 实现纠错式多阶段工作流。该系统结合文档检索、相关性评分、查询转换和 Web 搜索，以提供全面且准确的回答。

## 功能

- **智能文档检索**：使用 Qdrant 向量存储高效检索文档
- **文档相关性评分**：使用 Claude 4.5 Sonnet 评估文档相关性
- **查询转换**：在需要时优化查询，以改善搜索结果
- **Web 搜索回退**：当本地文档不足时，使用 Tavily API 进行 Web 搜索
- **多模型方案**：针对不同任务组合使用 OpenAI Embedding 和 Claude 4.5 Sonnet
- **交互式 UI**：基于 Streamlit 构建，便于上传文档和执行查询

## 如何运行？

1. **克隆仓库**：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd rag_tutorials/corrective_rag
   ```

2. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

3. **配置 API Key**：
   你需要获取以下 API Key：
   - [OpenAI API Key](https://platform.openai.com/api-keys)（用于 Embedding）
   - [Anthropic API Key](https://console.anthropic.com/settings/keys)（用于 Claude 4.5 Sonnet LLM）
   - [Tavily API Key](https://app.tavily.com/home)（用于 Web 搜索）
   - Qdrant Cloud 配置
      1. 访问 [Qdrant Cloud](https://cloud.qdrant.io/)
      2. 创建账户或登录
      3. 创建一个新集群
      4. 获取凭据：
         - Qdrant API Key：可在 API Keys 部分找到
         - Qdrant URL：你的集群 URL（格式：`https://xxx-xxx.aws.cloud.qdrant.io`）

4. **运行应用**：
   ```bash
   streamlit run corrective_rag.py
   ```

5. **使用应用**：
   - 上传文档或提供 URL
   - 在查询框中输入问题
   - 查看 Corrective RAG 的逐步处理过程
   - 获取完整回答

## 技术栈

- **LangChain**：用于 RAG 编排和 Chain
- **LangGraph**：用于工作流管理
- **Qdrant**：用于文档存储的向量数据库
- **Claude 4.5 Sonnet**：用于分析和生成的主要语言模型
- **OpenAI**：用于文档 Embedding
- **Tavily**：提供 Web 搜索能力
- **Streamlit**：用于用户界面
