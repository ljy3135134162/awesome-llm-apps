# AI 服务型数字代理公司 👨‍💼

这是一个使用多个 AI 智能体模拟全服务数字代理公司的应用，用于分析和规划软件项目。每个智能体代表项目生命周期中的不同角色，从战略规划一直到技术实施。

## 演示

https://github.com/user-attachments/assets/a0befa3a-f4c3-400d-9790-4b9e37254405

## 功能特性

### 五个专业 AI 智能体

- **CEO 智能体**：战略负责人和最终决策者
  - 使用结构化评估分析创业想法
  - 在产品、技术、营销和财务领域做出战略决策
  - 使用 AnalyzeProjectRequirements 工具

- **CTO 智能体**：技术架构与可行性专家
  - 评估技术需求和可行性
  - 提供架构决策建议
  - 使用 CreateTechnicalSpecification 工具

- **产品经理智能体**：产品战略专家
  - 定义产品战略和路线图
  - 协调技术团队与营销团队
  - 聚焦产品市场匹配（Product-Market Fit）

- **开发者智能体**：技术实施专家
  - 提供详细的技术实现指导
  - 推荐合适的技术栈和云解决方案
  - 估算开发成本和时间周期

- **客户成功智能体**：营销战略负责人
  - 制定 Go-to-Market（GTM）策略
  - 规划客户获取方案
  - 与产品团队协同工作

### 自定义工具

该代理公司使用基于 OpenAI Schema 构建的专用工具进行结构化分析：
- **分析工具**：AnalyzeProjectRequirements，用于市场评估和创业想法分析
- **技术工具**：CreateTechnicalSpecification，用于技术评估

### 🤝 多智能体协作

系统通过明确的沟通流程协调五名专业智能体：
- CEO 负责团队整体战略监督
- CTO 与开发者协同评估实施可行性
- 产品经理与客户成功负责人协同规划产品路线图和 GTM 策略
- 每项分析都会在 Streamlit UI 中以独立区块展示

### 🔗 智能体通信流程
- CEO ↔️ 所有智能体（战略监督）
- CTO ↔️ 开发者（技术实施）
- 产品经理 ↔️ 客户成功经理（GTM 策略）
- 产品经理 ↔️ 开发者（功能实施）
- 以及更多协作关系

## 运行方法

按照以下步骤配置并运行应用。
在开始之前，请先在这里获取 OpenAI API Key：https://platform.openai.com/api-keys

1. **克隆仓库**：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd advanced_ai_agents/multi_agent_apps/agent_teams/ai_services_agency
   ```

2. **安装依赖**：
    ```bash
    pip install -r requirements.txt
    ```

3. **运行 Streamlit 应用**：
    ```bash
    streamlit run agency.py
    ```

4. 按提示在侧边栏中输入你的 OpenAI API Key，然后即可开始分析你的创业项目构想。
