# ♜ Agent White vs Agent Black：AI 国际象棋对战

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-multi-agent-chess-game)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个高级 AI 国际象棋系统：两个 AI Agent 在 Streamlit 应用中基于 Autogen 相互对弈。系统包含可靠的走法验证和棋局状态管理机制。

## 功能特性

### 多 Agent 架构
- 白方玩家：由 OpenAI 驱动，负责战略决策
- 黑方玩家：由 OpenAI 驱动，作为战术对手
- Board Proxy：负责验证走法合法性和维护棋局状态

### 安全与验证
- 可靠的走法验证系统
- 防止非法走子
- 实时监控棋盘状态
- 安全控制棋局推进流程

### 战略对弈
- AI 驱动的局面评估
- 深度战术分析
- 动态调整策略
- 完整实现国际象棋规则

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/autonomous_game_playing_agent_apps/ai_chess_agent
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册 [OpenAI](https://platform.openai.com/) 账号（或选择其他 LLM Provider）并获取 API Key。

4. 运行 Streamlit 应用

```bash
streamlit run ai_chess_agent.py
```
