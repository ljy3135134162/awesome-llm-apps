# 🧠 Headroom - 上下文优化层

通过智能上下文压缩，将 LLM API 成本降低 50-90%。工具输出中通常有 70-95% 属于重复样板内容——Headroom 会压缩这些冗余，同时保留准确性。

## 📋 概览

这个示例展示了如何使用 [Headroom](https://github.com/chopratejas/headroom)，在 AI Agent 和大量调用工具的 LLM 应用中显著减少 Token 使用量。与简单截断不同，Headroom 会通过统计分析保留真正重要的信息，并压缩不重要的内容。

### 核心优势

- **💰 Token 减少 47-92%** —— 已在真实工作负载中验证
- **🎯 无需修改代码** —— 可作为透明代理直接使用
- **🔄 可逆压缩** —— LLM 可通过 CCR 取回原始数据
- **🧠 内容感知** —— 可针对代码、日志和 JSON 采用合适的压缩方式
- **⚡ 提供商缓存优化** —— 自动优化前缀以提高缓存命中率
- **🔌 原生框架支持** —— LangChain、Agno、MCP 以及任何 OpenAI 客户端

## 🚀 功能

- **SmartCrusher**：对 JSON 工具输出进行统计压缩——保留开头项目、结尾项目、异常项以及与查询相关的匹配项
- **CacheAligner**：稳定上下文前缀，以提高 OpenAI、Anthropic、Google 等提供商侧的缓存效果
- **RollingWindow**：在不破坏工具调用/响应配对关系的前提下管理上下文窗口限制
- **代码感知压缩**：使用 tree-sitter 基于 AST 压缩代码
- **LLMLingua-2 集成**：可选的基于机器学习的最高 20 倍压缩
- **记忆系统**：持久化、由 LLM 驱动的记忆，并支持零延迟内联提取
- **CCR（Compress-Cache-Retrieve）**：可逆压缩——LLM 在需要时可以请求原始数据

## 📦 安装

### 基础安装

```bash
pip install headroom-ai
```

### 安装框架集成

```bash
pip install "headroom-ai[proxy]"      # 代理服务器（无需修改代码）
pip install "headroom-ai[langchain]"  # LangChain 集成
pip install "headroom-ai[agno]"       # Agno Agent 框架
pip install "headroom-ai[code]"       # 基于 AST 的代码压缩
pip install "headroom-ai[llmlingua]"  # 基于机器学习的压缩
pip install "headroom-ai[all]"        # 安装全部功能
```

## 💻 使用方法

### 方案 1：代理模式（无需修改代码）

```bash
headroom proxy --port 8787
```

将现有工具指向该代理：

```bash
# Claude Code
ANTHROPIC_BASE_URL=http://localhost:8787 claude

# Cursor 或任何兼容 OpenAI 的客户端
OPENAI_BASE_URL=http://localhost:8787/v1 cursor
```

### 方案 2：LangChain 集成

```python
from langchain_openai import ChatOpenAI
from headroom.integrations import HeadroomChatModel

# 包装模型即可
llm = HeadroomChatModel(ChatOpenAI(model="gpt-4o"))
response = llm.invoke("Analyze these logs and find the error")
```

### 方案 3：Agno Agent 框架

```python
from agno.agent import Agent
from agno.models.openai import OpenAIChat
from headroom.integrations.agno import HeadroomAgnoModel

# 包装模型
model = HeadroomAgnoModel(OpenAIChat(id="gpt-4o"))
agent = Agent(model=model, tools=[search_github, search_code, query_db])

response = agent.run("Investigate the memory leak")
print(f"Tokens saved: {model.total_tokens_saved}")
```

## 📊 真实场景性能

以下数据来自实际 API 调用，而不是估算：

| 场景 | 优化前 | 优化后 | 节省 |
|----------|--------|-------|---------|
| 代码搜索（100 条结果） | 17,765 tokens | 1,408 tokens | **92%** |
| SRE 故障排查 | 65,694 tokens | 5,118 tokens | **92%** |
| 代码库探索 | 78,502 tokens | 41,254 tokens | **47%** |
| GitHub Issue 分类 | 54,174 tokens | 14,761 tokens | **73%** |
| 多工具 Agent | 15,662 tokens | 6,100 tokens | **76%** |

## 🔬 验证：大海捞针测试

**测试设置：** 100 条生产日志，其中只有一条关键 FATAL 错误隐藏在第 67 条。

**使用 Headroom 前：** 10,144 tokens  
**使用 Headroom 后：** 1,260 tokens（**减少 87.6%**）

**问题：** “是什么导致了宕机？错误码是什么？应该如何修复？”

**两个回答（基线与 Headroom）完全一致：** *“payment-gateway service，错误 PG-5523，修复方式：将 max_connections 提高到 500，共影响 1,847 笔交易”*

**答案相同，但 Token 减少了 87.6%。**

```bash
# 自己运行测试
python headroom_demo.py
```

## 🎯 最适合的场景

**适合使用 Headroom 的情况：**
- ✅ 构建包含多个工具的 AI Agent（搜索、数据库、API 等）
- ✅ 处理大型工具输出（日志、代码搜索结果、API 响应）
- ✅ 上下文窗口被大量重复数据占满
- ✅ 希望大规模降低 LLM API 成本

**Headroom 特别适合：**
- 🔍 代码搜索结果
- 📋 日志分析
- 🗄️ 数据库查询结果
- 🔗 API 响应处理
- 🤖 多工具 Agent 工作流

## 🛡️ 安全保证

- **不会删除人工内容** —— 用户和助手消息都会保留
- **不会破坏工具调用顺序** —— 工具调用与响应始终保持配对
- **解析失败时不做修改** —— 格式异常的内容会原样通过
- **压缩可逆** —— LLM 可以通过 CCR 取回原始内容

## 🔗 资源

- **GitHub**: https://github.com/chopratejas/headroom
- **PyPI**: https://pypi.org/project/headroom-ai/
- **文档**: https://github.com/chopratejas/headroom/tree/main/docs
- **演示视频**: https://github.com/chopratejas/headroom/releases

## 🔌 模型提供商支持

| 提供商 | Token 计数 | 缓存优化 |
|----------|----------------|-------------------|
| OpenAI | tiktoken（精确） | 自动前缀缓存 |
| Anthropic | 官方 API | cache_control 块 |
| Google | 官方 API | 上下文缓存 |
| Cohere | 官方 API | - |
| Mistral | 官方 tokenizer | - |

## 🤝 贡献

欢迎贡献，你可以：
- 报告 Bug
- 提议新的压缩策略
- 添加基准测试
- 改进文档

## 📄 许可证

Apache License 2.0，详见 [LICENSE](https://github.com/chopratejas/headroom/blob/main/LICENSE)。

## 🙏 致谢

由 [Tejas Chopra](https://github.com/chopratejas) 为 AI 开发者社区构建。
