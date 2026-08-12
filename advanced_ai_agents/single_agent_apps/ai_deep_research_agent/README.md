# 基于 OpenAI Agents SDK 与 Firecrawl 的深度研究 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-deep-research-agent-with-openai-agents-sdk-and-firecrawl)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个功能强大的研究助手，结合 OpenAI Agents SDK 与 Firecrawl 的深度研究能力，可针对任意主题和问题执行全面的 Web 研究。

## 功能特性

- **深度 Web 研究**：自动搜索网页、提取内容并综合研究发现
- **增强分析**：使用 OpenAI Agents SDK 对研究结果进行进一步展开，补充上下文和洞察
- **交互式界面**：提供简洁易用的 Streamlit UI
- **可下载报告**：支持将研究结果导出为 Markdown 文件

## 工作流程

1. **输入阶段**：用户提供研究主题和 API 凭证
2. **研究阶段**：使用 Firecrawl 搜索 Web 并提取相关信息
3. **分析阶段**：根据研究结果生成初步报告
4. **增强阶段**：第二个 Agent 在初步报告基础上补充更多深度、背景与解释
5. **输出阶段**：向用户展示增强后的报告，并支持下载

## 环境要求

- Python 3.8+
- OpenAI API Key
- Firecrawl API Key
- 所需 Python 依赖（见 `requirements.txt`）

## 安装

1. 克隆仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_deep_research_agent
```

2. 安装依赖：

```bash
pip install -r requirements.txt
```

## 使用方式

1. 运行 Streamlit 应用：

```bash
streamlit run deep_research_openai.py
```

2. 在侧边栏输入 API Key：
   - OpenAI API Key
   - Firecrawl API Key

3. 在主输入框中填写研究主题。

4. 点击 `Start Research` 开始研究。

5. 查看并下载增强后的研究报告。

## 示例研究主题

- “量子计算的最新进展”
- “气候变化对海洋生态系统的影响”
- “可再生能源储能技术的发展”
- “人工智能中的伦理问题”
- “远程办公技术的新兴趋势”

## 技术细节

该应用由两个专用 Agent 组成：

1. **Research Agent（研究 Agent）**：调用 Firecrawl 的 deep research endpoint，从多个 Web 来源收集全面信息。

2. **Elaboration Agent（扩展 Agent）**：在初步研究基础上补充详细解释、示例、案例研究和实际影响。

Firecrawl 的 deep research 工具会执行多轮 Web 搜索、内容提取和分析，从而尽可能完整地覆盖研究主题。
