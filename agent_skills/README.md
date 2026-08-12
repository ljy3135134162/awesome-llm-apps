# 🧩 Agent Skills

**适用于 Claude Code、Codex、Cursor、OpenClaw、Hermes、Antigravity，以及任何兼容 [SKILL.md](https://agentskills.io) 的 Agent，可直接安装使用。**

一个 Skill 就是一个包含 `SKILL.md` 的文件夹，并可附带脚本和参考资料；Agent 会在需要时自动发现并加载它。同一个 Skill 可以跨 Claude Code、Codex、Cursor 和其他 Coding Agent 使用。

## 收录标准

许多 Skill 注册表中的所谓“Skills”实际上只是纯文本 Prompt 集合——把模型本来就知道的建议包装进 frontmatter。本仓库中的 Skill 必须真正提供额外价值：

- **真实脚本** —— 确定性的工作通过代码执行，而不是消耗 Token 生成
- **经过研究的参考资料** —— 深度内容按需加载，并提供来源
- **证据优先** —— Skill 提出的每项结论都应该可以验证
- **默认本地且私密** —— 除非明确声明，否则不会进行网络请求，也不会让数据离开你的机器
- **发布前经过测试** —— 使用真实输入测试，而不仅仅验证理想路径的测试样例

## Skills

| Skill | 功能 |
|---|---|
| [🧠 advisor-orchestrator-worker](advisor-orchestrator-worker/) | 将 Agent 变成三级模型团队的编排器：廉价、无状态 Worker 并行执行任务，昂贵的 Advisor 仅在关键决策边界介入，每一步之间都有验证门，并设置预算避免一次运行消耗过多 API 费用 |
| [🏺 commit-archaeologist](commit-archaeologist/) | 根据本地 Git 历史重建某个文件或代码区域存在的原因，包括首次引入它的 Commit、后续修改、经常一起变化的文件、当前作者归属以及设计意图线索 |
| [🩺 dependency-doctor](dependency-doctor/) | 检查依赖清单中的标准库同名依赖、过时 backport、未固定版本、重复或冲突约束，并可选择检查 PyPI 上已撤回的版本 |
| [🪦 project-graveyard](project-graveyard/) | 扫描电脑中的废弃副项目，通过 Git 历史分析每个项目停止开发的原因（例如害怕部署、支付功能障碍、被新项目取代），总结个人项目失败模式，并找出最值得复活的项目，同时跟踪复活后的再次停滞情况 |
| [🔭 scope-creep-detector](scope-creep-detector/) | 根据声明的修改意图检查 diff，标记无关文件和范围扩张信号，并建议哪些修改应该保留、拆分或说明理由 |
| [♾️ self-improving-agent-skills](self-improving-agent-skills/) | 使用 Gemini 和 ADK 自动优化 Agent Skills |
| [🎙️ thinking-out-loud](thinking-out-loud/) | 在 Agent 真正执行前审计它从语音表达中理解到了什么：针对一段自由口述生成可快速检查的结构化摘要，将模型自己的猜测单独隔离，并标记用户中途推翻的决定。追问只能验证模型“不确定”的内容，而回显审计能够验证模型“自认为已经理解”的内容 |

更多 Skill 将逐个发布。

## ⚡ 安装

一条命令即可适配不同 Agent。[skills CLI](https://skills.sh) 会检测你安装的 Claude Code、Codex、Cursor、Copilot、Antigravity、OpenClaw、Hermes 等 Coding Agent，并自动将 Skill 放入正确的位置：

```bash
npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/<skill>
```

如果更喜欢手动安装，可以克隆仓库，然后把 Skill 文件夹复制到对应 Agent 的 Skills 目录：

| Agent | Skills 目录 |
|---|---|
| Claude Code | `~/.claude/skills/` |
| Codex | `~/.codex/skills/` |
| Cursor | `~/.cursor/skills/` |
| GitHub Copilot / VS Code | `~/.copilot/skills/` |
| Antigravity CLI | 项目中的 `.agents/skills/` |
| OpenClaw | `~/.openclaw/skills/` |
| Hermes | `~/.hermes/skills/`（同时也会读取 `~/.agents/skills/`） |

团队安装：将 Skill 放入仓库中的 `.agents/skills/`。这是 2026 年多数 Agent 使用的共享项目级目录，包括 Codex、Cursor、Copilot 和 Antigravity；Claude Code 使用 `.claude/skills/`。

## 安装任何 Skill 前——包括本仓库的 Skill

Skill 会继承 Agent 的权限，包括 Shell、文件以及凭据访问权限。因此应该把 Skill 当作软件，而不是普通文档。

无论 Skill 来自本仓库还是其他来源，在安装前都应阅读 `SKILL.md` 以及其中的所有脚本。本仓库中的 Skill 会提前声明任何网络访问行为，并且不会在安装阶段执行代码——不会要求你的 Agent 执行任何类似 `curl | bash` 的命令。

每个 Skill 在 [`evals/`](evals/) 中还提供了可执行评估。你可以在克隆仓库后、正式安装前运行这些测试。同时请注意实际安装时不需要复制的内容：Skill 文件夹本身只包含运行时真正需要的文件。
