# 🔗 第三方工具

第三方工具可以让你集成 LangChain、CrewAI 等框架中已经成熟的**现有工具生态**。通过复用 AI 社区中经过大量实践验证的工具，可以显著扩展 Agent 的能力。

## 🎯 你将学到什么

- **LangChain 集成**：使用 LangChain 丰富的工具库
- **CrewAI 工具**：复用 CrewAI 的专用 Agent 工具
- **工具适配器**：了解 ADK 如何封装外部工具
- **生态优势**：理解成熟工具库带来的价值
- **最佳实践**：了解何时以及如何使用第三方工具

## 🧠 核心概念：第三方工具

第三方工具本质上是**经过 ADK 适配封装的外部库能力**：
- **LangChain Tools**：网页抓取、文档加载、API 调用等
- **CrewAI Tools**：网页抓取、文件操作以及各种专用功能
- **自定义集成**：任意外部服务或第三方库
- **Wrapper 类**：ADK 提供适配器，实现较平滑的集成

### 主要优势
- ✅ **丰富生态**：可直接使用数百种现成工具
- ✅ **经过实战验证**：许多工具已经被大量开发者使用
- ✅ **社区支持**：拥有活跃社区和完善文档
- ✅ **开发速度快**：无需重复造轮子

## 🔧 可用的第三方集成

### 1. **LangChain Tools**
- **用途**：完整的通用工具生态
- **示例**：网页抓取、文件操作、API 调用
- **优势**：成熟且文档完善

### 2. **CrewAI Tools**
- **用途**：面向 Agent 工作流的专用工具
- **示例**：网页抓取、文件操作、内容处理
- **优势**：针对 Agent 使用方式进行了优化

### 3. **自定义集成**
- **用途**：连接任意外部服务或库
- **示例**：数据库连接器、API 客户端
- **优势**：几乎不受扩展能力限制

## 🚀 教程示例

本节包含两个实际实现：

### 📍 **LangChain Agent**
**位置**：`./langchain_agent/`
- **Web 搜索**：集成 DuckDuckGo，获取实时信息
- **Wikipedia 集成**：访问百科知识和文章
- **研究能力**：组合多个来源完成较完整的研究任务
- **内容分析**：整合信息并给出来源引用

### 📍 **CrewAI Agent**
**位置**：`./crewai_agent/`
- **网站操作**：搜索网页内容并执行抓取
- **文件系统工具**：目录搜索和文件读取
- **内容提取**：高级网页抓取与数据提取
- **文档处理**：分析本地文件并处理内容

## 📁 项目结构

```text
4_3_thirdparty_tools/
├── README.md                    # 本文件：第三方工具指南
├── requirements.txt             # 第三方工具依赖
├── ../env.example               # 共享环境变量模板
├── langchain_agent/             # LangChain 集成
│   ├── __init__.py
│   └── agent.py                 # 使用 LangChain 工具的 Agent
└── crewai_agent/                # CrewAI 集成
    ├── __init__.py
    └── agent.py                 # 使用 CrewAI 工具的 Agent
```

## 🎯 学习目标

完成本节后，你将理解：
- ✅ 如何在 ADK 中集成 LangChain 工具
- ✅ 如何在 ADK Agent 中使用 CrewAI 工具
- ✅ 第三方工具集成的最佳实践
- ✅ 何时选择第三方工具，何时自己实现
- ✅ 如何处理工具兼容性问题

## 🔗 快速开始

1. **配置环境**：
   ```bash
   cd 4_3_thirdparty_tools

   # 复制环境变量模板
   cp ../env.example .env

   # 编辑 .env 并添加 Google AI API Key
   # API Key 获取地址：https://aistudio.google.com/
   ```

2. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

3. **运行 Agent**：
   ```bash
   # 启动 ADK Web 界面
   adk web

   # 在 Web 界面中选择：
   # - langchain_agent：Web 搜索与 Wikipedia 研究
   # - crewai_agent：网页抓取与文件操作
   ```

4. **尝试 Agent**：
   - **LangChain Agent**：`Search for latest AI news`、`Tell me about machine learning`
   - **CrewAI Agent**：`Scrape content from example.com`、`Search for Python files in current directory`

5. **比较不同方案**：观察两个工具生态在功能和使用方式上的差异。

## 💡 实用建议

- **优先选择成熟工具**：尽量使用维护良好的库
- **阅读文档**：了解工具限制和运行要求
- **管理依赖**：谨慎控制外部库版本
- **验证兼容性**：确保工具与 ADK 当前版本兼容
- **关注性能**：部分第三方工具可能比自定义实现更慢

## 🔧 集成模式

### 1. **LangChain 工具 Wrapper**
```python
from google.adk.tools.langchain_tool import LangchainTool
from langchain_community.tools import DuckDuckGoSearchRun

# 将 LangChain 工具封装给 ADK 使用
search_tool = LangchainTool(DuckDuckGoSearchRun())
```

