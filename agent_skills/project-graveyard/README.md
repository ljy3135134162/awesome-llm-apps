# 🪦 Project Graveyard Agent Skill

**每个开发者都有这样一个文件夹：二十多个已经“死掉”的项目，每个项目都因为某些没人记录下来的原因被放弃。**

这个 Skill 会读取你机器上所有废弃项目的 Git 历史，并回答三个问题：

**每个项目为什么死掉了？** 看证据，不靠感觉。最后几次提交都在改 Stripe 相关文件？那它很可能死在支付功能这堵墙前。另一个仓库的第一次提交，正好出现在这个项目最后一次提交后的几天内？那“凶手”就找到了。

**你的放弃模式是什么？** 也许你的项目总是在第 19 天死亡；也许 6 个项目里有 4 个，都是在你开始新项目后的 48 小时内被放弃。

**哪个项目其实还活着？** 在那个文件夹里，可能有一个已经完成 90% 的项目：代码写完了、文档也有了，只是从未发布。这个 Skill 会找到它，检查自从项目停止后有哪些事情变得更容易，并整理出从当前状态到真正上线之间还需要完成的少量步骤。

下面是一份典型扫描结果的示意（项目名称是虚构的，但分类器结论来自真实规则）。你的 Agent 会把结果整理成一场“葬礼”：为每个项目写墓志铭，指出你的重复模式，并主动询问是否要立即复活那个生命迹象最强的项目。你自己的扫描结果通常会更扎心。

<img width="1672" height="941" alt="ChatGPT Image Jul 9, 2026, 06_46_05 PM" src="https://github.com/user-attachments/assets/b80456c7-cd6f-49d8-adcf-641230d4c601" />

## 可以检测什么

根据 Git 历史判断项目“死因”：

| 死因 | 判断依据 |
|---|---|
| **新鲜事物诱惑（shiny object）** | 你拥有的另一个仓库，在当前项目最后一次提交后的几天内出现首次提交。系统会指出这个“凶手”。 |
| **部署恐惧（deploy fear）** | README 已完成、有 20+ 次提交、存在真实业务代码，但完全没有部署配置。项目已经能工作，却从未真正发布。 |
| **支付 / 认证墙（payments / auth wall）** | 最后的提交集中修改 Stripe、Billing、OAuth 或 Login 相关代码。 |
| **样板配置墙（boilerplate wall）** | 所有文件变更中 60% 以上都是配置文件。项目死在了配置阶段。 |
| **重写螺旋（rewrite spiral）** | 多次出现 rewrite / migrate 类型提交；不断重构重写，却没有完成产品。 |
| **范围爆炸（scope explosion）** | 项目超过 100 个文件，却没有部署配置。它一直变大，而不是走向上线。 |
| **慢性衰亡（slow fade）** | 提交间隔越来越长，最后彻底停止。没有明显障碍，也没有明确“凶手”，只是逐渐失去动力。 |

它还会区分：**已完成项目**（已部署、已发布、有文档；它们不是被放弃，而是真正完成了）以及**未使用版本控制的项目**（没有 Git，因此无法进行“尸检”）。随后，系统会根据每个废弃项目距离上线还有多远，计算其 **Pulse（生命迹象）**，并由 Agent 继续处理：

- **尸检访谈（Autopsy interview）**：对于死因不明确的项目，Agent 会主动提问；来自 Git 的证据会标注为 *(forensic / 取证结果)*，你的回答会标注为 *(confirmed / 已确认)*。
- **环境变化检查（World-check）**：在建议复活方案之前，会先检查项目停止后外部环境发生了什么变化，例如以前难用的 API 现在有了 SDK，或者某个模型成本已经降低 20 倍。
- **项目复活（Resurrection）**：生成最多 7 步的复活计划，最后一步必须是 *shipped（真正上线）*，并主动询问是否立即执行第 1 步。
- **复发监控（Relapse watch）**：通过 `--state` 和 `--mark-resurrected` 记录已复活项目；后续扫描会报告该项目是否仍在持续推进。
- **死灵法师模式（Necromancer mode）**：当你让 Agent 开始构建一个新项目时，它会先检查“墓地”；你可能早在 2024 年就已经做完了其中 60%。

## 安装（10 秒）

```bash
npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/project-graveyard
```

[skills CLI](https://skills.sh) 会将它安装到你当前使用的 Agent 中，例如 Claude Code、Codex、Cursor、Copilot、Antigravity 等；也可以直接把这个目录复制到你的 Agent Skill 目录中。然后输入：*“run the graveyard on ~/dev and ~/projects”*。

也可以完全脱离 Agent 独立运行：

```bash
python3 project-graveyard/scripts/graveyard.py ~/dev ~/projects
```

## 扫描范围与隐私

所有操作都在本地执行：只有一个纯 Python 文件，仅使用标准库，不发起任何网络请求，并且整个扫描过程只读。它只读取 Git 元数据（提交日期、提交信息、文件名），**不会读取你的代码内容**。

你指定哪些目录，它就只扫描哪些目录；如果不传目录，则只会检查脚本中预设的一组常见项目路径（`DEFAULT_ROOTS`，脚本第 30 行附近），不会扫描“你整台电脑上的所有内容”。如果你希望公开分享扫描报告，可以使用 `--redact`，将项目名称替换为 `project-1..n`。

安装之前也可以先从本仓库的 Clone 中验证它是否工作：

```bash
python3 agent_skills/evals/project-graveyard/test_graveyard.py   # 16 项检查，约 10 秒
```

限制：没有 Git 的项目无法进行尸检（会计数，但不会诊断死因）。默认将 **45 天以上没有任何提交** 的项目视为“死亡”，可通过 `--days` 调整。已在 macOS 和 Linux 上测试。

## 文件结构

```text
project-graveyard/                  # ← 安装时实际只会复制这个目录
├── SKILL.md                        # Agent 指令：报告格式、墓志铭规则、复活流程
├── README.md                       # 当前说明文件
├── scripts/graveyard.py            # 扫描 + 尸检 + Pulse 排名（Python 3.8+、仅标准库、离线）
└── references/causes-of-death.md   # 死因分类体系：信号、置信度及每种死因的复活策略
```

属于 [awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps) 项目的一部分 · Apache-2.0 · 最后验证：2026 年 7 月
