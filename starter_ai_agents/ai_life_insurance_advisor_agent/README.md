# 🛡️ 人寿保险保障顾问 Agent

这是一个基于 Streamlit 的应用，用于帮助用户估算可能需要的定期寿险保额，并查找当前可用的保险产品。应用使用 **Agno** Agent 框架、**OpenAI GPT-5** 作为 LLM、**E2B** 沙盒进行可复现的保额计算，并通过 **Firecrawl** 执行实时 Web 研究。

## 主要特点
- 精简的信息输入表单，包括年龄、收入、被抚养人、债务、资产、现有保障、保障期限和所在地。
- Agent 会在 E2B 沙盒中运行 Python 代码，使用类似贴现现金流的收入替代模型计算建议保额。
- 使用 Firecrawl 搜索与用户所在地区及保障需求匹配的最新定期寿险产品。
- 返回简洁的保额建议、计算过程说明，以及最多三个带来源链接的产品建议。

## 前置条件
你需要为以下外部服务准备 API Key：

| 服务 | 用途 | 获取地址 |
| --- | --- | --- |
| OpenAI（GPT-5-mini） | 核心推理模型 | https://platform.openai.com/api-keys |
| Firecrawl | Web 搜索与网页抓取 | https://www.firecrawl.dev/app/api-keys |
| E2B | 安全代码执行沙盒 | https://e2b.dev |

## 安装

1. 克隆 GitHub 仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
```

2. 创建并激活虚拟环境（可选，但推荐）。

3. 安装依赖：

```bash
pip install -r requirements.txt
```

4. 运行 Streamlit 应用：

```bash
streamlit run life_insurance_advisor_agent.py
```

## 使用应用

1. 在侧边栏输入 OpenAI、Firecrawl 和 E2B API Key；这些 Key 仅保存在本地 Streamlit Session 中。
2. 填写所需财务信息，并选择收入替代期限。
3. 点击 **Generate Coverage & Options** 启动 Agno Agent 工作流。
4. 查看建议保额、计算依据及推荐的保险公司。原始 Agent 输出可在折叠区域中查看，便于调试。

## 免责声明

本项目仅用于教育和原型验证，**不构成持牌金融建议**。请始终由合格专业人士复核结果，并直接向保险提供商确认具体产品信息。
