# 🔥 Firecrawl Agent：通过 MCP 实现高级网页抓取

欢迎使用 **Firecrawl MCP Agent**。本示例演示如何通过模型上下文协议（MCP），将 Firecrawl 的高级网页抓取能力集成到 Google ADK 中。

## 🌟 你将学到什么

- **Firecrawl 集成**：连接 Firecrawl 的完整网页抓取平台
- **高级网页抓取**：支持单页抓取、批量处理和整站爬取
- **AI 驱动的数据提取**：使用 LLM 从网页内容中提取结构化数据
- **研究能力**：基于多来源分析执行深度 Web 研究
- **真实应用**：用于数据提取和研究的实际示例

## 🚀 主要功能

### 🔧 完整工具集
- **单页抓取**：通过高级选项提取单个 URL 的内容
- **批量处理**：并行、高效地抓取多个 URL
- **网站映射**：发现网站中的全部 URL，用于结构探索
- **Web 搜索**：搜索互联网并提取结果页面内容
- **整站爬取**：通过深度控制执行完整的网站分析
- **结构化提取**：使用 AI 从页面中抽取指定数据字段
- **深度研究**：结合多个来源执行深入研究与综合分析
- **LLMs.txt 生成**：为域名生成标准化的 AI 交互指南

### 🌍 高级能力
- **自动限流**：内置重试逻辑和退避策略
- **多种输出格式**：支持 Markdown、HTML 和 JSON
- **内容过滤**：支持高级内容选择和排除规则
- **移动端/桌面端渲染**：可选择不同页面渲染模式
- **身份验证支持**：处理需要登录凭据的网站
- **JavaScript 渲染**：完整支持动态网页内容

## 📋 前置条件

### 必需依赖
1. **Node.js**：Firecrawl MCP 服务器运行所必需
   ```bash
   # 如尚未安装 Node.js，请先安装
   # 安装说明：https://nodejs.org/
   ```

