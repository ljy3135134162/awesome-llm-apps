# 👨🏻‍💼 AI 销售情报智能体团队

这是一个基于 [Google ADK](https://google.github.io/adk-docs/) 和 Gemini 3 构建的多智能体 AI 流水线，可实时生成竞争销售 Battle Card（竞品作战卡）。

**输入一个竞争对手 + 你的产品** → 即可获得完整的 Battle Card，包括定位策略、异议处理话术以及可视化对比。

## 功能特性

- 🔍 **实时研究** - 通过实时网页搜索获取竞争对手情报
- 📊 **功能分析** - 深入分析竞争产品能力
- 🎯 **定位情报** - 挖掘竞争对手如何进行市场定位以及如何针对你的产品进行宣传
- ⚖️ **SWOT 分析** - 客观比较双方优劣势
- 💬 **异议处理话术** - 生成可直接用于销售通话的回复脚本
- 📄 **Battle Card** - 为销售人员生成专业 HTML 竞品作战卡
- 📈 **对比信息图** - 使用 Gemini 图像模型生成 AI 可视化竞品对比图

## 它能做什么

给定一个竞争对手和你的产品后，流水线会自动：

1. **研究竞争对手** - 公司信息、融资、客户、评价
2. **分析产品功能** - 能力、集成、定价
3. **挖掘定位策略** - 品牌话术、目标用户画像、分析师覆盖情况
4. **生成 SWOT 分析** - 你在哪些方面占优，对方在哪些方面占优
5. **生成异议处理脚本** - 针对最常见的 10 个异议生成回复
6. **构建 Battle Card** - 为销售团队生成专业 HTML 文档
7. **生成对比图表** - 创建功能级可视化对比

## 快速开始

### 1. 进入项目目录
```bash
cd awesome-llm-apps/advanced_ai_agents/multi_agent_apps/agent_team/ai_sales_intelligence_team
```

### 2. 配置环境变量
```bash
export GOOGLE_API_KEY=your_api_key
```

### 3. 安装并运行
```bash
pip install -r requirements.txt
adk web
```

### 4. 尝试使用
打开 `http://localhost:8000`，可尝试：
- *"为 Salesforce 创建一份 Battle Card。我们销售 HubSpot。"*
- *"针对 Slack 生成 Battle Card，我们销售 Microsoft Teams。"*
- *"帮我对抗 Zendesk，我卖的是 Freshdesk。"*

## 示例提示词

| 你的产品 | 竞争对手 | 提示词 |
|--------------|------------|--------|
| HubSpot | Salesforce | "Create a battle card for Salesforce. We sell HubSpot." |
| Asana | Monday.com | "Battle card against Monday.com, I sell Asana" |
| Zoom | Microsoft Teams | "Competitive analysis: Zoom vs our product Teams" |
| Notion | Confluence | "Help me compete against Confluence, we're Notion" |

---

## 流水线架构

```text
用户请求: "Battle card for Salesforce. We sell HubSpot."
    │
    ▼
┌─────────────────────────────────────────────────────────────────┐
│               BattleCardPipeline (SequentialAgent)              │
│                                                                 │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────┐  │
│  │    Stage 1      │    │    Stage 2      │    │   Stage 3   │  │
│  │   Competitor    │───▶│    Product      │───▶│ Positioning │  │
│  │   Research      │    │    Features     │    │  Analyzer   │  │
│  └─────────────────┘    └─────────────────┘    └─────────────┘  │
│           │                     │                     │         │
│           ▼                     ▼                     ▼         │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────┐  │
│  │    Stage 4      │    │    Stage 5      │    │   Stage 6   │  │
│  │      SWOT       │───▶│   Objection     │───▶│ Battle Card │  │
│  │    Analysis     │    │    Handler      │    │  Generator  │  │
│  └─────────────────┘    └─────────────────┘    └─────────────┘  │
│                                                       │         │
│                                                       ▼         │
│                                              ┌─────────────┐    │
│                                              │   Stage 7   │    │
│                                              │ Comparison  │    │
│                                              │    Chart    │    │
│                                              └─────────────┘    │
└─────────────────────────────────────────────────────────────────┘
    │
    ▼
产物: battle_card.html, comparison_chart.png
```

---

## 智能体详情

### 阶段 1：竞争对手研究智能体

**用途：** 通过网页搜索收集完整的竞争对手情报。

| 属性 | 值 |
|----------|-------|
| 模型 | `gemini-3-flash-preview` |
| 工具 | `google_search` |
| 输出键 | `competitor_profile` |

**研究内容：**
- 公司概况（成立时间、总部、规模、融资）
- 目标市场和理想客户
- 产品及定价层级
- 最新新闻、发布、收购
- 客户评价（G2、Capterra、TrustRadius）

---

### 阶段 2：产品功能智能体

**用途：** 深入分析竞争对手产品能力。

| 属性 | 值 |
|----------|-------|
| 模型 | `gemini-3-flash-preview` |
| 工具 | `google_search` |
| 输出键 | `feature_analysis` |

**分析内容：**
- 核心功能与能力
- 集成与生态
- 技术架构（云、API、移动端）
- 定价细节和隐藏成本
- 用户评价中提到的已知限制

---

### 阶段 3：市场定位分析智能体

**用途：** 分析竞争对手的定位和市场话术策略。

| 属性 | 值 |
|----------|-------|
| 模型 | `gemini-3-pro-preview` |
| 工具 | `google_search` |
| 输出键 | `positioning_intel` |

**挖掘内容：**
- 市场营销话术和宣传标语
- 重点目标用户画像
- 他们如何针对你的产品进行定位
- 分析师覆盖（Gartner、Forrester、G2）
- 社会证明和客户案例

---

### 阶段 4：SWOT 分析智能体

**用途：** 生成客观的优势与劣势分析。

| 属性 | 值 |
|----------|-------|
| 模型 | `gemini-3-pro-preview` |
| 工具 | 无（综合分析） |
| 输出键 | `swot_analysis` |

**输出内容：**
- 竞争对手最主要的 5 项优势（附证据）
- 竞争对手最主要的 5 项弱点
- 你的产品在哪些方面领先
- 在竞争性销售机会中可提前布置的“地雷”问题

---

### 阶段 5：异议处理智能体

**用途：** 为竞争性销售场景生成异议处理脚本。

| 属性 | 值 |
|----------|-------|
| 模型 | `gemini-3-pro-preview` |
| 工具 | 无（综合分析） |
| 输出键 | `objection_scripts` |

**生成内容：**
- 最常见的 10 个异议及标准回复
- 每个回复对应的证明点
- 可向潜在客户提出的关键问题
- 可在销售初期设置竞争壁垒的话术

---

### 阶段 6：Battle Card 生成智能体

**用途：** 生成专业 HTML Battle Card。

| 属性 | 值 |
|----------|-------|
| 模型 | `gemini-3-flash-preview` |
| 工具 | `generate_battle_card_html` |
| 输出键 | `battle_card_result` |

**Battle Card 包括：**
- 快速统计信息
- 一览式对比（我方优势 / 对方优势 / 势均力敌）
- 功能对比表
- 异议处理速查表
- 关键问题
- 可提前布置的竞争“地雷”

**产物：** `battle_card_TIMESTAMP.html`

---

### 阶段 7：对比图表智能体

**用途：** 使用 Gemini 图像生成能力创建竞品对比信息图。

| 属性 | 值 |
|----------|-------|
| 模型 | `gemini-3-flash-preview` |
| 工具 | `generate_comparison_chart`（使用 `gemini-2.0-flash-exp`） |
| 输出键 | `chart_result` |

**信息图特性：**
- AI 生成的专业竞品对比图
- 并排功能对比可视化
- 分数颜色编码（绿色 = 你方，红色 = 竞争对手）
- 高亮关键差异点
- 总体结论标签

**产物：** `comparison_infographic_TIMESTAMP.png`

---

## 项目结构

```text
ai_battle_card_agent/
├── __init__.py        # 导出 root_agent
├── agent.py           # 全部 7 个智能体 + 流水线
├── tools.py           # Battle Card HTML + 对比图表工具
├── outputs/           # 保存生成产物
├── requirements.txt   # 依赖
└── README.md          # 当前文档
```

## 生成产物

所有产物都会保存到 ADK Web 的 **Artifacts** 标签页以及 **`outputs/`** 文件夹：

```text
outputs/
├── battle_card_20260104_143052.html           # 完整 Battle Card 文档
└── comparison_infographic_20260104_143105.png # AI 生成的对比图
```

| 产物 | 格式 | 说明 |
|----------|--------|-------------|
| Battle Card | HTML | 可直接用于销售的竞品作战卡 |
| 对比信息图 | PNG/JPG | AI 生成的可视化竞品对比（Gemini 图像） |

---

## Battle Card 内容结构

生成的 HTML Battle Card 包括：

1. **页眉** - 竞争对手名称、最后更新时间
2. **快速统计** - 5-6 条关键信息
3. **一览对比** - 三列：我方优势 | 对方优势 | 势均力敌
4. **功能对比** - 带勾选标识的对比表
5. **对方优势** - 红色标记（需客观）
6. **对方弱点** - 绿色标记（机会点）
7. **异议处理** - 最主要的 5 个异议及快速回复
8. **关键问题** - 可向潜在客户提出的问题
9. **竞争地雷** - 可在竞争销售中提前设置的问题或陷阱

---

## 展示的 ADK 特性

| 特性 | 用法 |
|---------|-------|
| **SequentialAgent** | 7 阶段流水线编排 |
| **google_search** | 实时竞争对手研究 |
| **自定义工具** | HTML Battle Card、AI 生成信息图 |
| **图像生成** | 使用 Gemini 图像模型生成对比图 |
| **Artifacts** | 按会话保存 Battle Card |
| **状态管理** | 通过 `output_key` 在不同阶段之间传递研究结果 |
| **Coordinator Pattern** | 根智能体将请求路由至流水线 |

## 使用的模型

| 智能体 | 模型 | 原因 |
|-------|-------|-----|
| CompetitorResearch | `gemini-3-flash-preview` | 快速网页搜索 |
| ProductFeature | `gemini-3-flash-preview` | 快速网页搜索 |
| PositioningAnalyzer | `gemini-3-pro-preview` | 战略分析 |
| SWOT | `gemini-3-pro-preview` | 深度综合 |
| ObjectionHandler | `gemini-3-pro-preview` | 提升脚本质量 |
| BattleCardGenerator | `gemini-3-flash-preview` | HTML 生成 |
| ComparisonChart Agent | `gemini-3-flash-preview` | 流程编排 |
| Comparison Tool | `gemini-3-pro-image-preview` | 图像生成 |

---

## 了解更多

- [Google ADK 文档](https://google.github.io/adk-docs/)
- [ADK 多智能体模式](https://developers.googleblog.com/developers-guide-to-multi-agent-patterns-in-adk/)
- [Gemini API](https://ai.google.dev/gemini-api/docs)
