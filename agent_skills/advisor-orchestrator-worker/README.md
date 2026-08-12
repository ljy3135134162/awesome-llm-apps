# 🧠 顾问-编排器-执行者 Skill

**单个模型容易成为瓶颈；一个拥有一个大脑、二十双手和一名董事会顾问的团队则不会。**

这个 Skill 会把你的编程 Agent 变成一个三层模型团队的编排器。大型任务会被拆分成彼此独立的任务简报，并分发给低成本并行 Worker 执行；每个结果都会单独验证，同时只在两个关键节点调用更强模型进行判断：工作开始前一次，交付前一次。

<img width="3004" height="1408" alt="advisor_skill" src="https://github.com/user-attachments/assets/6f5dc5e8-6828-4598-b23c-72ede97fa238" />

## 团队组成

| 角色 | 默认模型 <sub>（2026 年 7 月，可自由替换）</sub> | 负责内容 | 明确不做什么 |
|---|---|---|---|
| **Orchestrator（编排器）** | GPT-5.6 | 定义成功标准、规划执行波次、分发任务简报、验证每个结果并综合最终交付物 | 不承担 Worker 级别的重复性执行工作 |
| **Workers（执行者）** | Gemini 3.5 Flash | 每个 Worker 独立处理一个自包含子任务，并行、无状态；只看到自己的任务简报，在子任务需要时使用工具 | 不互相通信、不扩张任务范围、同一次调用失败后不会自动获得第二次机会 |
| **Advisor（顾问）** | Claude Fable 5 | 在任务分发前审核计划，在交付前进行最终质量判断；仅在重大决策边界时允许中途调用 | 不直接执行任务 |

这个设计的核心就是成本结构：需要大量生成时使用廉价并行模型，需要影响关键决策时才使用昂贵的强模型进行判断。每次运行都会在开始时声明预算，并根据计划设定上限，不会在后台悄悄超支；如果预算不足，要么如实报告，要么明确向用户请求增加预算，而不是继续无声消耗。

模型本身只是可调参数。真正稳定、长期有效的是这种分层协作模式；这里列出的默认模型仅代表 2026 年 7 月时的推荐配置。

## 为什么这种架构不容易失控

多模型协作流程通常会因为上下文泄漏、局部失败未被发现，或质量判断介入过晚而崩坏。这里为每类问题都设置了明确规则：

- **无状态任务简报**（[references/worker-brief.md](references/worker-brief.md)）：每次任务分发都会在简报中完整携带输入、上下文和验收标准，不依赖任何共享上下文，也不会出现“如上文所述”这种隐式引用。任务简报通过临时文件传递（传给 `agy` 时使用经过引用的展开方式，fallback API 路径则通过 `jq` 构造请求体），不会把内容直接拼接进 Shell 字符串。
- **先验证，再合并**：每个结果都必须按照自身验收标准进行判断，结果只有三种：PASS、FIX（指出失败项后重新分发）或 ESCALATE。不存在“部分通过但默默接受”，也不会由编排器手工补丁式修复。
- **Advisor 是批评者，不是执行者**（[references/advisor-consult.md](references/advisor-consult.md)）：它只输出结论、按优先级排序的风险和具体修正建议，且控制在 300 字以内。每一条意见都必须被实际采用，或者明确说明为什么不采用。

## 安装

```bash
npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/advisor-orchestrator-worker
```

也可以直接把该目录复制到 Agent 的 Skills 目录，例如：`~/.claude/skills/`、`~/.codex/skills/` 或 `~/.agents/skills/`。

**依赖要求**（按照本仓库规则提前声明）：Worker 默认需要 `agy` CLI，Advisor 默认需要 `claude` CLI，同时还需要 `jq`。

如果缺少某个 CLI，该角色会回退到 API Key 模式：

- Worker：使用 `GEMINI_API_KEY` 或 `GOOGLE_API_KEY`
- Advisor：使用 `ANTHROPIC_API_KEY`

Orchestrator 就是当前加载该 Skill 的 Agent。你可以通过特定模型对应的 CLI 启动，例如 `codex exec`，来选择由哪个模型担任 Orchestrator。

如果某个角色既没有对应 CLI，也没有 API Key，Skill 会说明缺失项，并提供一个明确标记的降级运行模式。

## 使用方式

可以直接对 Agent 说：

> “这个任务一次处理不完，使用模型团队编排执行。”
>
> “把任务并行拆开：同时研究 12 个竞争对手，然后综合结果。”
>
> “对这个任务运行 advisor-worker 协作循环。”

每次运行结束时都会输出：最终交付物、执行计划、每个子任务的验证记录、Advisor 意见中已采纳和未采纳的部分，以及仍然存在的风险。

## 文件结构

```text
advisor-orchestrator-worker/
├── SKILL.md                          # 核心协作循环、团队角色、预算和升级规则
├── README.md                         # 当前文件
├── references/worker-brief.md        # Worker 接收的无状态任务简报格式
├── references/advisor-consult.md     # Advisor 返回的咨询格式
└── references/fallbacks.md           # Gemini 和 Anthropic API 回退调用方式
```

评估文件保存在仓库中的 `agent_skills/evals/advisor-orchestrator-worker/`；安装 Skill 时只会安装实际运行所需的部分。

属于 [awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps) 项目 · Apache-2.0 · 最后验证：2026 年 7 月
