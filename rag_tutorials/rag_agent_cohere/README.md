# 使用 Cohere ⌘R 的 RAG Agent

这是一个使用 Cohere 新模型 Command-r7b-12-2024 构建的 Agentic RAG 系统，使用 Qdrant 进行向量存储、LangChain 实现 RAG，并使用 LangGraph 进行编排。该应用允许用户上传文档、针对文档提出问题，并在必要时回退到 Web 搜索，以生成 AI 驱动的回答。

## 功能

- **文档处理**
  - PDF 文档上传和处理
  - 自动文本分块和 Embedding
  - 将向量存储到 Qdrant Cloud

- **智能查询**
  - 基于 RAG 的文档检索
  - 带阈值过滤的相似度搜索
  - 当未找到相关文档时自动回退到 Web 搜索
  - 为回答提供来源归因

- **高级能力**
  - 集成 DuckDuckGo Web 搜索
  - 使用 LangGraph Agent 进行 Web 调研
  - 上下文感知的响应生成
  - 长回答摘要

- **模型相关功能**
  - 使用 Command-r7b-12-2024 模型进行 Chat 和 RAG
  - 使用 Cohere `embed-english-v3.0` 模型生成 Embedding
  - 使用 LangGraph 的 `create_react_agent` 函数
  - 使用 `DuckDuckGoSearchRun` 工具进行 Web 搜索

## 环境要求

### 1. Cohere API Key
1. 前往 [Cohere Platform](https://dashboard.cohere.ai/api-keys)
2. 注册或登录账户
3. 进入 API Keys 页面
4. 创建新的 API Key

### 2. Qdrant Cloud 配置
1. 访问 [Qdrant Cloud](https://cloud.qdrant.io/)
2. 创建账户或登录
3. 创建一个新集群
4. 获取凭据：
   - Qdrant API Key：可在 API Keys 部分找到
   - Qdrant URL：你的集群 URL（格式：`https://xxx-xxx.aws.cloud.qdrant.io`）

## 如何运行

1. 克隆仓库：
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd rag_tutorials/rag_agent_cohere
```

2. 安装依赖：
```bash
pip install -r requirements.txt
```

3. 运行应用：
```bash
streamlit run rag_agent_cohere.py
```
