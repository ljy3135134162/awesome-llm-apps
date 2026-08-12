# 💰 基于 Google ADK 的 AI 财务教练 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-multi-agent-personal-finance-coach)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

**AI Financial Coach** 是一个基于 Google ADK（Agent Development Kit）框架构建的个性化财务顾问应用。它会根据用户输入的收入、支出、债务和财务目标，提供综合性的财务分析与建议。

## 功能特性

- **多 Agent 财务分析系统**
  - Budget Analysis Agent：分析消费模式并提出预算优化建议
  - Savings Strategy Agent：制定个性化储蓄计划和应急资金策略
  - Debt Reduction Agent：使用债务雪崩法和雪球法制定优化的还债方案

- **支出分析**
  - 支持上传 CSV，也支持手动录入支出
  - 可基于日期、类别和金额分析 CSV 交易记录
  - 按类别可视化展示支出构成
  - 自动识别支出类别和消费模式

- **储蓄建议**
  - 评估应急资金规模并制定建立策略
  - 根据不同目标制定自定义储蓄分配方案
  - 提供可执行的自动储蓄方式
  - 提供进度跟踪和阶段性目标建议

- **债务管理**
  - 支持多笔债务，并根据利率优化偿还顺序
  - 对比债务雪崩法与雪球法
  - 可视化展示还债时间线和利息节省情况
  - 提供可执行的债务削减建议

- **交互式可视化**
  - 支出分类饼图
  - 收入与支出柱状图
  - 债务对比图
  - 财务进度指标

## 运行方式

按照以下步骤配置并运行应用：

1. **获取 API Key**
   - 从 Google AI Studio 获取免费的 Gemini API Key：https://aistudio.google.com/apikey
   - 在项目根目录创建 `.env` 文件，并加入：

```env
GOOGLE_API_KEY=your_api_key_here
```

2. **克隆仓库**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/advanced_ai_agents/multi_agent_apps/ai_financial_coach_agent/
```

3. **安装依赖**

```bash
pip install -r requirements.txt
```

4. **运行 Streamlit 应用**

```bash
streamlit run ai_financial_coach_agent.py
```

## CSV 文件格式

应用接受包含以下必填列的 CSV 文件：

- `Date`：交易日期，格式为 YYYY-MM-DD
- `Category`：支出类别
- `Amount`：交易金额，支持货币符号和千位分隔符

示例：

```csv
Date,Category,Amount
2024-01-01,Housing,1200.00
2024-01-02,Food,150.50
2024-01-03,Transportation,45.00
```

可以直接从应用侧边栏下载 CSV 模板文件。
