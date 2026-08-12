# 🎮 AI 谈判对战模拟器

### 基于 AG-UI 的实时 Agent 对 Agent 谈判对决

观看两个 AI Agent 在二手车谈判中正面交锋。本项目使用 **Google ADK** 构建后端 Agent，并结合 **AG-UI + CopilotKit** 实现实时响应式前端。

## ✨ 功能特性

- **🤖 双 AI Agent 对战**：买方与卖方拥有不同性格和谈判策略
- **🔄 AG-UI 协议**：实时流式展示 Agent 行为、工具调用和状态变化
- **💅 动态对战界面**：包含动画效果和实时谈判时间线
- **🎭 8 种独特人格**：4 个买方角色 + 4 个卖方角色，各自具备不同谈判风格
- **📊 Generative UI**：自定义 React 组件实时渲染工具调用结果
- **🔗 共享状态**：Agent 状态与前端进行双向同步

## 🏗️ 架构

```text
┌─────────────────────────────────────────────────────────────────┐
│                    Next.js + CopilotKit 前端                    │
│   ┌─────────────┐    ┌──────────────┐    ┌─────────────┐        │
│   │  对战时间线  │    │   VS 展示区   │    │ 聊天侧边栏  │        │
│   │ Battle Arena│    │ 买方 / 卖方   │    │  (AG-UI)    │        │
│   └──────┬──────┘    └──────────────┘    └──────┬──────┘        │
└──────────┼────────────────────────────────────────┼─────────────┘
           │              AG-UI Events              │
           └────────────────────┬───────────────────┘
                                │
                    ┌───────────▼───────────┐
                    │   CopilotKit Runtime  │
                    │   (/api/copilotkit)   │
                    └───────────┬───────────┘
                                │ HTTP/SSE
                    ┌───────────▼───────────┐
                    │    FastAPI + AG-UI    │
                    │    ADK Middleware     │
                    └───────────┬───────────┘
                                │
                    ┌───────────▼─────────────┐
                    │  ADK Negotiation Agent  │
                    │    （Battle Master）    │
                    │                         │
                    │  工具：                 │
                    │  • configure_negotiation│
                    │  • start_negotiation    │
                    │  • buyer_make_offer     │
                    │  • seller_respond       │
                    └─────────────────────────┘
```

## 🚀 快速开始

### 前置条件

- Python 3.11+
- Node.js 18+
- Google AI API Key（可从 [Google AI Studio](https://aistudio.google.com/) 获取）

### 1. 克隆并进入项目目录

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/multi_agent_apps/ai_negotiation_battle_simulator
```

### 2. 配置后端

```bash
cd backend
pip install -r requirements.txt

# 创建 .env 文件
echo "GOOGLE_API_KEY=your_api_key_here" > .env

# 启动后端
python agent.py
```

后端默认运行在：`http://localhost:8000`

### 3. 配置前端

```bash
cd frontend
npm install

# 启动前端
npm run dev
```

前端默认运行在：`http://localhost:3000`

### 4. 开始谈判

打开 `http://localhost:3000`，可以向 Battle Master 输入例如：

- `Start a negotiation for a used car`
- `Show me available scenarios`
- `Use Desperate Dan as buyer and Shark Steve as seller`

## 🎭 角色人格

### 买方

| 角色 | 表情 | 风格 |
|---|---|---|
| Desperate Dan | 😰 | 今天就必须买到车，心理活动容易暴露 |
| Analytical Alex | 🧮 | 重视所有数据点，极度理性 |
| Cool-Hand Casey | 😎 | 擅长用“离场”作为谈判筹码 |
| Fair-Deal Fran | 🤝 | 追求双方都能接受的双赢结果 |

### 卖方

| 角色 | 表情 | 风格 |
|---|---|---|
| Shark Steve | 🦈 | 每轮降价绝不超过 5% |
| By-The-Book Beth | 📊 | 严格按照 KBB 等参考价格谈判 |
| Motivated Mike | 😅 | 很想尽快卖掉车辆 |
| Drama Queen Diana | 🎭 | 几乎每次报价都称为“最终报价” |

## 📁 项目结构

```text
ai_negotiation_battle_simulator/
├── README.md
├── .env.example
│
├── backend/                    # Python ADK + AG-UI
│   ├── agent.py               # 主 Agent 与工具
│   ├── requirements.txt
│   ├── config/
│   │   ├── personalities.py   # 8 种人格
│   │   └── scenarios.py       # 3 个谈判场景
│   └── agents/
│       ├── buyer_agent.py
│       ├── seller_agent.py
│       └── orchestrator.py
│
└── frontend/                   # Next.js + CopilotKit
    ├── package.json
    ├── src/
    │   └── app/
    │       ├── layout.tsx     # CopilotKit Provider
    │       ├── page.tsx       # Battle Arena UI
    │       ├── globals.css    # 对战动画
    │       └── api/
    │           └── copilotkit/
    │               └── route.ts  # CopilotKit Runtime
    └── tailwind.config.js
```

## 🎬 示例对战

```text
🔔 谈判开始：2019 Honda Civic EX

📋 要价：$15,500

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

😎 COOL-HAND CASEY（第 1 轮）：
“我看到类似 Civic 的成交价更低。按照当前市场，$11,500 比较合理。
我今天可以直接付现金。”

🦈 SHARK STEVE（第 1 轮）：
“$15,000。这辆车车况非常好，这周末还有另外两个买家要来看。”

😎 COOL-HAND CASEY（第 2 轮）：
“$12,500 是我的上限。不接受我就走。”

🦈 SHARK STEVE（第 2 轮）：
*考虑片刻* “$14,000。最终报价。”

😎 COOL-HAND CASEY（第 3 轮）：
“$13,000，我们各退一步。”

🦈 SHARK STEVE（第 3 轮）：
“……$13,500，就成交。”

😎 COOL-HAND CASEY（第 4 轮）：
“$13,250。最终价格。”

🦈 SHARK STEVE（第 4 轮）：
“成交。🤝”

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ 最终成交价：$13,250 🎉
   买方节省：$2,250（比原要价低 14.5%）
```

## 🧠 工作原理

1. **用户请求**：用户告诉 Battle Master 希望运行什么类型的谈判
2. **配置阶段**：Agent 设置谈判场景及双方人格
3. **工具调用**：Agent 在 `buyer_make_offer` 和 `seller_respond` 工具之间轮流调用
4. **AG-UI 流式传输**：每次工具调用都会通过 AG-UI 协议实时发送至前端
5. **Generative UI**：自定义 React 组件将报价和回应实时渲染出来
6. **共享状态**：谈判时间线持续实时更新
7. **最终结果**：根据是否达成交易显示对应结果和动画

## 📚 相关资料

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [AG-UI Protocol Docs](https://docs.ag-ui.com/)
- [CopilotKit Documentation](https://docs.copilotkit.ai/)

## 🤝 参与贡献

欢迎扩展以下内容：

- 新的谈判场景，例如薪资、租房、合同谈判
- 更多人格类型
- 更丰富的 UI 动画和视觉效果
- 跨框架 Agent，例如通过 A2A 集成 LangChain、CrewAI

---

*愿最优秀的谈判者获胜。* 🏆
