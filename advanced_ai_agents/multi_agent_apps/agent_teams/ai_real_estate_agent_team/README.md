# 🏠 AI 房地产智能体团队

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-real-estate-agent-team)，通过详细代码讲解、说明和最佳实践，从零开始构建本项目。**

**AI 房地产智能体团队**是一个由多个专业 AI 智能体协作驱动的房产搜索与分析平台，并使用 Firecrawl 的 Extract 接口获取结构化数据。该应用结合高级网页抓取和 AI 搜索能力，为用户提供全面的房地产洞察、市场分析和房产推荐。

## 功能特性

- **多智能体分析系统**
    - **房产搜索智能体**：通过 Firecrawl 直接集成查找房源
    - **市场分析智能体**：提供精炼的市场趋势与社区洞察
    - **房产估值智能体**：提供房产估值与投资分析

- **多平台房源搜索**：
  - **Zillow**：大型房地产平台，拥有丰富房源
  - **Realtor.com**：美国全国房地产经纪人协会官方平台
  - **Trulia**：侧重社区信息的房地产搜索平台
  - **Homes.com**：综合房产搜索平台

- **高级房产分析**：
  - 提取详细房产信息（地址、价格、卧室数、浴室数、面积）
  - 分析房产特征和配套设施
  - 提供房源链接和经纪人联系方式
  - 提供可点击的房源链接，便于直接访问

- **综合市场洞察**：
  - 当前市场状态（买方市场 / 卖方市场）
  - 价格趋势及市场方向
  - 社区分析与关键洞察
  - 投资潜力评估
  - 战略建议

- **顺序式手动执行**：
  - 针对速度和稳定性进行优化
  - 智能体之间直接传递数据
  - 手动协调以获得更好的控制能力
  - 降低额外开销并提升性能

- **交互式界面功能**：
  - 实时跟踪智能体执行进度
  - 为每个搜索阶段显示进度指示
  - 支持下载分析报告
  - 显示执行耗时，便于性能监控

## 环境要求

应用需要以下 Python 库：

- `agno`
- `streamlit`
- `firecrawl-py`
- `python-dotenv`
- `pydantic`

同时需要以下 API 密钥：
- **云端版本**：Google AI（Gemini）+ Firecrawl
- **本地版本**：仅需 Firecrawl（AI 模型通过本地 Ollama 运行）

## 运行方法

按照以下步骤配置并运行应用：

### **API 版本（Gemini 2.5 Flash）**

1. **克隆仓库**：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd advanced_ai_agents/multi_agent_apps/agent_teams/ai_real_estate_agent_team
   ```

2. **安装依赖**：
    ```bash
    pip install -r requirements.txt
    ```

3. **配置 API 密钥**：
    - 获取 Google AI API 密钥：https://aistudio.google.com/app/apikey
    - 获取 Firecrawl API 密钥：[Firecrawl 网站](https://firecrawl.dev)

4. **运行 Streamlit 应用**：
    ```bash
    streamlit run ai_real_estate_agent_team.py
    ```

### **本地版本（Ollama）**

1. **安装 Ollama 并拉取模型**：
   ```bash
   # 拉取模型：建议设备拥有超过 16GB 内存，以便在本地运行该模型
   ollama pull gpt-oss:20b
   ```

2. **安装依赖**：
    ```bash
    pip install -r requirements.txt
    ```

3. **配置 API 密钥**：
    - 获取 Firecrawl API 密钥：[Firecrawl 网站](https://firecrawl.dev)

4. **运行本地 Streamlit 应用**：
    ```bash
    streamlit run local_ai_real_estate_agent_team.py
    ```

## 使用方法

### **云端版本**

1. 在侧边栏中输入 API 密钥：
   - Google AI API Key
   - Firecrawl API Key

2. 选择要搜索的房地产网站：
   - Zillow
   - Realtor.com
   - Trulia
   - Homes.com

3. 配置房产需求：
   - 地点（城市、州）
   - 预算范围
   - 房产详情（类型、卧室数、浴室数、面积）
   - 特殊需求和时间要求

4. 点击 `Start Property Analysis` 开始分析，并生成：
   - 带详细信息的房源列表
   - 市场分析与趋势
   - 房产估值与推荐建议

### **本地版本**

1. 在侧边栏中输入 Firecrawl API Key
2. 确保 Ollama 正在运行，并已加载 `gpt-oss:20b` 模型
3. 按照云端版本相同的步骤配置房产条件
4. 使用本地 AI 推理获得同样完整的分析结果

## 智能体工作流

### **房产搜索智能体**
- 直接使用 Firecrawl 搜索房地产网站
- 重点寻找符合用户条件的房产
- 提取包含完整详情的结构化房产数据
- 整理结果并提供可点击的房源 URL

### **市场分析智能体**
- **市场状况**：买方市场 / 卖方市场、价格趋势
- **重点社区**：简要介绍房产所在区域
- **投资前景**：提供 2-3 个关键投资判断
- **格式**：每个部分使用精炼要点，控制在 100 词以内

### **房产估值智能体**
- **价值评估**：判断合理价格，以及是否高估或低估
- **投资潜力**：高 / 中 / 低，并提供简要理由
- **关键建议**：针对每套房产给出一条可执行建议
- **格式**：每套房产的评估控制在 50 词以内

## 技术架构

### **数据来源**：
- **Firecrawl Extract API**：提取结构化房产数据
- **Pydantic Schema**：用于结构化数据校验和格式化

### **AI 框架**：
- **云端版本**：Agno Framework + Google Gemini 2.5 Flash
- **本地版本**：Agno Framework + Ollama gpt-oss:20b
- **Streamlit**：交互式 Web 应用界面

### **性能特性**：
- **顺序执行**：采用手动协调方式优化性能
- **进度跟踪**：实时更新分析状态
- **错误恢复**：对数据提取失败进行平滑处理
- **直接集成**：绕过额外工具封装，以提升执行速度

## 文件结构

```text
ai_real_estate_agent_team/
├── ai_real_estate_agent_team.py        # API 版本（Google Gemini）
├── local_ai_real_estate_agent_team.py  # 本地版本（Ollama）
├── requirements.txt                    # Python 依赖
├── README.md                           # 本说明文档
└── .env                                # 环境变量文件（需要自行创建）
```

## API 要求

### **云端版本**

#### **Google AI API**
- **模型**：Gemini 2.5 Flash
- **用途**：多智能体分析与房产洞察
- **速率限制**：遵循 Google AI 标准速率限制

#### **Firecrawl API**
- **接口**：Extract API，用于结构化数据提取
- **用途**：从房地产网站提取房源信息
- **速率限制**：遵循 Firecrawl 标准速率限制

### **本地版本**

#### **Firecrawl API**
- **接口**：Extract API，用于结构化数据提取
- **用途**：从房地产网站提取房源信息
- **速率限制**：遵循 Firecrawl 标准速率限制

#### **Ollama（本地）**
- **模型**：gpt-oss:20b
- **用途**：在本地完成全部 AI 推理处理
- **硬件要求**：建议约 16GB 或更多内存
- **无需 API 成本**：AI 推理完全在本地运行
