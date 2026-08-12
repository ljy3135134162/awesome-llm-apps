# ♾️ 自我改进 Agent Skills

使用基于 **Google ADK（Agent Development Kit）** 和 **Gemini** 构建的多 Agent 系统，自动优化你的 Agent Skill。上传一个 Skill，让 Agent 自动生成测试场景和评估标准，然后由三个专门的 ADK Agent 协作，通过迭代优化不断改进 Skill。

<img width="960" height="718" alt="Screenshot 2026-04-12 at 7 26 04 PM" src="https://github.com/user-attachments/assets/35a31f1a-398d-4797-a5d8-de538b4391e5" />

## 工作原理

该应用实现了一套受 Karpathy autoresearch 方法启发的自动 Skill 改进循环，由一组 ADK Agent 驱动：

1. **上传**：上传符合 [agentskills.io](https://agentskills.io) 规范的 Skill 文件夹
2. **配置**：Executor Agent 自动生成测试场景和评估标准，你可以按需编辑、新增或重新生成
3. **优化**：三个 ADK Agent 协作——一个负责执行和评分，一个诊断失败原因，一个应用修复
4. **结果**：下载改进后的 Skill，并获得详细变更日志

### ADK Agent 团队

| Agent | 角色 | 职责 |
|-------|------|------|
| **Executor** | Skill 执行器与评分器 | 根据测试场景执行 Skill，按照评估标准对输出评分，并在分析阶段生成初始测试场景 |
| **Analyst** | 失败诊断器 | 分析失败的评估结果、定位根因并推荐修改策略。使用 Pydantic `output_schema` 保证输出为结构化 JSON |
| **Mutator** | Prompt 编辑器 | 根据 Analyst 的诊断结果，对 Skill Prompt 进行且仅进行一次有针对性的修改。使用 Pydantic `output_schema` 保证结构化 JSON 输出 |

### 优化循环

- **Executor** 针对全部测试场景运行 Skill
- **Executor** 根据二元“是/否”评估标准为每个输出评分
- **Analyst** 诊断失败模式并选择策略：`add_example`、`add_constraint`、`restructure` 或 `add_edge_case`
- **Mutator** 对 Skill Prompt 应用一次精准修复
- **Executor** 重新运行并重新评分修改后的 Skill
- 如果得分提高则保留修改，否则回滚
- 重复执行，直到达到目标通过率或达到最大轮数

## 架构

```
self-improving-agent-skills/
├── backend/                 # FastAPI 服务 + ADK 优化引擎
│   ├── app.py              # REST API 端点 + SSE 流式传输
│   ├── adk_optimizer.py    # 多 Agent 优化器（Executor、Analyst、Mutator）
│   └── requirements.txt
├── frontend/               # Next.js + React + Tailwind
│   ├── src/
│   │   ├── app/            # 主页面 + 布局
│   │   └── components/     # 上传、配置、运行、结果步骤
│   ├── package.json
│   └── *.config.ts
│   ├── code-reviewer/
│   └── content-writer/
└── README.md
```

## 技术栈

- **后端**：Python 3.10+、FastAPI、Google ADK、Pydantic
- **前端**：Next.js 15、React 19、Tailwind CSS v4、Recharts
- **AI**：使用 Gemini（`gemini-3-flash-preview`）的 Google ADK 多 Agent 系统；Analyst 和 Mutator 通过 `output_schema` 输出结构化结果
- **实时通信**：通过 Server-Sent Events（SSE）实时展示优化进度

## 快速开始

### 后端配置

```bash
cd backend

# 创建虚拟环境
python -m venv venv
source venv/bin/activate  # Windows：venv\Scripts\activate

# 安装依赖
pip install -r requirements.txt

# 配置环境（可选——应用也会在 UI 中提示输入 API Key）
cp .env.example .env
# 编辑 .env 并添加 GOOGLE_API_KEY

# 启动服务
python app.py
# 服务运行于 http://localhost:8891
```

### 前端配置

```bash
cd frontend

# 安装依赖
npm install

# 启动开发服务器
npm run dev
# 应用运行于 http://localhost:3000
```

### 使用方法

1. 从 [Google AI Studio](https://aistudio.google.com/apikey) 获取 Gemini API Key
2. 打开 http://localhost:3000
3. 将 Skill 文件夹打包为 `.zip` 后上传，或者使用示例
4. 输入 Gemini API Key
5. 检查并编辑自动生成的测试场景和评估标准
6. 点击“Start Optimization”，观察多个 Agent 协作改进 Skill
7. 完成后下载优化后的 Skill

## Skill 格式

Skill 遵循 [agentskills.io](https://agentskills.io) 规范：

```
my-skill/
├── SKILL.md           # 必需：YAML frontmatter + 指令
├── scripts/           # 可选：可执行代码
├── references/        # 可选：附加文档
└── assets/            # 可选：模板、资源
```

SKILL.md 示例：

```markdown
---
name: my-skill
description: What this skill does and when to use it
license: MIT
metadata:
  author: your-name
  version: "1.0"
---

# My Skill

Your skill instructions here...
```

## 尝试运行

将任意 Skill 文件夹压缩后上传即可。例如，可以使用本仓库中的 [project-graveyard](../project-graveyard/)：

```bash
cd agent_skills
zip -r project-graveyard.zip project-graveyard/
```

应用中的“examples”选择器也会自动列出本仓库同级目录中的真实 Skill，而不是专门制作的玩具示例。

## 多 Agent 优化机制

### 1. 分析阶段

**Executor** Agent 分析 Skill 并生成：

- 3–4 个不同的测试场景
- 4–6 个二元评估标准（是/否问题）

在优化开始前，你可以编辑、新增或删除这些场景和标准。

### 2. 基线运行

**Executor** 针对全部测试场景运行 Skill，并根据所有评估标准对每个输出评分，从而建立初始基准分数。

### 3. 优化循环

每一轮中，三个 Agent 按以下方式协作：

1. **Executor** 针对全部测试场景运行 Skill 并评分
2. **Analyst** 分析失败结果、确定根因并选择修改策略，通过 `output_schema` 返回结构化 JSON
3. **Mutator** 应用一次具体修改来改进 Skill，并通过 `output_schema` 返回结构化 JSON
4. **Executor** 重新运行并重新评分修改后的 Skill
5. 比较得分——如果改善则保留修改，否则回滚
6. 重复执行，直到达到目标通过率或最大轮数

### 4. 输出

- 应用了所有成功修改的改进版 `SKILL.md`
- 详细说明修改内容及原因的变更日志
- 性能对比（基线分数与最终分数）

## API 端点

| 方法 | 端点 | 说明 |
|------|------|------|
| `POST` | `/api/upload` | 上传 Skill ZIP 文件（最大 10MB，仅文本文件） |
| `POST` | `/api/upload-files` | 上传多个文件（文件夹上传） |
| `POST` | `/api/analyze` | 生成测试场景和评估项（需要 Gemini API Key） |
| `POST` | `/api/regenerate` | 重新生成测试场景和评估项 |
| `POST` | `/api/update-config` | 保存用户选择或编辑后的配置 |
| `POST` | `/api/start/{session_id}` | 开始优化 |
| `GET` | `/api/stream/{session_id}` | 通过 SSE 流式返回优化进度 |
| `POST` | `/api/stop/{session_id}` | 停止优化 |
| `GET` | `/api/download/{session_id}` | 下载改进后的 Skill |
| `GET` | `/api/examples` | 列出可用示例 Skill |
| `POST` | `/api/examples/{name}/load` | 加载示例 Skill |
| `GET` | `/api/status/{session_id}` | 轮询状态端点 |
| `GET` | `/health` | 健康检查 |

## 配置

### 后端

Gemini API Key 会由前端随每次请求传递。进行本地开发时，也可以选择在 `.env` 中设置 `GOOGLE_API_KEY`。服务器运行于 **8891** 端口。

上传限制：

- 总上传大小最大 **10MB**
- 单个文件最大 **1MB**
- 每次最多上传 **50** 个文件
- 仅允许文本文件（`.md`、`.txt`、`.json`、`.yaml`、`.py`、`.js`、`.ts` 等）

Session 会在 **1 小时**后自动过期。

### 前端

API Key 在 UI 中输入，仅保存在组件状态中，不会持久化。每次请求都会将 Key 发送到后端，后端通过设置 `GOOGLE_API_KEY` 完成 ADK Agent 身份验证。

### 优化参数

在 `RunningStep.tsx` 中调整 `max_rounds`，最大值为 50：

```typescript
body: JSON.stringify({
  max_rounds: 20,  // 默认：20，最大：50
}),
```

在 `adk_optimizer.py` 中调整模型：

```python
def __init__(self, api_key: str, model: str = "gemini-3-flash-preview"):
```

## 开发

### 后端测试

```bash
cd backend
python -c "from adk_optimizer import SkillOptimizer; print('OK')"
```

### 前端构建

```bash
cd frontend
npm run build
```

### 实时开发

前后端服务器均支持热重载，修改代码后可以立即看到效果。

## 基于 Karpathy 的 Autoresearch

该工具将 Andrej Karpathy 的 autoresearch 方法——利用 LLM 迭代改进自身 Prompt——应用到 Agent Skill 上。

核心思想是：与其手工反复调整 Prompt，不如定义明确的成功标准，然后让 AI 自动优化自身；这里进一步使用了一组专门化 ADK Agent 来完成这一过程。

原始项目：[https://github.com/karpathy/autoresearch](https://github.com/karpathy/autoresearch)
