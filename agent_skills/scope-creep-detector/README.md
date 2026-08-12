# 🔭 范围蔓延检测器

根据一行简短的开发意图，检查工作区、暂存区、已保存内容或分支之间的 diff，判断修改是否超出了原本计划的范围。

它会标记无关路径、新增依赖、公共 API 重命名、配置或 CI 修改、过大的代码块、仅格式化的文件，以及修改跨越过多子系统等情况。最终会生成一份本地 JSON 报告，帮助你决定哪些修改应该保留、拆分或给出合理说明。

![范围蔓延检测器演示](https://github.com/mvanhorn/awesome-llm-apps/releases/download/demo-assets/scope-creep-detector.gif)

演示中分析的是一个“修复解析器中的空指针解引用”修改，但实际提交同时改动了 CI 配置、新增了依赖，并加入了一个无关文件。真正的解析器修复会被保留，其余修改则会被标记出来。

## 安装

```bash
npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/scope-creep-detector
```

## 运行

可以直接询问 Agent：

> 我的修改是否已经超出了原本只修复解析器问题的范围？

也可以直接运行确定性的核心脚本：

```bash
python3 scripts/scope_creep.py --repo /path/to/repo \
  --intent "fix null dereference in parser" --json
```

使用 `--staged` 可检查暂存区，使用 `--base main` 可检查相对于主分支的 diff，或者使用 `--diff -` 从标准输入读取 unified diff。

该脚本仅使用 Python 标准库，不会进行任何网络请求，也不会修改仓库内容。
