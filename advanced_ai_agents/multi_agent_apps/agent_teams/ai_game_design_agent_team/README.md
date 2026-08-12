# AI 游戏设计智能体团队 🎮

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-an-ai-game-design-agent-team)，通过详细的代码讲解、说明和最佳实践，从零开始构建本项目。**

AI 游戏设计智能体团队是一套由 [AG2](https://github.com/ag2ai/ag2?tab=readme-ov-file)（原 AutoGen）AI Agent 框架驱动的协作式游戏设计系统。该应用通过多个专业 AI 智能体协同工作，根据用户提供的游戏类型、目标受众、美术风格和技术要求等信息，生成完整的游戏概念方案。项目基于 AG2 的新 Swarm 能力，并通过 `initiate_chat()` 方法运行。

## 功能特性

- **专业化游戏设计智能体团队**

  - 🎭 **剧情智能体**：专注叙事设计与世界观构建，包括角色塑造、剧情弧线、对白编写和背景设定
  - 🎮 **玩法智能体**：专注游戏机制和系统设计，包括玩家成长、战斗系统、资源管理和平衡性
  - 🎨 **视觉智能体**：负责美术方向和音频设计，包括 UI/UX、角色与环境美术风格、音效和音乐设计
  - ⚙️ **技术智能体**：提供技术架构和实现建议，包括引擎选择、性能优化、网络需求和开发路线图
  - 🎯 **任务智能体**：协调所有专业智能体，并确保不同游戏设计要素能够一致整合

- **完整的游戏设计输出**：

  - 详细的叙事和世界观设定
  - 核心玩法机制与系统
  - 视觉与音频方向
  - 技术规格和需求
  - 开发时间线和预算考量
  - 由团队协同生成的一致性游戏设计方案

- **可自定义输入参数**：

  - 游戏类型和目标受众
  - 美术风格与视觉偏好
  - 平台要求
  - 开发约束（时间、预算）
  - 核心机制与玩法功能

- **交互式结果展示**：
  - 快速查看各个智能体提出的游戏设计思路
  - 详细结果通过可展开区域展示，方便浏览和查阅

## 运行方法

按照以下步骤配置并运行应用：

1. **克隆仓库**：

   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd advanced_ai_agents/multi_agent_apps/agent_teams/ai_game_design_agent_team
   ```

2. **安装依赖**：

   ```bash
   pip install -r requirements.txt
   ```

3. **配置 OpenAI API Key**：

   - 从 [OpenAI 平台](https://platform.openai.com) 获取 OpenAI API 密钥
   - 运行应用后，在侧边栏中输入该密钥

4. **运行 Streamlit 应用**：
   ```bash
   streamlit run game_design_agent_team.py
   ```

## 使用方法

1. 在侧边栏中输入 OpenAI API Key
2. 填写游戏相关信息：
   - 背景氛围与世界设定
   - 游戏类型与目标受众
   - 视觉风格偏好
   - 技术要求
   - 开发限制条件
3. 点击“Generate Game Concept（生成游戏概念）”，获取所有智能体协同生成的完整设计文档
4. 在各个可展开区域中查看游戏设计不同方面的详细输出
