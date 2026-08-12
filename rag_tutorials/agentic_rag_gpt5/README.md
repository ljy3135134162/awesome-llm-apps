# 🧠 使用 GPT-5 构建 Agentic RAG

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-agentic-rag-with-openai-gpt-5)，通过详细的代码讲解、说明和最佳实践，从零开始构建该项目。**

这是一个基于 Agno 框架构建的 Agentic RAG 应用，结合 GPT-5 与 LanceDB，实现高效的知识检索和问答。

## ✨ 功能

- **🤖 GPT-5**：使用 OpenAI 模型生成智能响应
- **🗄️ LanceDB**：轻量级向量数据库，用于快速相似度搜索
- **🔍 Agentic RAG**：智能检索增强生成
- **📝 Markdown 格式化**：生成美观且结构化的响应
- **🌐 动态知识库**：通过添加 URL 扩展知识库
- **⚡ 实时流式输出**：实时查看答案生成过程
- **🎯 简洁界面**：减少复杂配置，提供简化的 UI

## 🚀 快速开始

### 环境要求

- Python 3.11+
- 具备 GPT-5 访问权限的 OpenAI API Key

### 安装

1. **进入项目目录**
   ```bash
   cd rag_tutorials/agentic_rag_gpt5
   ```

2. **安装依赖**
   ```bash
   pip install -r requirements.txt
   ```

3. **配置 OpenAI API Key**
   ```bash
   export OPENAI_API_KEY="your-api-key-here"
   ```
   或创建 `.env` 文件：
   ```
   OPENAI_API_KEY=your-api-key-here
   ```

4. **运行应用**
   ```bash
   streamlit run agentic_rag_gpt5.py
   ```

## 🎯 使用方法

1. 在侧边栏中**输入 OpenAI API Key**
2. 在侧边栏输入 URL，**添加知识来源**
3. 使用文本区域或推荐提示词**提出问题**
4. 实时查看以 Markdown 格式流式生成的答案

### 推荐问题

- **“What is Agno?”** ——了解 Agno 框架和 Agent
- **“Teams in Agno”** ——了解 Agno 中 Team 的工作方式
- **“Build RAG system”** ——获取构建 RAG 系统的分步指南

## 🏗️ 架构

### 核心组件

- **`Agent`**：编排整个问答流程
- **`UrlKnowledge`**：负责从 URL 加载文档
- **`LanceDb`**：用于高效相似度搜索的向量数据库
- **`OpenAIEmbedder`**：将文本转换为嵌入向量
- **`OpenAIChat`**：使用 GPT-5-nano 模型生成响应

### 数据流

1. **知识加载**：处理 URL 内容并存储到 LanceDB
2. **向量搜索**：通过 OpenAI Embedding 实现语义搜索
3. **响应生成**：GPT-5-nano 处理检索到的信息并生成答案
4. **流式输出**：实时显示格式化后的响应

## 🔧 配置

### 数据库设置

- **向量数据库**：使用本地存储的 LanceDB
- **表名**：`agentic_rag_docs`
- **搜索类型**：向量相似度搜索

## 📚 知识管理

### 添加数据源

- 使用侧边栏添加新的 URL
- 数据源会自动处理并建立索引
- 当前数据源以编号列表形式显示

### 默认知识源

- 默认使用 Agno 文档：`https://docs.agno.com/introduction/agents.md`
- 可以使用任意基于 Web 的文档继续扩展

## 🎨 UI 功能

### 侧边栏

- **API Key 管理**：安全输入 OpenAI 凭据
- **添加 URL**：动态扩展知识库
- **当前数据源**：以编号列表显示已加载的 URL

### 主界面

- **推荐提示词**：快速访问常见问题
- **查询输入**：用于自定义问题的大型文本区域
- **实时流式输出**：实时生成答案
- **Markdown 渲染**：显示格式化后的响应
