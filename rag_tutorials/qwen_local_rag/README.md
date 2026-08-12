# 🐋 Qwen 3 本地 RAG 推理 Agent

这个 RAG 应用演示了如何通过 Ollama 使用本地运行的 Qwen 3 和 Gemma 3 模型，构建一个强大的检索增强生成（RAG）系统。它结合文档处理、向量搜索和 Web 搜索能力，为用户查询提供准确且具备上下文感知能力的回答。基于 Agno v2.0 构建。

## 功能

- **🧠 多种本地 LLM 选项**：
  - Qwen3（1.7b、8b）——阿里巴巴最新语言模型
  - Gemma3（1b、4b）——Google 的高效语言模型，具备多模态能力
  - DeepSeek（1.5b）——可选替代模型

- **📚 完整的 RAG 系统**：
  - 上传并处理 PDF 文档
  - 从 Web URL 提取内容
  - 智能分块和 Embedding
  - 支持可调阈值的相似度搜索

- **🌐 Web 搜索集成**：
  - 当文档知识不足时回退到 Web 搜索
  - 可配置域名过滤
  - 在响应中提供来源归因

- **🔄 灵活的运行模式**：
  - 在 RAG 和直接 LLM 交互之间切换
  - 可在需要时强制执行 Web 搜索
  - 调整文档检索的相似度阈值

- **💾 向量数据库集成**：
  - 使用 Qdrant 向量数据库执行高效相似度搜索
  - 持久化存储文档 Embedding

- **🔧 Agno v2.0 框架**：
  - 使用 Agno v2.0 Knowledge Embedder 系统
  - 提供 Debug 模式以增强开发体验
  - 使用改进了工具集成能力的现代 Agent 架构

## 如何开始

### 环境要求

- 本地已安装 [Ollama](https://ollama.ai/)
- Python 3.8+
- 本地运行 Qdrant（通过 Docker）用于向量存储
- Exa API Key（可选，用于 Web 搜索）
- 已安装 Agno v2.0

### 安装

1. 克隆 GitHub 仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd rag_tutorials/qwen_local_rag
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 使用 Ollama 拉取所需模型：

```bash
ollama pull qwen3:1.7b # 或你想使用的其他模型
ollama pull snowflake-arctic-embed # 用于 Embedding
```

4. 通过 Docker 在本地运行 Qdrant：

```bash
docker pull qdrant/qdrant

docker run -p 6333:6333 -p 6334:6334 \
    -v "$(pwd)/qdrant_storage:/qdrant/storage:z" \
    qdrant/qdrant
```

5. 获取 API Key（可选）：
   - Exa API Key（用于 Web 搜索回退能力）

6. 运行应用：

```bash
streamlit run qwen_local_rag_agent.py
```

## 工作原理

1. **文档处理**：
   - 使用 PyPDFLoader 处理 PDF 文件
   - 使用 WebBaseLoader 提取 Web 内容
   - 使用 RecursiveCharacterTextSplitter 将文档切分为多个文本块
   - 添加元数据，用于追踪来源类型和时间戳

2. **向量数据库**：
   - 通过 Agno 的 OllamaEmbedder，使用 Ollama Embedding 模型生成文档块向量
   - 将 Embedding 存储在 Qdrant 向量数据库中
   - 基于查询执行相似度搜索，并通过可配置阈值检索相关文档

3. **查询处理**：
   - 分析用户查询以确定最佳信息来源
   - 使用相似度阈值检查文档相关性
   - 如果没有找到相关文档，并且已启用 Web 搜索，则回退到 Web 搜索
   - 支持通过开关强制启用 Web 搜索模式

4. **响应生成**：
   - 本地 LLM（Qwen / Gemma / DeepSeek）根据检索得到的上下文生成回答
   - Agno Agent 使用 Debug 模式，以提高工具调用过程的可见性
   - 向用户显示并引用相关来源
   - 使用 Web 搜索结果时会明确标识
   - 对推理模型显示其推理过程

## 配置选项

- **模型选择**：在不同的 Qwen、Gemma 和 DeepSeek 模型之间选择
- **RAG 模式**：在启用 RAG 和直接 LLM 交互之间切换
- **搜索调优**：调整文档检索的相似度阈值（0.0–1.0）
- **Web 搜索**：启用或禁用 Web 搜索回退，并配置域名过滤
- **Debug 模式**：Agent 默认使用 Debug 模式，以更清晰地查看工具调用和执行流程

## 使用场景

- **文档问答**：针对上传的文档提出问题
- **研究助手**：结合文档知识和 Web 搜索进行研究
- **本地隐私**：处理敏感文档时无需将数据发送到外部 API
- **离线运行**：在网络有限或完全离线的环境下运行高级 AI 能力

## 依赖要求

完整依赖列表请参阅 `requirements.txt`。
