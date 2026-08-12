# 👨‍🏫 AI 教学智能体团队

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-an-ai-teaching-agent-team)，通过详细代码讲解、说明和最佳实践，从零开始构建本项目。**

这是一个 Streamlit 应用，将多个专业化的 AI 教学智能体组合成一支类似专业教师团队的协作系统。每个智能体承担不同的教育角色，包括课程设计、学习路径规划、资源整理和练习指导，并通过 Google Docs 协同生成完整的学习体验。

## 🪄 认识你的 AI 教学智能体团队

#### 🧠 教授智能体
- 在 Google Docs 中创建基础知识库
- 使用清晰的标题与章节组织内容
- 提供详细解释和示例
- 输出：带目录的完整知识库文档

#### 🗺️ 学术顾问智能体
- 在结构化 Google Doc 中设计学习路径
- 创建渐进式学习里程碑
- 包含时间预估和前置知识要求
- 输出：具有清晰学习进阶路径的可视化路线图文档

#### 📚 研究资料管理员智能体
- 在 Google Doc 中整理学习资源
- 包含学术论文和教程链接
- 添加资源说明和难度等级
- 输出：按类别整理并带质量评级的资源清单

#### ✍️ 助教智能体
- 在交互式 Google Doc 中设计练习
- 创建结构化练习章节
- 提供解答指南
- 输出：包含答案的完整练习手册

## 运行方法

1. 克隆仓库
  ```bash
   # 克隆仓库
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd advanced_ai_agents/multi_agent_apps/agent_teams/ai_teaching_agent_team

   # 安装依赖
   pip install -r requirements.txt
   ```

## 配置——重要步骤

1. 获取 OpenAI API Key
- 在 [OpenAI Platform](https://platform.openai.com/) 注册账户
- 进入 API Keys 页面
- 创建新的 API Key

2. 获取 Composio API Key
- 在 [Composio Platform](https://composio.ai/) 注册账户
- **重要**：要正常使用本应用，需要为 Google Docs 与 Composio 创建新的连接 ID。请完成以下步骤：
  - 在终端运行 `composio add googledocs`
  - 创建一个新的连接
  - 选择 `OAUTH2`
  - 选择 Google Account 并完成授权
  - 登录 Composio 网站，进入 Apps，选择 Google Docs 工具，然后[点击创建集成](https://app.composio.dev/app/googledocs)（紫色按钮），再点击默认 Google Docs 的连接按钮完成配置

3. 注册并获取 [SerpAPI Key](https://serpapi.com/)

## 使用方法

1. 启动 Streamlit 应用
```bash
streamlit run teaching_agent_team.py
```

2. 使用应用
- 在侧边栏输入 OpenAI API Key（如果尚未设置为环境变量）
- 在侧边栏输入 Composio API Key
- 输入你想学习的主题，例如“Python 编程”或“机器学习”
- 点击“Generate Learning Plan（生成学习计划）”
- 等待各个智能体生成个性化学习计划
- 在界面中查看结果和终端输出
