# AI 健康与健身规划 Agent 🏋️‍♂️

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-personal-health-and-fitness-ai-agent-using-google-gemini)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

**AI Health & Fitness Planner** 是一个基于 Agno AI Agent 框架构建的个性化健康与健身 Agent。应用会根据用户的年龄、体重、身高、活动水平、饮食偏好和健身目标，生成定制化的饮食与训练计划。

## 功能特性

- **Health Agent 与 Fitness Agent**
  - 应用包含两个专门的 Agent，分别负责饮食建议和健身/训练建议。

- **个性化饮食计划**
  - 生成详细的每日餐单，包括早餐、午餐、晚餐和加餐。
  - 包含补水、电解质和膳食纤维等重要注意事项。
  - 支持 Keto、Vegetarian、Low Carb 等多种饮食偏好。

- **个性化健身计划**
  - 根据健身目标生成定制化训练方案。
  - 覆盖热身、主要训练和放松环节。
  - 提供可执行的健身建议以及进度跟踪建议。

- **交互式问答**
  - 用户可以针对生成的计划继续提出追问。

## 环境要求

应用需要以下 Python 库：

- `agno`
- `google-generativeai`
- `streamlit`

请根据 `requirements.txt` 中指定的版本安装这些依赖。

## 如何运行

按照以下步骤配置并启动应用。

开始之前，请先在 Google AI Studio 获取免费的 Gemini API Key：
https://aistudio.google.com/apikey

1. **克隆仓库**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_health_fitness_agent
```

2. **安装依赖**

```bash
pip install -r requirements.txt
```

3. **运行 Streamlit 应用**

```bash
streamlit run health_agent.py
```
