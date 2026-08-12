# 📊 AI 数据可视化 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-data-visualization-agent)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个由 LLM 驱动、充当个人数据可视化专家的 Streamlit 应用。只需上传数据集并使用自然语言提问，AI Agent 就会分析数据、生成合适的可视化，并通过图表、统计数据和文字解释提供洞察。

## 功能特性

#### 自然语言数据分析
- 使用自然语言针对数据提出问题
- 快速获得可视化和统计分析结果
- 获取对分析发现和洞察的解释
- 支持交互式追问

#### 智能可视化选择
- 自动选择适合数据和问题的图表类型
- 动态生成可视化
- 支持统计类可视化
- 支持自定义图表格式与样式

#### 多模型 AI 支持
- Meta-Llama 3.1 405B：用于复杂分析
- DeepSeek V3：用于详细洞察
- Qwen 2.5 7B：用于快速分析
- Meta-Llama 3.3 70B：用于高级查询

## 运行方式

按照以下步骤配置并运行应用：

- 首先在这里获取免费的 Together AI API Key：https://api.together.ai/signin
- 在这里获取免费的 E2B API Key：https://e2b.dev/；https://e2b.dev/docs/legacy/getting-started/api-key

1. **克隆仓库**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd starter_ai_agents/ai_data_visualisation_agent
```

2. **安装依赖**

```bash
pip install -r requirements.txt
```

3. **运行 Streamlit 应用**

```bash
streamlit run ai_data_visualisation_agent.py
```
