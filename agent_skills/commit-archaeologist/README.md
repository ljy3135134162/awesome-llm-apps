# 🏺 Commit 考古 Agent Skill

`git blame` 只能告诉你最后是谁修改了某一行，而 Commit Archaeologist 会进一步还原：**为什么这行代码会存在。**

将它指向一个已被 Git 跟踪的文件，并可选择指定行范围。它的本地 Python 脚本会寻找最初引入该代码的提交，按时间顺序整理后续修改，对 Commit Message 进行分类，总结当前代码作者归属，检测经常一起变更的文件，并标记 Issue 引用、回滚、临时绕过方案、临时性设计以及 TODO。

随后，Agent 会把这些证据整理成一份可读的历史说明，并补充代码修改风险提示。

![演示](https://github.com/mvanhorn/awesome-llm-apps/releases/download/demo-assets/commit-archaeologist.gif)

## 安装

```bash
npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/commit-archaeologist
```

[skills CLI](https://skills.sh) 会将该目录安装到兼容的编程 Agent 中。你也可以手动把这个目录复制到 Agent 的 Skills 目录。

## 运行

可以直接向 Agent 提问：

> `src/cache.py` 第 40-72 行为什么会存在？在修改它之前，我应该了解哪些历史背景？

也可以从本仓库的 Clone 中直接运行确定性的核心脚本：

```bash
python3 agent_skills/commit-archaeologist/scripts/archaeologist.py \
  /path/to/repo src/cache.py --lines 40-72 --json
```

省略 `--lines` 时，会跟踪整个文件的历史；省略 `--json` 时，则输出简洁的终端摘要。

## 输出内容

生成的 JSON 考古报告包括：

- 标准化后的代码区域、历史路径别名以及共同变更阈值
- 最初引入该代码的 Commit
- 从旧到新的完整时间线，包括修改过的文件和意图分类
- 高频共同变更文件及支持这些结论的 Commit
- 当前 `git blame` 行数统计，以及历史 Commit 数量
- 基于 Commit 标题和正文提取出的意图信号

整个流程都在本地运行，只依赖 Python 标准库和 Git。脚本为只读模式，不会发起任何网络请求。

## 验证

```bash
python3 agent_skills/evals/commit-archaeologist/test_archaeologist.py
```

评测脚本会自行创建临时 Git 仓库，并检查时间排序、共同变更、作者归属、意图信号、确定性 JSON 输出以及参数校验错误等行为。

## 文件结构

```text
commit-archaeologist/
|-- SKILL.md
|-- README.md
|-- scripts/archaeologist.py
`-- references/reading-git-history.md
```

本项目属于 [awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps)。
采用 Apache-2.0 许可证。最后验证时间：2026 年 7 月。
