# 📊 AI 风投尽调智能体团队

这是一个面向初创公司投资分析的多智能体 AI 流水线，基于 [Google ADK](https://google.github.io/adk-docs/)、Gemini 3 Pro、Gemini 3 Flash 和 Nano Banana Pro 构建。

**适用于任何初创公司**——无论是早期、知名度很低的项目，还是已经获得大量融资的公司。你只需要提供公司名称、网站 URL，或两者同时提供即可。

## 功能特性

- 🔍 **实时研究** - 实时网络搜索公司和市场数据
- 🌐 **URL 支持** - 可通过网站 URL 分析任意初创公司
- 📈 **收入图表** - 使用 matplotlib 生成悲观/基准/乐观三种情景的预测图
- 🧠 **深度风险分析** - 从 5 个维度进行全面风险评估
- 📄 **专业报告** - 生成麦肯锡风格的 HTML 投资报告
- 🎨 **可视化摘要** - 生成 AI 信息图，便于快速浏览结论

## 它能做什么

给定一个初创公司名称或 URL 后，该流水线会自动：

1. **研究公司** - 创始人、融资、产品、业务进展
2. **分析市场** - TAM/SAM、竞争对手、市场定位
3. **建立财务模型** - 收入预测、单位经济模型
4. **评估风险** - 市场、执行、财务、监管、退出风险
5. **生成投资备忘录** - 形成结构化投资论点
6. **创建 HTML 报告** - 输出专业尽调文档
7. **生成信息图** - 生成便于快速评审的可视化摘要

## 快速开始

### 1. 克隆并进入项目目录
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/advanced_ai_agents/multi_agent_apps/agent_teams/ai_vc_due_diligence_agent_team
```

### 2. 配置环境变量
```bash
export GOOGLE_API_KEY=your_api_key
# 或创建 .env 文件：
echo "GOOGLE_API_KEY=your_api_key" > .env
```

### 3. 安装并运行
```bash
pip install -r requirements.txt
adk web
```

### 4. 尝试使用
支持公司名称、URL，或两者同时输入：

打开 `http://localhost:8000`，可以尝试：
- *"Analyze https://agno.com for Series A investment of $30-50M"*
- *"Research Genspark AI for its next funding round"*
- *"Analyze Lovable for Series C funding opportunities"*
- *"Research emergent.sh for Series B funding in the $40-60M range"*

## 流水线架构

```text
用户请求："Analyze https://agno.com for Series A"
    │
    ▼
┌─────────────────────────────────────────────────────────────────┐
│              DueDiligencePipeline (SequentialAgent)             │
│                                                                 │
│  ┌─────────────┐    ┌────────────────┐    ┌──────────────────┐  │
│  │   阶段 1    │    │     阶段 2     │    │      阶段 3      │  │
│  │   公司研究   │───▶│     市场分析    │───▶│      财务建模      │  │
│  └─────────────┘    └────────────────┘    └──────────────────┘  │
│         │                   │                      │            │
│         ▼                   ▼                      ▼            │
│  ┌─────────────┐    ┌────────────────┐    ┌──────────────────┐  │
│  │   阶段 4    │    │     阶段 5     │    │      阶段 6      │  │
│  │   风险评估   │───▶│    投资备忘录    │───▶│      报告生成      │  │
│  └─────────────┘    └────────────────┘    └──────────────────┘  │
│                                                    │            │
│                                                    ▼            │
│                                          ┌──────────────────┐   │
│                                          │      阶段 7      │   │
│                                          │      信息图生成     │   │
│                                          └──────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
    │
    ▼
产物：revenue_chart.png、investment_report.html、infographic.png
```

---

## 智能体详情

### 阶段 1：公司研究智能体

**用途：** 通过网络搜索收集全面的公司信息。

| 属性 | 值 |
|---|---|
| 模型 | `gemini-3-flash-preview` |
| 工具 | `google_search` |
| 输出键 | `company_info` |

**研究内容：**
- **公司基础信息** - 业务内容、成立时间、总部位置、团队规模
- **创始人与团队** - 核心人员、背景、LinkedIn 信息
- **产品/技术** - 核心产品、工作方式、目标客户
- **融资历史** - 融资轮次、投资方、融资金额
- **业务进展** - 客户、合作伙伴、增长信号
- **近期新闻** - 媒体报道、产品发布、公司公告

**针对早期初创公司：** 会检查官网、LinkedIn、Crunchbase、AngelList、创始人访谈等来源，并在信息不足时明确标注。

---

### 阶段 2：市场分析智能体

**用途：** 分析市场规模、竞争格局和市场定位。

| 属性 | 值 |
|---|---|
| 模型 | `gemini-3-flash-preview` |
| 工具 | `google_search` |
| 输入 | `{company_info}` |
| 输出键 | `market_analysis` |

**分析内容：**
- **市场规模** - 来自行业报告的 TAM、SAM、增长率
- **竞争对手** - 同领域公司及其融资/业务进展
- **市场定位** - 公司如何形成差异化
- **趋势** - 市场驱动因素、新兴技术、监管变化

**针对早期项目：** 会更关注大类市场、已融资竞争对手和市场验证信号。

---

### 阶段 3：财务建模智能体

**用途：** 构建收入预测并生成财务图表。

| 属性 | 值 |
|---|---|
| 模型 | `gemini-3-pro-preview` |
| 工具 | `generate_financial_chart` |
| 输入 | `{company_info}`, `{market_analysis}` |
| 输出键 | `financial_model` |

**计算内容：**
- **当前指标** - 估算 ARR、增长阶段
- **增长情景**（5 年预测）：
  - 悲观情景：保守增长率
  - 基准情景：预期增长轨迹
  - 乐观情景：较乐观增长假设
- **回报分析** - 退出估值、MOIC、IRR 估算

**不同阶段参考指标：**
- Seed：ARR 约 $0.1-0.5M，年增长 3-5 倍
- Series A：ARR 约 $1-3M，年增长 2-3 倍
- Series B：ARR 约 $5-15M，年增长 1.5-2 倍

**产物：** 保存 `revenue_chart_TIMESTAMP.png`，展示悲观/基准/乐观三种情景预测。

---

### 阶段 4：风险评估智能体

**用途：** 从多个维度执行深入风险分析。

| 属性 | 值 |
|---|---|
| 模型 | `gemini-3-pro-preview` |
| 工具 | 无（扩展推理） |
| 输入 | `{company_info}`, `{market_analysis}`, `{financial_model}` |
| 输出键 | `risk_assessment` |

**风险类别：**
1. **市场风险** - 竞争、市场时机、采用门槛
2. **执行风险** - 团队缺口、技术挑战、规模化
3. **财务风险** - 烧钱速度、融资、单位经济模型
4. **监管风险** - 合规、法律、地缘政治
5. **退出风险** - 潜在收购方、IPO 可行性

**每项风险会给出：**
- 严重程度（Low/Medium/High/Critical）
- 带证据的风险描述
- 缓解策略

**最终输出：**
- 总体风险评分（1-10）
- 最可能导致投资失败的 3 大风险
- 建议的保护性投资条款

---

### 阶段 5：投资备忘录智能体

**用途：** 将全部研究结果汇总成结构化投资备忘录。

| 属性 | 值 |
|---|---|
| 模型 | `gemini-3-pro-preview` |
| 工具 | 无 |
| 输入 | 所有前置阶段结果 |
| 输出键 | `investor_memo` |

**备忘录结构：**
1. **执行摘要** - 公司一句话介绍、投资建议、关键亮点
2. **公司概览** - 业务、团队、产品/技术
3. **融资与估值** - 历史融资、估值区间
4. **市场机会** - 市场规模、增长、竞争对手、差异化
5. **财务分析** - 收入、单位经济模型、现金跑道
6. **风险分析** - 核心风险、严重度、总体评分
7. **投资逻辑** - 投资理由、主要顾虑、回报情景
8. **最终建议** - 结论和下一步建议

**建议等级：** Strong Buy / Buy / Hold / Pass

---

### 阶段 6：报告生成智能体

**用途：** 生成专业的 HTML 投资报告。

| 属性 | 值 |
|---|---|
| 模型 | `gemini-3-flash-preview` |
| 工具 | `generate_html_report` |
| 输入 | `{investor_memo}` |
| 输出键 | `html_report_result` |

**报告特性：**
- 麦肯锡 / 高盛风格
- 深蓝色（#1a365d）与金色（#d4af37）配色
- 顶部执行摘要
- 清晰的章节标题和专业排版
- 用表格展示关键指标
- 适合打印的布局

**产物：** 保存 `investment_report_TIMESTAMP.html`，可直接在浏览器中查看。

---

### 阶段 7：信息图生成智能体

**用途：** 使用 AI 图像生成能力制作可视化摘要。

| 属性 | 值 |
|---|---|
| 模型 | `gemini-3-flash-preview` |
| 工具 | `generate_infographic`（使用 `gemini-3-pro-image-preview`） |
| 输入 | `{investor_memo}` |
| 输出键 | `infographic_result` |

**信息图包含：**
- 醒目的公司名称
- 大字号展示关键指标
- 市场规模可视化
- 风险评分指示器（1-10）
- 投资建议徽章
- 专业投资银行风格

**产物：** 保存 `infographic_TIMESTAMP.png`，便于快速浏览。

---

## 项目结构

```text
ai_due_diligence_agent/
├── __init__.py        # 导出 root_agent
├── agent.py           # 定义全部 7 个智能体和流水线
├── tools.py           # 自定义工具（图表、报告、信息图）
├── outputs/           # 保存生成产物
├── requirements.txt   # Python 依赖
└── README.md          # 本文档
```

## 生成产物

所有产物都会保存到 ADK Web 的 **Artifacts 标签页** 以及 **`outputs/`** 目录：

```text
outputs/
├── revenue_chart_20260104_143030.png        # 财务预测
├── investment_report_20260104_143052.html   # 完整 HTML 报告
└── infographic_20260104_143105.png          # 可视化摘要
```

| 产物 | 格式 | 说明 |
|---|---|---|
| Revenue Chart | PNG | 悲观/基准/乐观三种 5 年预测 |
| Investment Report | HTML | 完整尽调文档 |
| Infographic | PNG/JPG | 一页式可视化摘要 |

---

## 展示的 ADK 功能

| 功能 | 用途 |
|---|---|
| **SequentialAgent** | 7 阶段流水线编排 |
| **LlmAgent** | 各个专业智能体 |
| **google_search** | 实时公司/市场研究 |
| **Custom Tools** | 图表生成、HTML 报告、信息图 |
| **Artifacts** | 保存并管理生成文件版本 |
| **State Management** | 通过 `output_key` 在阶段之间传递数据 |
| **Multi-modal Output** | 文本分析 + 图像生成 |

## 使用的模型

| 智能体 | 模型 | 原因 |
|---|---|---|
| CompanyResearch | `gemini-3-flash-preview` | 快速网络搜索 |
| MarketAnalysis | `gemini-3-flash-preview` | 快速网络搜索 |
| FinancialModeling | `gemini-3-pro-preview` | 复杂计算 |
| RiskAssessment | `gemini-3-pro-preview` | 深度推理 |
| InvestorMemo | `gemini-3-pro-preview` | 高质量综合分析 |
| ReportGenerator | `gemini-3-flash-preview` | 快速 HTML 生成 |
| InfographicGenerator | `gemini-3-flash-preview` | 流程编排 |
| Infographic Tool | `gemini-3-pro-image-preview` | 图像生成 |

---

## 了解更多

- [Google ADK 文档](https://google.github.io/adk-docs/)
- [ADK 中的多智能体模式](https://developers.googleblog.com/developers-guide-to-multi-agent-patterns-in-adk/)
- [Gemini API](https://ai.google.dev/gemini-api/docs)
- [Gemini 图像生成](https://ai.google.dev/gemini-api/docs/image-generation)
