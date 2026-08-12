# 📠 带数据库路由的 RAG Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-a-rag-agent-with-database-routing)，通过详细的代码讲解、说明和最佳实践，从零开始构建该项目。**

这是一个 Streamlit 应用，用于演示具备智能查询路由能力的高级 RAG Agent。系统结合多个专用数据库和智能回退机制，以确保对用户查询提供可靠且准确的回答。

## 功能

- **文档上传**：用户可以上传与某家公司相关的多个 PDF 文档。这些文档会被处理并存储到三个数据库之一：产品信息、客户支持与 FAQ、财务信息。

- **自然语言查询**：用户可以使用自然语言提问。系统会使用 Agno Agent 作为路由器，自动将查询分发到最相关的数据库。

- **RAG 编排**：使用 LangChain 编排检索增强生成流程，确保检索并展示最相关的信息。

- **回退机制**：如果数据库中没有找到相关文档，则使用带 DuckDuckGo 搜索工具的 LangGraph Agent 执行 Web 调研并生成答案。

## 如何运行？

1. **克隆仓库**：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd rag_tutorials/rag_database_routing
   ```

2. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

3. **运行应用**：
   ```bash
   streamlit run rag_database_routing.py
   ```

4. **获取 OpenAI API Key**：获取 OpenAI API Key 并在应用中配置。该 Key 用于初始化应用中的语言模型。

5. **配置 Qdrant Cloud**：
- 访问 [Qdrant Cloud](https://cloud.qdrant.io/)
- 创建账户或登录
- 创建一个新集群
- 获取凭据：
   - Qdrant API Key：可在 API Keys 部分找到
   - Qdrant URL：你的集群 URL（格式：`https://xxx-xxx.aws.cloud.qdrant.io`）

6. **上传文档**：使用文档上传区域，将 PDF 文档添加到目标数据库。

7. **提出问题**：在查询区域输入问题。应用会将问题路由到适当的数据库并返回答案。

## 使用的技术

- **LangChain**：用于 RAG 编排，实现高效的信息检索和生成。
- **Agno Agent**：作为路由 Agent，判断给定查询最适合使用哪个数据库。
- **LangGraph Agent**：作为回退机制，在必要时使用 DuckDuckGo 执行 Web 调研。
- **Streamlit**：提供易用的文档上传和查询界面。
- **Qdrant**：用于管理数据库，并高效存储和检索文档 Embedding。

## 工作原理

**1. 查询路由**
系统采用三阶段路由策略：
- 在所有数据库中执行向量相似度搜索
- 对存在歧义的查询使用基于 LLM 的路由
- 对未知主题回退至 Web 搜索

**2. 文档处理**
- 自动从 PDF 中提取文本
- 使用重叠机制进行智能文本分块
- 生成向量 Embedding
- 高效存储到数据库

**3. 答案生成**
- 上下文感知检索
- 智能组合相关文档
- 基于置信度生成响应
- 集成 Web 调研结果
