# 🚀 AI 邮件 GTM 外联 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-email-gtm-outreach-agent-team)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个智能、全自动化的 B2B 外联系统。它使用 AI Agent 自动发现目标公司、寻找决策者、研究公司情报，并生成个性化冷邮件。

## ✨ 功能特性

### 🔍 自动发现目标公司
- 使用 Exa Search 根据行业、规模和业务条件寻找目标公司
- 识别处于增长、近期融资或扩张阶段的公司
- 支持 SaaS/科技、电商/零售、金融服务、医疗/生物科技、制造/工业等类别

### 👥 智能寻找联系人
- 自动寻找目标部门中的关键决策者
- 查找邮箱地址和 LinkedIn 资料
- 可针对 CEO、CTO、VP of Engineering、CMO、VP Marketing、Sales Director、HR Director 等职位

### 🔬 深度公司研究
- 收集完整的公司情报
- 分析官网、近期新闻、产品服务和技术栈
- 识别痛点、增长机会和市场定位
- 提取适用于个性化外联的信息

### ✉️ 个性化邮件生成
- 根据研究结果生成高度个性化的冷邮件
- 针对不同职业和部门使用专用模板
- 保持友好、自然的对话式语气
- 避免企业套话和陈词滥调
- 在邮件中引用目标公司的具体挑战和成果

### 🎯 智能目标筛选
- **公司类别**：SaaS/科技、电商、金融服务、医疗、制造业
- **公司规模**：Startup（1-50）、SMB（51-500）、Enterprise（500+）、全部规模
- **目标部门**：GTM（销售与市场）、HR、工程/技术、运营、财务、产品、高管团队
- **服务类型**：软件解决方案、咨询服务、专业服务、技术平台、定制开发

## 🛠️ 安装

1. **克隆仓库**

```bash
git clone <repository-url>
cd ai_email_gtm_reachout_agent
```

2. **安装依赖**

```bash
pip install -r requirements.txt
```

**注意**：请确保安装 Agno 2.0.4 或更高版本。

3. **配置 API Keys**

```bash
# 必需的 API Keys
export EXA_API_KEY="your_exa_api_key"
export OPENAI_API_KEY="your_openai_api_key"
```

## 🚀 快速开始

1. **运行应用**

```bash
streamlit run ai_email_gtm_reachout.py
```

2. **配置外联 Campaign**：
   - 选择目标公司类别和规模
   - 选择需要联系的部门
   - 输入你的联系信息
   - 描述你提供的服务
   - 选择个性化程度

3. **启动自动化 Campaign**：
   - 点击 `Start Automated Campaign`
   - AI 会自动发现公司、寻找联系人、研究详细信息并生成个性化邮件

## 📋 使用指南

### 第 1 步：发现目标公司
- 从预设公司类别中选择目标类型
- 选择公司规模
- 指定需要寻找的公司数量（1-20）

### 第 2 步：填写你的信息
- **必填**：姓名、邮箱、组织、服务说明
- **可选**：LinkedIn、电话、网站、Calendar Link

### 第 3 步：配置外联
- 选择服务/产品类别
- 选择个性化级别（Basic / Medium / Deep）
- 选择目标部门

### 第 4 步：生成 Campaign
- 检查配置
- 点击 `Start Automated Campaign`
- 查看执行进度以及生成的邮件

## 🎨 邮件模板

系统包含针对不同部门的专用模板：

### GTM（销售与市场）
- 软件解决方案模板
- 咨询服务模板

### Human Resources
- 软件解决方案模板
- 咨询服务模板
- 投资机会模板

### Marketing Professional
- 产品 Demo 模板
- 服务推广模板

### B2B Sales Representative
- 产品 Demo 模板
- 服务推广模板

## 🔧 配置选项

### 公司类别
- **SaaS/Technology Companies**：软件、云服务、技术平台
- **E-commerce/Retail**：在线零售、Marketplace、D2C 品牌
- **Financial Services**：银行、Fintech、保险、投资机构
- **Healthcare/Biotech**：医疗服务商、生物科技、医疗科技
- **Manufacturing/Industrial**：制造业、工业自动化、供应链

### 个性化级别
- **Basic**：使用公司名称和基本资料进行标准个性化
- **Medium**：加入公司近期新闻和成果
- **Deep**：结合具体痛点和业务机会进行深度个性化

## 📊 输出格式

每个生成结果包括：
- **Personalized Email**：可直接使用的个性化冷邮件
- **Company Research**：详细公司情报
- **Contacts Found**：发现的决策者和联系人信息

## 🔑 API 要求

### Exa API
- 用于发现公司和执行公司研究
- API Key 可从 [exa.ai](https://exa.ai) 获取
- 查找目标公司和收集情报时必须使用

### OpenAI API
- 用于邮件和内容生成
- API Key 可从 [platform.openai.com](https://platform.openai.com) 获取
- AI 个性化邮件生成必须使用

## 🎯 使用场景

### 销售团队
- 自动执行 B2B Prospecting
- 大规模个性化外联
- 精确定位特定行业和公司规模

### 营销机构
- 客户开发 Campaign
- 为多个客户生成 Lead
- 针对不同行业制定外联策略

### 顾问
- 自动化 Business Development
- 推广专业服务
- 扩展职业网络

### 创业公司
- 投资人外联
- 商业合作开发
- 客户获取
