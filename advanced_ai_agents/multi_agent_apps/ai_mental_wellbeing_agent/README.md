# AI 心理健康 Agent 团队 🧠

AI Mental Wellbeing Agent Team 是一个提供支持性心理健康评估与指导的系统，基于 [AG2](https://github.com/ag2ai/ag2?tab=readme-ov-file)（原 AutoGen）的 AI Agent 框架构建。该应用通过多个专业 Agent 的协同，为用户提供个性化心理健康支持。各 Agent 会根据用户输入的情绪状态、压力水平、睡眠情况和当前症状等信息，分别负责不同方面的分析与建议。

本项目使用 AG2 新增的 Swarm 功能，并通过 `initiate_swarm_chat()` 方法运行。

## 功能特性

- **专业心理健康支持团队**
  - 🧠 **Assessment Agent（评估 Agent）**：以较严谨且富有同理心的方式分析用户情绪状态与心理需求
  - 🎯 **Action Agent（行动 Agent）**：制定即时行动计划，并帮助用户找到适合的支持资源
  - 🔄 **Follow-up Agent（跟进 Agent）**：设计长期支持策略和预防方案

- **全面的心理健康支持**
  - 详细心理状态评估
  - 即时应对策略
  - 支持资源推荐
  - 长期支持规划
  - 危机预防策略
  - 进展监测机制

- **可自定义输入参数**
  - 当前情绪状态
  - 睡眠情况
  - 压力水平
  - 社会支持系统信息
  - 近期生活变化
  - 当前症状

- **交互式结果展示**
  - 实时评估摘要
  - 可展开查看的详细建议
  - 清晰的行动步骤与资源
  - 长期支持策略

## 如何运行

按照以下步骤配置并启动应用：

1. **克隆仓库**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/multi_agent_apps/ai_mental_wellbeing_agent
```

2. **安装依赖**

```bash
pip install -r requirements.txt
```

3. **创建环境配置文件**

在项目目录中创建 `.env` 文件：

```bash
echo "AUTOGEN_USE_DOCKER=0" > .env
```

该配置用于关闭 AutoGen 代码执行时对 Docker 的依赖。

4. **配置 OpenAI API Key**

- 从 [OpenAI Platform](https://platform.openai.com) 获取 OpenAI API Key
- 启动应用后，在侧边栏中输入该 API Key

5. **运行 Streamlit 应用**

```bash
streamlit run ai_mental_wellbeing_agent.py
```

## ⚠️ 重要说明

本应用仅作为辅助支持工具，不能替代专业心理健康服务。如果用户出现自伤念头或处于严重心理危机中，应立即寻求专业帮助。

原项目提供的是美国地区的紧急求助信息：

- 美国 988 Suicide & Crisis Lifeline：拨打 `988`
- 美国紧急服务：拨打 `911`
- 或立即联系当地专业医疗与心理健康机构
