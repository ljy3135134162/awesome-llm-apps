# Skill 评估

本仓库通过以下方式检查 Skill 是否真正有效——既在发布前检查，也在之后的每次变更中持续检查。目录结构为每个 Skill 对应一个文件夹：`evals/<skill-name>/`，名称与 Skill 保持一致。这些评估文件不会随安装包发布；Skill 文件夹中只包含运行时真正需要的内容。

分层模型沿用 [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills/tree/main/evals) 的设计——层级名称和职责保持一致——同时额外增加了两个本仓库自己的层级，因为这里的 Skill 会附带可执行代码，而他的不会：

| 层级 | 检查内容 | 运行方式 | 成本 |
|---|---|---|---|
| 1. 结构检查 | Frontmatter、命名、`name == dir`、未填写的占位符、纯文本提示词堆砌（`tools/skill_lint.py --strict`） | CI | 免费 |
| 1b. 安全检查 *（本仓库新增）* | 安装诱导、未声明的网络调用、凭据访问、混淆载荷（`tools/skill_scanner.py`） | CI | 免费 |
| 2. 触发与路由 | 正向提示必须能明显区分容易混淆的负向样例；存在 2 个以上 Skill 时，正向提示应将自己的 Skill 排在首位，并且任意两个 Skill 的描述都不能高度近似（`tools/run_trigger_evals.py`） | CI | 免费 |
| 2b. 确定性脚本 *（本仓库新增）* | Skill 自带脚本是否确实完成其声明的功能——使用合成测试夹具覆盖每个分类器、边界情况和输出结构（`<skill>/test_*.py`） | CI | 免费，约 10 秒 |
| 3. 行为评估 | 按照 Skill 执行的 Agent 是否满足其 `expectations[]`。`evals.json` 完全沿用 [skill-creator 的 schema](https://github.com/anthropics/skills/tree/main/skills/skill-creator)，因此其 `run_eval.py`、基准测试和评估查看器无需修改即可直接用于本仓库文件 | 按需 | Token |

## 运行方式

```bash
# 层级 1–2b，与 CI 实际运行内容完全一致——确定性执行，仅依赖 git + Python
python3 agent_skills/evals/tools/skill_lint.py agent_skills/project-graveyard --strict
python3 agent_skills/evals/tools/skill_scanner.py agent_skills
python3 agent_skills/evals/tools/run_trigger_evals.py
python3 agent_skills/evals/project-graveyard/test_graveyard.py
```

层级 3 按需运行，并会消耗 Token。每个 Skill 的 `evals.json` 都采用 skill-creator 的 schema，因此可以直接使用 Anthropic 自己的工具运行（安装 skill-creator 插件，并让 `run_eval.py` 指向对应文件），也可以手动执行——启动一个全新的 Agent 会话，逐个粘贴提示词，并根据 `expectations[]` 评分。`trigger-cases.json` 中标记为 `lexical: false` 的案例（即依赖推理触发，例如 necromancer 模式）只会在这一层覆盖。每当 `SKILL.md` 的行为发生变化时，应重新运行层级 3；每当 `description` 发生变化时，应重新运行层级 2。

## 实际成果

这不是形式主义：层级 1 曾发现一个符号链接路径错误，该错误会在 macOS 上悄悄禁用 relapse 检测；一位外部审查者在干净的 Linux checkout 中运行评估时，又发现了一个文件系统排序错误，它会导致 kill-chain 的归因对象出错。这两个问题都在合并前得到修复。这正是这些评估存在的意义。

## 添加新的 Skill

新增 Skill → 新增对应的 `evals/<skill-name>/`，至少包含一个确定性 `test_<skill>.py`（自包含、使用临时目录测试夹具，并通过退出码 0/1 表示结果）以及一个 `trigger-cases.json`。CI 会自动发现并运行 `test_*.py`。
