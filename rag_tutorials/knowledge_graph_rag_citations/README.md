# 🔍 带可验证引用的知识图谱 RAG

一个 Streamlit 应用，用于演示**基于知识图谱的检索增强生成（RAG）**如何通过多跳推理提供完整且可验证的来源归因。

## 🎯 有什么不同？

传统的向量 RAG 擅长查找相似文本块，但在以下场景中表现有限：
- 需要综合多个文档信息的问题
- 复杂的推理链
- 为每个结论提供可验证来源

**知识图谱 RAG**通过以下方式解决这些问题：
1. 从文档中**构建由实体和关系组成的结构化图谱**
2. **沿连接关系进行遍历**，查找相关信息并实现多跳推理
3. **跟踪数据来源（provenance）**，使每个结论都能回溯到原始来源

## ✨ 功能

| 功能 | 说明 |
|---------|-------------|
| 🔗 **多跳推理** | 遍历实体关系以回答复杂问题 |
| 📚 **可验证引用** | 每个结论都附带来源文档和原始文本 |
| 🧠 **推理轨迹** | 清楚查看答案是如何推导出来的 |
| 🏠 **完全本地运行** | 使用 Ollama 运行 LLM，使用 Neo4j 存储图谱 |

## 🚀 快速开始

### 环境要求

1. **Ollama** —— 本地 LLM 推理
   ```bash
   # 从 https://ollama.ai 安装
   ollama pull llama3.2
   ```

2. **Neo4j** —— 知识图谱数据库
   ```bash
   # 使用 Docker
   docker run -d \
     --name neo4j \
     -p 7474:7474 -p 7687:7687 \
     -e NEO4J_AUTH=neo4j/password \
     neo4j:latest
   ```

### 安装

```bash
cd knowledge_graph_rag_citations
pip install -r requirements.txt
streamlit run knowledge_graph_rag.py
```

## 📖 工作原理

### 第 1 步：文档 → 知识图谱

```text
文档（文本/PDF） → LLM 提取（实体 + 关系） → 知识图谱（Neo4j）
```

LLM 会提取：
- **实体**：人物、组织、概念、技术等
- **关系**：实体之间如何连接，例如 `works_for`、`created`、`uses`
- **来源信息**：每次提取对应的源文档和文本块

### 第 2 步：查询 → 多跳遍历

```text
查询 → 查找起始实体 → 遍历关系 → 获取上下文 + 来源
```

### 第 3 步：回答 → 已验证引用

```text
上下文 + 来源 → 生成答案 → 带 [1][2] 引用的回答
                              ↓
                         引用详情
                         • 来源文档
                         • 原始文本
                         • 推理路径
```

## 🖥️ 使用示例

### 1. 添加文档

粘贴或选择示例文档。系统会提取实体和关系：

```text
Document: "GraphRAG was developed by Microsoft Research.
           Darren Edge led the project..."

Extracted:
  ├── Entity: GraphRAG (TECHNOLOGY)
  ├── Entity: Microsoft Research (ORGANIZATION)
  ├── Entity: Darren Edge (PERSON)
  └── Relationship: Darren Edge --[WORKS_FOR]--> Microsoft Research
```

### 2. 提出问题

```text
Question: "Who developed GraphRAG and what organization are they from?"
```

### 3. 获取带验证引用的回答

```text
Answer: GraphRAG was developed by researchers at Microsoft Research [1],
        with Darren Edge leading the project [2].

Citations:
  [1] Source: AI Research Paper
      Text: "GraphRAG is a technique developed by Microsoft Research..."

  [2] Source: AI Research Paper
      Text: "...introduced by researchers including Darren Edge..."
```

## 🔧 配置

| 设置 | 默认值 | 说明 |
|---------|---------|-------------|
| Neo4j URI | `bolt://localhost:7687` | Neo4j 连接字符串 |
| Neo4j User | `neo4j` | 数据库用户名 |
| Neo4j Password | - | 数据库密码 |
| LLM Model | `llama3.2` | 用于抽取和生成的 Ollama 模型 |

## 🏗️ 架构

```text
knowledge_graph_rag_citations/
├── knowledge_graph_rag.py   # 主 Streamlit 应用
├── requirements.txt         # Python 依赖
└── README.md                # 本文件
```

### 核心组件

- **`KnowledgeGraphManager`**：用于图操作的 Neo4j 接口
- **`extract_entities_with_llm()`**：基于 LLM 的实体和关系提取
- **`generate_answer_with_citations()`**：带来源追踪的多跳 RAG

## 🎓 延伸阅读

这个示例受到 [VeritasGraph](https://github.com/bibinprathap/VeritasGraph) 启发。VeritasGraph 是一个面向企业场景的框架，支持：
- 本地部署的知识图谱 RAG
- 可视化推理轨迹（Veritas-Scope）
- LoRA 微调 LLM 集成

## 📝 许可证

MIT License
