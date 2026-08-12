## 🔍 AI 欺诈调查 Agent

这是一个由 AI 驱动的自主欺诈调查 Agent，用于将托儿服务提供商的许可记录与实际建筑数据进行交叉核验，从而发现异常。该 Agent 使用公开数据，包括 Cook County 房产记录、Illinois DCFS 许可信息、Google Maps 以及 Secretary of State 企业注册数据，寻找“纸面记录”与“现实证据”不一致的机构。

### 功能特性

- 按 ZIP Code 搜索 Illinois DCFS 许可数据库中的托儿服务提供商
- 将许可容量与 Cook County GIS 记录中的实际建筑面积进行交叉核验
- 使用 Illinois 建筑规范计算合法最大托儿人数
- 分析 Google Street View 图像，验证目标地点是否看起来确实像托儿机构
- 查询 Google Places，检查营业状态、评分，以及该地址是否实际对应其他商户
- 通过 Illinois Secretary of State 验证企业实体注册状态
- 发现跨机构模式，例如共同所有人、地址聚集、缺乏公开足迹的实体
- 实时展示完整调查过程，让 Agent 的分析逻辑和调查步骤可见

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_fraud_investigation_agent
```

2. 安装依赖

```bash
pip install -r requirements.txt
```

3. 获取 OpenRouter API Key

- 在 [openrouter.ai](https://openrouter.ai) 注册并创建 API Key
- 免费额度通常足够用于 Demo 调查
- 默认模型：`anthropic/claude-sonnet-4.6`

4. 获取 Google Maps API Key（可选；未提供时会跳过视觉分析）

- 在 [console.cloud.google.com](https://console.cloud.google.com) 创建项目
- 启用：**Geocoding API**、**Places API**、**Street View Static API**
- 创建 API Key，并将权限限制到上述三个 API

5. 运行 Streamlit 应用

```bash
streamlit run fraud_investigation_agent.py
```

### 工作原理

AI Fraud Investigation Agent 使用 7 个专业工具，全部基于公开数据源：

- **Provider Search** —— 查询 Illinois DCFS 许可门户，获取指定 ZIP Code 下所有活跃服务商，并返回许可容量、许可类型和许可状态

- **Property Analysis** —— 通过 Cook County Assessor 开放数据获取建筑面积、土地面积、房产类别和建造年份（使用 Socrata API，无需认证）

- **Capacity Calculation** —— 使用 Illinois DCFS Part 407 的建筑规范公式：`(building_sqft × 0.65) ÷ 35 = max legal children`。例如，900 平方英尺的建筑不可能合法容纳 50 名儿童，这属于数学层面的不可能，而非主观判断

- **Street View** —— 获取四个方向的 Google Street View 图像，判断地址看起来是否确实像托儿设施，还是属于其他用途

- **Places Info** —— 查询 Google Places，检查该地址当前登记的商户、营业状态、评分和近期评论

- **Business Registration** —— 查询 Illinois Secretary of State，验证服务商是否为合法注册实体

- **Geocoding** —— 将地址转换成坐标，用于空间分析

Agent 会逐个调查 ZIP Code 内的服务提供商，并实时解释其分析过程。当发现可疑情况，例如建筑面积不足以支撑许可容量、已经关闭的店面却登记为托儿机构，或者同一个名称出现在多个服务商记录中时，它会继续沿该线索深入调查，并解释其重要性。

### Agent 能检测和不能检测的内容

**可以检测：**
- 许可容量在建筑面积上明显不可能实现
- Google 显示该地址属于其他商户，或建筑已经关闭、空置
- 没有 Google 商户信息、没有评价或没有企业注册记录的服务商
- 多个服务商之间共享所有人姓名或注册代理人的情况

**无法检测：**
- 出勤欺诈，例如为实际未到场的儿童收费；这需要非公开的 CCAP Billing Records
- 任何必须访问 DHS 或 County 内部账单数据才能判断的欺诈行为

所有发现都只能视为调查线索，而不是法律结论。Agent 会使用“需要进一步调查”“存在异常”等表达，而不会直接认定某个机构“存在欺诈”。

### 地理范围

该 Demo 当前仅覆盖 **Illinois 州 Cook County**。房产信息来源于 Cook County Assessor 开放数据，因此 ZIP Code 选择器仅覆盖 10 个芝加哥高密度社区：

| ZIP | 社区 |
|-----|------|
| 60623 | Little Village / North Lawndale |
| 60629 | Chicago Lawn |
| 60644 | Austin |
| 60621 | Englewood |
| 60628 | Roseland |
| 60619 | Chatham / Auburn Gresham |
| 60636 | West Englewood |
| 60612 | Near West Side |
| 60620 | Auburn Gresham |
| 60624 | Garfield Park |

完整的 [Surelock Homes](https://github.com/oso95/Surelock-Homes) 系统还支持 Minnesota 以及 Illinois 的其他 County，并提供 FastAPI 后端、流式 Dashboard 和离线模式。
