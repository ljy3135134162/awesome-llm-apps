# 🐋 Deepseek 本地 RAG 推理 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-a-local-rag-reasoning-agent-with-deepseek-r1)，通过详细的代码讲解、说明和最佳实践，从零开始构建该项目。**

这是一个功能强大的推理 Agent，将本地 Deepseek 模型与 RAG 能力结合起来。项目使用 Deepseek（通过 Ollama）、Snowflake Embedding、Qdrant 向量存储以及 Agno Agent 编排框架构建，同时提供简单的本地聊天模式和高级 RAG 增强交互，并具备完整的文档处理和 Web 搜索能力。

## 功能

- **双运行模式**
  - 本地聊天模式：直接与本地 Deepseek 模型交互
  - RAG 模式：结合文档上下文和 Web 搜索增强推理能力，并使用 llama3.2

- **文档处理**（RAG 模式）
  - 上传和处理 PDF 文档
  - 提取网页内容
  - 自动进行文本分块和 Embedding
  - 将向量存储到 Qdrant Cloud

- **智能查询**（RAG 模式）
  - 基于 RAG 的文档检索
  - 带阈值过滤的相似度搜索
  - 自动回退到 Web 搜索
  - 为答案提供来源归属

- **高级能力**
  - 集成 Exa AI Web 搜索
  - 支持 Web 搜索的自定义域名过滤
  - 上下文感知的响应生成
  - 聊天历史管理
  - 推理过程可视化

- **模型相关功能**
  - 灵活选择模型：
    - Deepseek R1 1.5B（更轻量，适合大多数笔记本电脑）
    - Deepseek R1 7B（能力更强，需要更好的硬件）
  - 使用 Snowflake Arctic Embedding 模型生成向量嵌入
  - 使用 Agno Agent 框架进行编排
  - 基于 Streamlit 的交互式界面

## 环境要求

### 1. 配置 Ollama
1. 安装 [Ollama](https://ollama.ai)
2. 拉取 Deepseek R1 模型：
```bash
# 较轻量的模型
ollama pull deepseek-r1:1.5b

# 能力更强的模型（如果硬件支持）
ollama pull deepseek-r1:7b

ollama pull snowflake-arctic-embed
ollama pull llama3.2
```

### 2. 配置 Qdrant Cloud（RAG 模式）
1. 访问 [Qdrant Cloud](https://cloud.qdrant.io/)
2. 创建账户或登录
3. 创建一个新集群
4. 获取凭据：
   - Qdrant API Key：可在 API Keys 部分找到
   - Qdrant URL：你的集群 URL（格式：`https://xxx-xxx.cloud.qdrant.io`）

### 3. Exa AI API Key（可选）
1. 访问 [Exa AI](https://exa.ai)
2. 注册账户
3. 生成用于 Web 搜索的 API Key

## 如何运行

1. 克隆仓库：
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd rag_tutorials/deepseek_local_rag_agent
```

2. 安装依赖：
```bash
pip install -r requirements.txt
```

3. 运行应用：
```bash
streamlit run deepseek_rag_agent.py
```
