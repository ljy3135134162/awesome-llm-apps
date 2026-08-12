# 保险理赔实时 Agent 团队

这是一个语音优先的保险理赔报案应用，允许索赔人自然地进行对话，同时 Agent 会实时构建结构化理赔资料包。界面会显示实时对话、提取出的理赔事实、操作员指引、缺失信息以及可直接交接给理赔员的资料。

该应用按照真实的首次损失通知（FNOL）工作流设计：索赔人无需填写僵硬的表单，操作员也无需手动把杂乱的对话整理成理赔字段。

![保险理赔实时 Agent 团队架构](assets/insurance-claim-live-agent-team-architecture.png)

## 功能

### 语音 + 文本理赔报案

- 与理赔报案 Agent 进行原生语音对话
- 实时显示索赔人与 Agent 双方的对话转录
- 支持文本输入作为理赔信息录入的备用方式
- Agent 实时生成语音回复

### 实时理赔资料包

- 自动提取索赔人姓名、联系方式、保单号、损失类型、日期、地点、事件描述、安全情况、证据以及报告编号
- 随着对话推进持续更新理赔状态
- 高亮显示缺失或不确定的信息
- 在通话进行过程中同步构建可交接给理赔员的资料包

### 操作员指引

- 显示当前理赔处置状态
- 建议下一步最适合提出的问题或需要确认的信息
- 列出完成交接前的阻塞项
- 将面向操作员的摘要与底层审计轨迹分开显示

### 保险业务专用路由

- 支持住宅漏水损失、汽车碰撞、盗窃/财产损失、旅行保险、医疗费用报销示例以及类型不明确的理赔
- 应用确定性的证据与文件检查规则
- 标记人身伤害、安全、居住条件、时间、SIU（特别调查部门）以及升级处理信号
- 避免承诺保险责任范围、赔付金额或责任认定

## 应用引擎

该应用结合实时语音、ADK Graph、结构化信息提取以及确定性的保险业务规则：

| 层 | 模型 / 引擎 | 用途 |
| --- | --- | --- |
| 实时语音 | `gemini-3.1-flash-live-preview` | 语音到语音对话、音频回复和转录 |
| ADK Graph | `agent.py` 中的 `root_agent` | 理赔规范化、分类、验证、路由和资料包生成的唯一事实来源 |
| 结构化提取 | `gemini-3-flash-preview` | 在 ADK Graph 内将杂乱的理赔描述转换为结构化理赔事实 |
| 业务规则 | Python FunctionNodes + Pydantic | 确定性的缺失字段检查、证据门控、安全路由、SIU 信号和交接资料包输出 |
| 应用后端 | FastAPI | 提供前端服务、管理 WebSocket 音频，并在索赔人每次发言后调用 `agent.py` 中的 `run_claim_workflow()` |
| 前端 | HTML、CSS、JavaScript | 专业深色实时操作台，用于语音、对话转录、理赔状态和交接管理 |

## 工作原理

`agent.py` 负责生产环境中的理赔工作流。它公开 ADK `root_agent`，并提供 `run_claim_workflow()` 辅助函数，让实时应用能够以编程方式运行该 Graph。

`server.py` 负责实时 Web 传输。它管理浏览器会话、Gemini Live 音频流、转录以及 FastAPI 路由，但不会重复实现信息提取、分类、证据检查、路由或资料包逻辑。

实时应用流程如下：

```text
索赔人通过语音或文本输入
        |
        v
server.py 捕获本轮输入
        |
        v
run_claim_workflow() 执行 root_agent
        |
        v
ADK Graph 运行 LLM 节点 + 确定性 FunctionNodes
        |
        v
server.py 将返回的理赔状态渲染到 UI
```

## 项目结构

```text
insurance_claim_live_agent_team/
|-- agent.py
|-- schemas.py
|-- policies.py
|-- examples.py
|-- requirements.txt
|-- .env.example
|-- assets/
|   `-- insurance-claim-live-agent-team-architecture.png
|-- live_demo/
|   |-- index.html
|   |-- styles.css
|   |-- app.js
|   `-- server.py
`-- README.md
```

## 开始使用

在应用目录中执行：

```bash
cd voice_ai_agents/insurance_claim_live_agent_team
python3.12 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

编辑 `.env` 并设置 Google API Key：

```bash
GOOGLE_GENAI_USE_VERTEXAI=False
GOOGLE_API_KEY=your-google-api-key
```

## 运行应用

启动后端和前端服务器：

```bash
python -m uvicorn live_demo.server:app --reload --host 127.0.0.1 --port 4177
```

打开应用：

```text
http://127.0.0.1:4177/index.html
```

使用麦克风按钮开始实时理赔对话；如果无法使用麦克风，也可以通过文本框输入理赔信息。
