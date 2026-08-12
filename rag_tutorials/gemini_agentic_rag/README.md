# 🤔 使用 Gemini Flash Thinking 构建 Agentic RAG

这是一个基于全新 Gemini 2.0 Flash Thinking 模型和 gemini-exp-1206 构建的 Agentic RAG 系统，使用 Qdrant 进行向量存储，并使用 Agno（原 phidata）进行 Agent 编排。该应用具备智能查询重写、文档处理以及 Web 搜索回退能力，可提供全面的 AI 驱动回答。

## 功能

- **文档处理**
  - PDF 文档上传和处理
  - Web 页面内容提取
  - 自动文本分块和 Embedding
  - 将向量存储到 Qdrant Cloud

- **智能查询**
  - 重写查询以改善检索效果
  - 基于 RAG 的文档检索
  - 带阈值过滤的相似度搜索
  - 自动回退至 Web 搜索
  - 为回答提供来源归因

- **高级能力**
  - 集成 Exa AI Web 搜索
  - 支持为 Web 搜索自定义域名过滤
  - 上下文感知的响应生成
  - 聊天历史管理
  - 查询重构 Agent

- **模型相关功能**
  - 使用 Gemini Thinking 2.0 Flash 进行聊天和推理
  - 使用 Gemini Embedding 模型生成向量嵌入
  - 使用 Agno Agent 框架进行编排
  - 基于 Streamlit 的交互式界面

## 环境要求

### 1. Google API Key
1. 前往 [Google AI Studio](https://aistudio.google.com/apikey)
2. 注册或登录账户
3. 创建新的 API Key

### 2. Qdrant Cloud 配置
1. 访问 [Qdrant Cloud](https://cloud.qdrant.io/)
2. 创建账户或登录
3. 创建一个新集群
4. 获取凭据：
   - Qdrant API Key：可在 API Keys 部分找到
   - Qdrant URL：你的集群 URL（格式：`https://xxx-xxx.cloud.qdrant.io`）

### 3. Exa AI API Key（可选）
1. 访问 [Exa AI](https://exa.ai)
2. 注册账户
3. 生成用于 Web 搜索功能的 API Key

## 如何运行

1. 克隆仓库：
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd rag_tutorials/gemini_agentic_rag
```

2. 安装依赖：
```bash
pip install -r requirements.txt
```

3. 运行应用：
```bash
streamlit run agentic_rag_gemini.py
```

## 使用方法

1. 在侧边栏配置 API Key：
   - 输入 Google API Key
   - 添加 Qdrant 凭据
   - （可选）添加 Exa AI Key 以启用 Web 搜索

2. 上传文档：
   - 使用文件上传器上传 PDF
   - 输入 URL 以添加 Web 内容

3. 提出问题：
   - 在聊天界面输入查询
   - 查看重写后的查询和来源
   - 在相关情况下查看 Web 搜索结果

4. 管理会话：
   - 根据需要清除聊天历史
   - 配置 Web 搜索域名
   - 查看已处理的文档
