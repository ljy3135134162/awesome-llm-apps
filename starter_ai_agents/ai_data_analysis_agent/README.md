# 📊 AI 数据分析 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-data-analysis-agent)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个使用 Agno Agent 框架和 OpenAI `gpt-4o` 模型构建的 AI 数据分析 Agent。它可以通过自然语言查询分析 CSV、Excel 等数据文件，并结合 OpenAI 语言模型与 DuckDB 高效处理数据，让没有 SQL 经验的用户也能完成数据分析。

## 功能特性

- 📤 **文件上传支持：**
  - 上传 CSV 和 Excel 文件
  - 自动检测数据类型并推断 Schema
  - 支持多种文件格式

- 💬 **自然语言查询：**
  - 将自然语言问题转换为 SQL 查询
  - 快速获得数据相关答案
  - 无需掌握 SQL

- 🔍 **高级分析：**
  - 执行复杂的数据聚合
  - 数据筛选和排序
  - 生成统计摘要
  - 创建数据可视化

- 🎯 **交互式 UI：**
  - 易于使用的 Streamlit 界面
  - 实时处理查询
  - 清晰展示分析结果

## 运行方式

1. **配置环境**

```bash
# 克隆仓库
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/starter_ai_agents/ai_data_analysis_agent

# 安装依赖
pip install -r requirements.txt
```

2. **配置 API Key**
   - 从 [OpenAI Platform](https://platform.openai.com) 获取 OpenAI API Key。

3. **运行应用**

```bash
streamlit run ai_data_analyst.py
```

## 使用方式

1. 使用上述命令启动应用。
2. 在 Streamlit 侧边栏中提供 OpenAI API Key。
3. 通过 Streamlit 界面上传 CSV 或 Excel 文件。
4. 使用自然语言针对数据提出问题。
5. 查看分析结果以及自动生成的数据可视化。
