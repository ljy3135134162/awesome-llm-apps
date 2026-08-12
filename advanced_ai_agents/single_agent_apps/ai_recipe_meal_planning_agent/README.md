# 🍽️ AI 食谱与膳食规划 Agent

这是一个基于 Agno 构建的智能膳食规划 Agent，可根据你现有的食材和饮食偏好，帮助发现食谱、分析营养、估算成本，并创建每周膳食计划。

## 功能特性

🔍 **食谱发现**
- 根据现有食材查找食谱
- 支持素食、纯素、Keto、Paleo 等饮食限制
- 提供食材替代建议
- 提供详细的烹饪步骤与时间安排

📊 **营养分析**
- 提供每份食物的完整营养成分明细
- 提供更易理解的健康评估
- 追踪卡路里、蛋白质、碳水和脂肪
- 分析钠和膳食纤维含量

💰 **成本估算**
- 估算食材采购成本
- 推荐更符合预算的餐食
- 计算每份餐食的成本

📅 **每周膳食规划**
- 根据家庭人数生成均衡的膳食计划
- 适配不同饮食偏好
- 优化购物清单
- 支持预算导向的规划

🧠 **基于 Session 的对话记忆**
- 在当前浏览器 Session 中记住上下文
- 应用重启后不会持久保存偏好，不包含长期存储

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_recipe_meal_planning_agent
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册 [OpenAI](https://platform.openai.com/) 账号并获取 API Key。

4. 获取 Spoonacular API Key

- 注册 [Spoonacular](https://spoonacular.com/food-api) 账号并获取 API Key（免费额度约为每天 50 次请求，具体以当前官方政策为准）。

5. 在当前目录创建 `.env` 文件

```bash
# 必需
OPENAI_API_KEY=your_openai_api_key_here

# 可选，但推荐配置，以启用完整的食谱和营养功能
SPOONACULAR_API_KEY=your_spoonacular_api_key_here
```

6. 运行 Streamlit 应用

```bash
streamlit run ai_recipe_meal_planning_agent.py
```

7. 在浏览器中打开 `http://localhost:8501`

## 示例交互

**食谱发现：**
- “我有鸡肉、西兰花和米饭，可以做什么？”
- “帮我找使用扁豆的纯素食谱”
- “给我一些 30 分钟内能完成的晚餐方案”

**营养分析：**
- “这道食谱的营养成分是多少？”
- “这顿饭的蛋白质含量高吗？”
- “每份有多少卡路里？”

**膳食规划：**
- “为 2 个人制定一周的素食餐单”
- “我需要一份低钠膳食计划”
- “为四口之家安排一周的低预算餐食”

**成本估算：**
- “这些食材大概需要多少钱？”
- “哪个方案最省预算？”
- “估算这份膳食计划一周的采购费用”

## 应用架构

### 基于 Agno Framework 构建
- **Agent**：由 OpenAI GPT-5 mini 驱动的膳食规划 Agent
- **Memory**：使用对话记忆生成更个性化的建议
- **Tools**：自定义食谱搜索与分析工具，加上 DuckDuckGo Web Search
- **Interface**：Streamlit Web 应用

### 自定义工具
1. `search_recipes(ingredients, diet_type=None)` - 通过 Spoonacular API 搜索食谱并返回详细步骤
2. `analyze_nutrition(recipe_name)` - 通过 Spoonacular 提供详细营养分析
3. `estimate_costs(ingredients, servings=4)` - 用于预算规划和成本估算
4. `create_meal_plan(dietary_preference="balanced", people=2, days=7, budget="moderate")` - 生成包含购物清单的完整周度膳食计划
5. `DuckDuckGoTools` - 用于获取补充上下文的 Web 搜索工具

### 核心技术
- **Agno**：AI Agent 框架
- **Streamlit**：Web UI 与用户交互
- **Spoonacular API**：食谱与营养数据
- **OpenAI GPT-5 mini**：自然语言理解与生成

## 自定义

### 添加新的饮食偏好
修改 `search_recipes` 工具，加入 Spoonacular API 支持的其他饮食类型。

### 扩展成本数据库
更新 `estimate_grocery_costs()` 中的 `ingredient_costs` 字典，替换为本地价格数据。

### 自定义餐食类别
编辑 `create_weekly_meal_plan()` 中的 `meal_categories`，使其符合你的实际偏好。

## 故障排查

**API Key 问题：**
- 确认 `.env` 文件位于正确目录
- 检查 API Key 是否有效并且额度充足
- 检查 API Key 格式，避免多余空格或引号
- 如果未配置 `SPOONACULAR_API_KEY`，食谱搜索和营养工具会返回错误，但其他功能仍可加载

**食谱搜索无法工作：**
- 确认 Spoonacular API Key 配置正确
- 检查 API 调用额度
- 尝试使用更简单的食材搜索条件

**记忆相关问题：**
- Agent 使用对话记忆保存当前 Session 中的偏好
- 如果出现持续异常，可清理浏览器缓存
- 重启应用即可重置对话历史

## 贡献

欢迎通过以下方式参与贡献：
- 添加新的食谱数据源或 API
- 改进营养分析算法
- 提高成本估算准确度
- 增加新的膳食规划功能

## License

本项目为开源项目，具体许可证信息请查看主仓库。

## 支持

遇到问题时可以：
- 查看上方故障排查章节
- 阅读 Agno 官方文档
- 在主仓库提交 Issue
