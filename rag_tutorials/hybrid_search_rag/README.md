# 👀 使用混合搜索的 RAG 应用

一个强大的文档问答应用，利用混合搜索（Hybrid Search / RAG）和 Claude 的高级语言能力提供全面回答。该系统使用 RAGLite 实现可靠的文档处理和检索，并使用 Streamlit 构建直观的聊天界面，将文档专属知识与 Claude 的通用智能无缝结合，从而生成准确且具备上下文的响应。

## 功能

- **混合搜索问答**
  - 针对文档相关查询使用 RAG 生成答案
  - 针对通用知识问题回退至 Claude

- **文档处理**：
  - PDF 文档上传和处理
  - 自动文本分块和 Embedding
  - 结合语义匹配和关键词匹配的混合搜索
  - 通过重排序选择更优上下文

- **多模型集成**：
  - Claude 用于文本生成——已使用 Claude 3 Opus 测试
  - OpenAI 用于 Embedding——已使用 text-embedding-3-large 测试
  - Cohere 用于重排序——已使用 Cohere 3.5 Reranker 测试

## 环境要求

你需要准备以下 API Key 和数据库：

1. **数据库**：在 [Neon](https://neon.tech) 创建免费的 PostgreSQL 数据库：
   - 注册或登录 Neon
   - 创建一个新项目
   - 复制连接字符串（格式类似：`postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname`）

2. **API Key**：
   - [OpenAI API Key](https://platform.openai.com/api-keys)，用于 Embedding
   - [Anthropic API Key](https://console.anthropic.com/settings/keys)，用于 Claude
   - [Cohere API Key](https://dashboard.cohere.com/api-keys)，用于重排序

## 如何开始？

1. **克隆仓库**：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd awesome-llm-apps/rag_tutorials/hybrid_search_rag
   ```

2. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

3. **安装 spaCy 模型**：
   ```bash
   pip install https://github.com/explosion/spacy-models/releases/download/xx_sent_ud_sm-3.7.0/xx_sent_ud_sm-3.7.0-py3-none-any.whl
   ```

4. **运行应用**：
   ```bash
   streamlit run main.py
   ```

## 使用方法

1. 启动应用
2. 在侧边栏输入 API Key：
   - OpenAI API Key
   - Anthropic API Key
   - Cohere API Key
   - 数据库 URL（可选，默认使用 SQLite）
3. 点击“Save Configuration”
4. 上传 PDF 文档
5. 开始提问：
   - 文档相关问题将使用 RAG
   - 通用问题将直接使用 Claude

## 数据库选项

应用支持多种数据库后端：

- **PostgreSQL**（推荐）：
  - 在 [Neon](https://neon.tech) 创建免费的 Serverless PostgreSQL 数据库
  - 支持即时创建和缩容至零（scale-to-zero）
  - 连接字符串格式：`postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname`

- **MySQL**：
  ```
  mysql://user:pass@host:port/db
  ```

- **SQLite**（本地开发）：
  ```
  sqlite:///path/to/db.sqlite
  ```

## 贡献

欢迎贡献！可以随时提交 Pull Request。
