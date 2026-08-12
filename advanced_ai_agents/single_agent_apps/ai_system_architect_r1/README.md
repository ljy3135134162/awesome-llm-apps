# 🤖 AI 系统架构顾问（R1）

这是一个基于 Agno 构建的 Agentic 系统，通过 DeepSeek R1 的推理能力与 Claude 的表达能力组成双模型架构，为复杂软件系统提供专业的软件架构分析和建议。系统可以输出详细技术分析、实施路线图以及关键架构决策。

## 功能特性

- **双 AI 模型架构**
  - **DeepSeek Reasoner**：负责对架构模式、工具选型和实施策略进行初步技术分析与结构化推理
  - **Claude-3.5**：基于 DeepSeek 的分析结果生成详细说明、实施路线图和技术规格

- **完整分析维度**
  - 架构模式选择
  - 基础设施资源规划
  - 安全措施与合规要求
  - 数据库架构
  - 性能需求
  - 成本估算
  - 风险评估

- **支持的分析类型**
  - 实时事件处理系统
  - 医疗数据平台
  - 金融交易平台
  - 多租户 SaaS 解决方案
  - 数字内容分发网络
  - 供应链管理系统

## 运行方式

1. **配置环境**

```bash
# 克隆仓库
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_system_architect_r1

# 安装依赖
pip install -r requirements.txt
```

2. **配置 API Keys**
   - 从 DeepSeek 平台获取 DeepSeek API Key
   - 从 [Anthropic Platform](https://www.anthropic.com) 获取 Anthropic API Key

3. **运行应用**

```bash
streamlit run ai_system_architect_r1.py
```

4. **使用界面**
   - 在侧边栏输入 API 凭据
   - 建议按照以下结构组织提示词：
     - 项目背景（Project Context）
     - 需求（Requirements）
     - 约束（Constraints）
     - 规模（Scale）
     - 安全/合规要求（Security/Compliance）
   - 查看详细架构分析结果

## 示例测试提示词

### 1. 金融交易平台

"我们需要构建一个高频交易平台，用于处理市场数据流，以亚毫秒级延迟执行交易，维护完整审计轨迹，并处理复杂风险计算。系统需要全球分布式部署，每秒处理 100,000 笔交易，并具备可靠的灾难恢复能力。"

### 2. 多租户 SaaS 平台

"设计一个企业资源规划类多租户 SaaS 平台，需要支持每个租户独立定制，满足不同的数据驻留要求，支持离线能力，并在不同租户之间保持性能隔离。系统应能扩展到 10,000 个并发用户，并支持自定义集成。"

## 说明

- 同时需要 DeepSeek 和 Anthropic API Key
- 提供带详细解释的实时分析
- 支持聊天式交互
- 所有架构决策都会给出明确的推理依据
- 使用相关 API 会产生对应费用
