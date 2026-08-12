## 🌍 MCP 旅行规划 Agent 团队

这是一个基于 Streamlit 构建的高级 AI 旅行规划应用，通过多个 MCP Server 与 Google Maps 集成，生成极其详细且高度个性化的旅行行程。应用使用 Airbnb MCP 获取真实住宿数据，并通过自定义 Google Maps MCP 完成精确距离计算和位置服务。

## ✨ 功能特性

### 🤖 AI 驱动的旅行规划
- **超详细行程**：生成完整的逐日旅行计划，包括具体时间、地址和费用
- **距离计算**：使用 Google Maps MCP 计算行程中各地点之间的精确距离和通行时间
- **实时住宿数据**：集成 Airbnb MCP 获取当前价格与可订状态
- **个性化推荐**：根据用户偏好和预算限制定制行程

### 🏨 Airbnb MCP 集成
- 提供包含当前价格和可订状态的**真实住宿房源**
- 提供包括设施、住客评价和预订状态在内的**房源详情**
- 根据地点和偏好筛选**符合预算的住宿推荐**
- 提供包含实时价格的**直接预订信息**

### 🗺️ Google Maps MCP 集成
- 计算行程中所有地点之间的**精确距离**
- 提供用于交通规划的**通行时间估算**
- 为兴趣点和导航提供**位置服务**
- 对所有推荐地点进行**地址验证**
- 通过逐向导航信息进行**交通方案优化**

### 🔍 Google 搜索集成
- 提供**当前天气预报**和详细穿衣建议
- 提供餐厅调研，包括具体地址、价格区间和评价
- 提供景点详情，包括开放时间、票价和最佳游览时间
- 提供当地信息和文化背景
- 提供实用旅行建议，包括货币兑换和安全信息

### 📅 其他功能
- **日历导出**：可将行程下载为 `.ics` 文件，并导入 Google Calendar、Apple Calendar 或 Outlook
- **完整费用明细**：对旅行各项组成部分进行详细预算分析
- **缓冲时间规划**：在行程安排中纳入通勤时间与意外延误
- **多住宿方案**：提供 3 个住宿选项，并标注与市中心的距离

## 配置

### 环境要求

1. **API Key**（两者都必需）：
   - **OpenAI API Key**：从 [OpenAI Platform](https://platform.openai.com/api-keys) 获取
   - **Google Maps API Key**：从 [Google Cloud Console](https://console.cloud.google.com/apis/credentials) 获取

2. **Python 3.8+**：确保已安装 Python 3.8 或更高版本。

3. **MCP Server**：应用会自动连接：
   - **Airbnb MCP Server**：提供真实 Airbnb 房源和价格数据
   - **自定义 Google Maps MCP**：提供精确距离计算和位置服务

### 安装

1. 克隆仓库：

   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd awesome-llm-apps/mcp_ai_agents/ai_travel_planner_mcp_agent_team
   ```

2. 安装所需 Python 依赖：

   ```bash
   pip install -r requirements.txt
   ```

### 运行应用

1. 启动 Streamlit 应用：

   ```bash
   streamlit run app.py
   ```

2. 在应用界面中：
   - 在侧边栏输入 **OpenAI API Key**
   - 在侧边栏输入 **Google Maps API Key**
   - 设置目的地、旅行天数、预算和偏好
   - 点击“🎯 Generate Itinerary”生成详细旅行计划

3. **可选**：将行程下载为日历文件（`.ics`），并导入 Google Calendar、Apple Calendar 或 Outlook。

## 故障排查

### 常见问题与解决方案

- **“Error: [error message]”**：检查网络连接和 API Key
  - 确认 OpenAI 与 Google Maps API Key 均输入正确
  - 几分钟后重试，MCP Server 可能暂时不可用

- **缺少距离信息**：通常是 Google Maps MCP 连接问题
  - 检查 Google Maps API Key 是否有效
  - 确保 API Key 已获得 Maps API 所需权限
  - 尝试刷新页面并重新输入 Key

- **响应较慢**：MCP Server 可能需要较长时间响应
  - 应用已配置 60 秒超时
  - 详细行程生成本身需要一定处理时间

- **网络/防火墙问题**：部分企业网络可能阻止 MCP 连接
  - 尝试切换到其他网络
  - 必要时使用 VPN
  - 如果 MCP Server 无法访问，应用会显示连接错误

### API Key 问题

- **OpenAI API Key**：确保 OpenAI 账户中有可用额度，并且 Key 有效
- **Google Maps API Key**：确保已启用 Maps API，并正确配置计费

### 工具状态

应用会显示实际成功使用了哪些数据源：

- 🏨 **“Your travel itinerary is ready with Airbnb data!”** = Airbnb MCP 连接成功
- 📝 **“Used general knowledge for accommodation suggestions”** = Airbnb MCP 连接失败，已回退到通用知识进行住宿推荐

**应用被设计为即使部分 MCP 连接失败也可以继续工作。** 在这种情况下，它仍会使用可用工具和信息生成完整行程。

## 项目结构

```text
├── app.py              # 集成 MCP 的主 Streamlit 应用
├── requirements.txt    # Python 依赖
└── README.md           # 本文档
```

## 工作原理

AI Travel Planner Agent Team 通过一个多步骤流程生成极其详细的旅行行程：

### 🤖 AI Agent 架构
- **GPT-4o 模型**：利用高级推理能力完成智能旅行规划
- **多 MCP 集成**：组合 Airbnb 和 Google Maps MCP Server 获取实时数据
- **Google 搜索工具**：提供当前天气、评价和当地信息
- **直接生成响应**：无需反复追问澄清问题，直接生成完整行程