2. **Firecrawl API Key**：从 [Firecrawl.dev](https://firecrawl.dev) 获取 API Key
   ```bash
   # 将 API Key 设置为环境变量
   export FIRECRAWL_API_KEY=your_api_key_here
   ```

3. **Google ADK 依赖**：确保已安装所需包
   ```bash
   pip install -r ../requirements.txt
   ```

## 🛠️ 配置说明

### 1. 环境配置
```bash
# 设置 Firecrawl API Key
export FIRECRAWL_API_KEY=fc-your_api_key_here

# 可选：配置重试参数
export FIRECRAWL_RETRY_MAX_ATTEMPTS=5
export FIRECRAWL_RETRY_INITIAL_DELAY=2000
```

### 2. 安装依赖
```bash
# 在教程根目录执行
pip install -r requirements.txt
```

### 3. 运行 Agent
```bash
# 在教程根目录执行
adk web
```

然后从下拉菜单中选择 `firecrawl_mcp_agent`。

## 🎯 使用示例

### 基础网页抓取
```text
用户：“抓取 https://example.com 的首页”
Agent：调用 firecrawl_scrape，并以 Markdown 格式提取干净内容
```

### 批量 URL 处理
```text
用户：“提取这三篇文章的内容：[url1, url2, url3]”
Agent：调用 firecrawl_batch_scrape 进行高效并行处理
```

### 网站发现
```text
用户：“找出 https://blog.example.com 上所有博客文章 URL”
Agent：调用 firecrawl_map 发现并列出网站中的 URL
```

### Web 搜索与提取
```text
用户：“搜索最近 4 周关于 AI Agent 的研究论文，并提取关键信息”
Agent：调用 firecrawl_search 查找相关论文并提取摘要
```

### 结构化数据提取
```text
用户：“从这个电商页面中提取商品名称、价格和描述”
Agent：调用 firecrawl_extract，并使用自定义 Schema 返回结构化数据
```

### 深度研究
```text
用户：“全面研究可持续能源技术”
Agent：调用 firecrawl_deep_research，执行多来源分析与综合
```

### 网站爬取
```text
用户：“爬取 https://docs.example.com 的文档区域”
Agent：调用 firecrawl_crawl，并设置合适的深度和过滤条件
```

## 🔧 可用工具

### 核心抓取工具
| 工具 | 用途 | 最适合 |
|---|---|---|
| `firecrawl_scrape` | 单页内容提取 | 已知 URL、指定页面 |
| `firecrawl_batch_scrape` | 多 URL 处理 | URL 列表、并行提取 |
| `firecrawl_map` | URL 发现 | 探索网站结构 |

### 高级工具
| 工具 | 用途 | 最适合 |
|---|---|---|
| `firecrawl_search` | Web 搜索 + 内容提取 | 查找相关内容 |
| `firecrawl_crawl` | 整站爬取 | 全面网站分析 |
| `firecrawl_extract` | 结构化数据提取 | 抽取指定数据字段 |
| `firecrawl_deep_research` | 多来源研究 | 复杂研究任务 |

### 实用工具
| 工具 | 用途 | 最适合 |
|---|---|---|
| `firecrawl_generate_llmstxt` | 生成 LLMs.txt | AI 交互指南 |
| `firecrawl_check_crawl_status` | 监控爬取进度 | 长时间运行的爬取任务 |
| `firecrawl_check_batch_status` | 监控批量任务进度 | 批处理状态跟踪 |

## 💡 最佳实践

### 工具选择指南
- **单个 URL**：使用 `firecrawl_scrape`
- **多个已知 URL**：使用 `firecrawl_batch_scrape`
- **发现 URL**：先使用 `firecrawl_map`
- **搜索互联网**：使用 `firecrawl_search`
- **结构化数据**：使用 `firecrawl_extract`
- **深度研究**：使用 `firecrawl_deep_research`
- **整站分析**：使用 `firecrawl_crawl`，并设置合理限制

### 性能优化
- 多 URL 场景优先使用批量操作，而不是逐个抓取
- 为爬取任务设置合适的页面数量和深度限制，避免超时
- 使用状态检查工具监控长时间任务
- 遵守限流规则，并避免对目标网站造成过大压力

### 内容质量
- 使用 `onlyMainContent: true` 提取更干净的正文
- 使用内容过滤选项提升结果质量
- 根据场景选择合适的输出格式：文本优先 Markdown，数据优先 JSON
- 对指定字段需求使用结构化提取

## ⚙️ 配置选项

### 抓取参数
```python
# 抓取操作配置示例
{
    "formats": ["markdown"],           # 输出格式
    "onlyMainContent": True,           # 仅提取正文
    "waitFor": 1000,                   # 页面加载等待时间
    "timeout": 30000,                  # 请求超时
    "mobile": False,                   # 是否使用移动端渲染
    "includeTags": ["article", "main"], # 指定包含的 HTML 标签
    "excludeTags": ["nav", "footer"]    # 指定排除的 HTML 标签
}
```

### 批量处理
```python
# 批处理配置示例
{
    "maxUrls": 50,                    # 最大 URL 数量
    "parallelLimit": 5,               # 并行处理上限
    "options": {
        "formats": ["markdown"],
        "onlyMainContent": True
    }
}
```

### 爬取参数
```python
# 爬取配置示例
{
    "maxDepth": 2,                    # 最大爬取深度
    "limit": 100,                     # 最大页面数量
    "allowExternalLinks": False,      # 限制在当前域名内
    "deduplicateSimilarURLs": True    # 去除相似重复 URL
}
```

## 🚨 重要说明

### 限流
- Firecrawl 内置自动限流和重试逻辑
- 批量任务会进入队列，完成可能需要一定时间
- 对长时间运行任务应使用状态检查工具

### 资源管理
- 整站爬取可能消耗较多资源
- 设置合理限制，避免超时或过高 Token 消耗
- 大型任务应通过批处理状态接口持续检查进度

### API 使用
- 云端操作需要有效的 Firecrawl API Key
- 高用量场景可考虑自托管部署
- 可通过 Firecrawl Dashboard 查看额度使用情况

## 🔍 故障排查

### 常见问题

**连接错误**
```bash
# 检查 Node.js
node --version

# 测试 Firecrawl MCP 服务器
npx -y firecrawl-mcp
```

**API Key 问题**
```bash
# 验证环境变量是否已设置
echo $FIRECRAWL_API_KEY

# 测试 API Key 是否有效
curl -H "Authorization: Bearer $FIRECRAWL_API_KEY" https://api.firecrawl.dev/v1/scrape
```

**找不到工具**
- 确认 ADK MCP 配置正确
- 检查 Node.js 是否安装且可访问
- 确认 Firecrawl MCP 包能够正常安装

### 调试命令
```bash
# 测试 MCP 服务器连接
npx @modelcontextprotocol/inspector

# 使用调试输出运行 Agent
adk web --debug
```

## 📚 更多资源

- **[Firecrawl 文档](https://docs.firecrawl.dev)**：完整 API 参考
- **[Firecrawl MCP Server](https://github.com/mendableai/firecrawl-mcp-server)**：源码与示例
- **[MCP 规范](https://modelcontextprotocol.io/docs/spec)**：协议细节
- **[ADK MCP 文档](https://google.github.io/adk-docs/tools/mcp-tools/)**：集成指南

## 🎯 实际应用场景

### 数据采集与研究
- 市场研究和竞品分析
- 学术研究和论文收集
- 新闻监控和趋势分析
- 商品目录提取
- 社交媒体内容分析

### 内容管理
- 网站迁移和内容审计
- SEO 分析和优化
- 内容质量评估
- 文档提取
- 知识库构建

### 商业智能
- 潜在客户发现和联系方式提取
- 价格监控与比较
- 评论和情感分析
- 行业趋势跟踪
- 合规监管监控
