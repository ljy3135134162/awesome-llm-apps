# 🎮 Agent X vs Agent O：AI 井字棋对战

这是一个交互式井字棋游戏，由两个使用不同语言模型的 AI Agent 相互对战。项目基于 Agno Agent Framework 构建，并使用 Streamlit 作为 UI。

该示例展示如何构建 AI Agent 相互竞争的交互式井字棋应用，包括：
- 在回合制游戏中协调多个 AI Agent
- 为不同玩家使用不同语言模型
- 使用 Streamlit 创建交互式 Web 界面
- 管理游戏状态并验证走法
- 实时显示游戏进度和走法历史

## 功能特性

- 支持多种 AI 模型（GPT-4、Claude、Gemini 等）
- 实时棋盘可视化
- 记录走法历史及对应棋盘状态
- 可交互选择玩家模型
- 游戏状态管理
- 走法验证与 Agent 协调

## 如何运行？

1. **配置环境**

```bash
# 克隆仓库
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/autonomous_game_playing_agent_apps/ai_tic_tac_toe_agent

# 安装依赖
pip install -r requirements.txt
```

### 2. 安装依赖

```shell
pip install -r requirements.txt
```

### 3. 配置 API Keys

游戏支持多种 AI 模型。在当前目录创建 `.env` 文件，并加入所需 API Key：

1. **创建 `.env` 文件：**

```bash
# 在 ai_tic_tac_toe_agent 目录中
touch .env
```

2. **将 API Keys 添加到 `.env` 文件：**

```env
# OpenAI 模型（gpt-4o、o3-mini）需要
OPENAI_API_KEY=your_actual_openai_api_key_here

# 可选 —— 用于其他模型
ANTHROPIC_API_KEY=your_actual_anthropic_api_key_here  # Claude 模型
GOOGLE_API_KEY=your_actual_google_api_key_here        # Gemini 模型
GROQ_API_KEY=your_actual_groq_api_key_here             # Groq 模型
```

> **注意：** 请将示例值替换为真实 API Key。如果缺少必要的 Key，应用会显示相应错误提示。

### 4. 运行游戏

```shell
streamlit run app.py
```

- 打开 [localhost:8501](http://localhost:8501) 查看游戏界面

## 工作原理

游戏由三个 Agent 组成：

1. **Master Agent（裁判）**
   - 协调整个游戏流程
   - 验证走法
   - 维护游戏状态
   - 判断游戏结果

2. **两个 Player Agents**
   - 进行策略性走子
   - 分析棋盘状态
   - 遵守游戏规则
   - 根据对手走法作出响应

## 可用模型

游戏支持多种 AI 模型：
- GPT-4o（OpenAI）
- GPT-o3-mini（OpenAI）
- Gemini（Google）
- Llama 3（Groq）
- Claude（Anthropic）

## 游戏功能

1. **交互式棋盘**
   - 实时更新
   - 可视化走法记录
   - 清晰显示游戏状态

2. **走法历史**
   - 详细记录每一步
   - 展示对应棋盘状态
   - 玩家操作时间线

3. **游戏控制**
   - 开始/暂停游戏
   - 重置棋盘
   - 选择 AI 模型
   - 查看游戏历史

4. **性能分析**
   - 走子耗时
   - 策略追踪
   - 游戏统计
