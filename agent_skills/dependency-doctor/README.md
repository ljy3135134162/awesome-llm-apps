# 依赖诊断 Agent Skill

Dependency Doctor（依赖诊断器）用于检查单个依赖清单中表层、直接可见的常见问题。它可以发现未固定的版本、与 Python 标准库同名的依赖、已经过时的 backport 包，以及清单内部明显的版本冲突；但它并不是用来诊断 pip 或 uv 依赖解析失败的工具。

它是一个由用户主动调用、在本地运行的开发辅助工具，而不是仓库中的 CI 强制规则。

![演示](https://github.com/mvanhorn/awesome-llm-apps/releases/download/demo-assets/dependency-doctor.gif)

## 检查内容

- 与 Python 标准库同名的错误依赖固定，例如 `pathlib==1.0.1`
- 已经过时的 backport 包，例如 `dataclasses`、`typing`、`enum34` 和 `futures`
- 没有有效版本约束的依赖
- 重复的依赖项以及相互冲突的精确版本固定
- 显式启用 `--online` 时，检查 PyPI 上已被完全撤回（yanked）的版本

离线核心支持 `requirements.txt`、PEP 621 或 Poetry 格式的 `pyproject.toml`，以及 `package.json`。整个离线检查仅使用 Python 标准库。

## 安装

```bash
npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/dependency-doctor
```

安装后可以直接让 Agent 执行：`检查我的 requirements.txt 是否存在依赖问题`。

## 直接运行脚本

```bash
python3 agent_skills/dependency-doctor/scripts/dep_doctor.py requirements.txt --json
```

默认命令不会进行任何网络请求。如果希望检查精确固定的 Python 包版本是否属于 PyPI 上已被完全撤回的版本，可以主动启用在线模式：

```bash
python3 agent_skills/dependency-doctor/scripts/dep_doctor.py requirements.txt --json --online
```

Dependency Doctor 会报告发现的问题并给出建议修复方式，但不会直接修改依赖清单。

安装前也可以在仓库克隆目录中运行评估：

```bash
python3 agent_skills/evals/dependency-doctor/test_dep_doctor.py
```

## 使用范围

这个工具专注于依赖清单本身的检查，不会解析完整的依赖关系图，也不会查询漏洞数据库。

如果需要 CVE 漏洞覆盖，应使用项目认可的安全审计工具。在启用 PyPI 在线查询或应用任何建议修改之前，应先获得用户确认。

Apache-2.0。最后验证：2026 年 7 月。