### 2. **CrewAI 工具 Wrapper**
```python
from google.adk.tools.crewai_tool import CrewaiTool
from crewai_tools import ScrapeWebsiteTool, DirectorySearchTool, FileReadTool

# 基础工具：最小配置
scrape_tool = CrewaiTool(
    name="scrape_website",
    description="Scrape and extract content from websites",
    tool=ScrapeWebsiteTool(
        config=dict(
            llm=dict(
                provider="google",  # 使用 Google，而不是默认 OpenAI
                config=dict(model="gemini-3-flash-preview"),
            ),
        )
    )
)

# 搜索工具：语义检索需要 Embedding
search_tool = CrewaiTool(
    name="website_search",
    description="Search for content within websites",
    tool=WebsiteSearchTool(
        config=dict(
            llm=dict(
                provider="google",
                config=dict(model="gemini-3-flash-preview"),
            ),
            embedder=dict(
                provider="google",
                config=dict(
                    model="gemini-embedding-001",
                    task_type="retrieval_document",
                ),
            ),
        )
    )
)
```

### 3. **自定义集成模式**
```python
from google.adk.tools import FunctionTool
import external_library

def custom_integration(query: str) -> dict:
    """Integrate with external library."""
    result = external_library.process(query)
    return {"result": result, "status": "success"}

# 作为函数工具使用
tool = FunctionTool(custom_integration)
```

## 🔧 常见第三方工具

### LangChain Tools
- **DuckDuckGoSearchRun**：Web 搜索
- **WebBaseLoader**：网页抓取
- **WikipediaQueryRun**：Wikipedia 搜索
- **PythonREPLTool**：Python 代码执行
- **ShellTool**：Shell 命令执行

### CrewAI Tools
- **ScrapeWebsiteTool**：网页抓取和内容提取
- **DirectorySearchTool**：文件系统搜索和浏览
- **FileReadTool**：文件读取和内容分析

### 自定义集成
- **数据库连接器**：SQLAlchemy、MongoDB
- **API 客户端**：REST、GraphQL
- **文件处理器**：PDF、Excel、CSV
- **云服务**：AWS、GCP、Azure

## 🚨 重要注意事项

- **依赖**：第三方工具会引入额外外部依赖
- **兼容性**：确保工具版本与 ADK 兼容
- **性能**：某些工具可能比专门的自定义实现更慢
- **维护风险**：外部工具可能发生重大变更或被弃用
- **安全性**：需要检查外部工具的权限需求和安全边界

### 🔧 **CrewAI 模型配置**
⚠️ **重要**：CrewAI 工具默认使用 OpenAI 模型。配合 Google ADK 时，建议显式改为 Google 模型，以保证模型栈一致：

```python
# ❌ 默认配置：使用 OpenAI 模型
tool = WebsiteSearchTool()

# ✅ 正确配置：工具同时配置 LLM 和 Embedding
tool = ScrapeWebsiteTool(
    config=dict(
        llm=dict(
            provider="google",
            config=dict(model="gemini-3-flash-preview"),
        ),
        embedder=dict(
            provider="google",
            config=dict(
                model="gemini-embedding-001",
                task_type="retrieval_document",
            ),
        ),
    )
)

# ✅ 其他工具也遵循同样的配置方式
tool = DirectorySearchTool(
    config=dict(
        llm=dict(
            provider="google",
            config=dict(model="gemini-3-flash-preview"),
        ),
        embedder=dict(
            provider="google",
            config=dict(
                model="gemini-embedding-001",
                task_type="retrieval_document",
            ),
        ),
    )
)
```

**关键点：**
- **LLM 配置**：始终设置 `provider="google"`，避免回退到默认 OpenAI
- **Embedding**：CrewAI 工具需要显式配置 Embedding，避免使用 OpenAI 默认值
- **可用 Provider**：`google`、`openai`、`anthropic`、`ollama`、`llama2` 等

## 🔧 常见应用场景

### Web 与研究
- 网页抓取和内容提取
- 网站内容分析
- 文档处理
- 内容研究和综合分析

### 文件操作
- 文件系统搜索和浏览
- 文件读取和内容分析
- 目录导航
- 本地文件处理

### 开发工具
- 代码执行
- 文档搜索
- 版本控制操作
- 测试辅助工具

### 云与外部服务
- 云存储操作
- 邮件和消息服务
- 身份认证服务
- 监控与日志

## 📊 对比：第三方工具 vs 自定义工具 vs 内置工具

| 维度 | 第三方工具 | 自定义工具 | 内置工具 |
|---|---|---|---|
| **开发时间** | 快 | 慢 | 几乎无需开发 |
| **灵活性** | 中等 | 高 | 低 |
| **性能** | 不固定 | 高 | 通常最高 |
| **维护责任** | 外部社区 | 自己维护 | 基本无需维护 |
| **功能丰富度** | 高 | 按需定制 | 基础 |
| **额外依赖** | 多 | 少 | 无 |
