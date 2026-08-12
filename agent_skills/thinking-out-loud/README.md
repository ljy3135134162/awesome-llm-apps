# 🎙️ 边想边说（Thinking Out Loud）

直接通过语音把想法一股脑说给 Agent，但在它真正开始行动之前，先检查它到底理解了什么。

语音输入本来就很适合让人连续说上十分钟，而 LLM 也很擅长从杂乱表达中整理出结构。真正危险的是下一步：模型会非常自信地把你没说清楚的部分自动补全。“平时用的那个模型”可能被它默认为某个具体模型；你在表达过程中已经推翻的想法，可能仍被它当成最终决定。随后，一个拥有工具调用能力的 Agent 就会基于错误理解继续执行，而你往往要等大量生成工作完成后才发现问题。

这个 Skill 就是为这种场景设计的：在 Agent 真正执行任何操作之前，它必须先返回一份简短、结构化的理解审计，把你明确说过的内容和它自己的推断、猜测严格分开。这样你只需要纠正几行内容，而不是事后调试一整个已经构建出来的产物。

https://github.com/user-attachments/assets/6f272cf5-8ad9-4d9d-a5bd-538076487b47

## 安装

```bash
npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/thinking-out-loud
```

[skills CLI](https://skills.sh) 会将该目录安装到兼容的编程 Agent 中。你也可以手动将此目录复制到 Agent 的 Skills 目录。

## 使用方式

直接通过语音向 Agent 连续表达想法，不需要刻意整理，越接近真实思考过程越合适。对于这种“边想边说”的输入，Skill 会自动触发，尤其是类似下面这样的开场：

> switching to speech recognition sorry for any typos. so the thing i
> keep coming back to is...

Agent 会先返回一份“回声审计”，其中包括：一句话任务目标、你已经明确锁定的决定和约束、仍未解决的问题、你在表达过程中反复修改或暂时搁置的想法，以及模型自行补充的内容（包括推断和猜测）。

你先修正其中不准确的部分；如有需要，可以再针对尚未确定的问题进行一次简短问答。确认无误后，再批准 Agent 开始执行，并选择是否以及将整理后的 Brief 保存到哪里。

如果你希望分多条消息慢慢表达，可以先显式调用它，例如说“让我先把想法说完”。此时 Agent 只会回复 `listening...`，直到你说 `done` 为止。

## 为什么不只是让 Agent 多问几个澄清问题？

- 澄清问题只能覆盖模型自己觉得“不确定”的地方，而回声审计会检查模型“自认为已经理解”的内容。最危险的往往正是那些模型非常自信、但实际上理解错了的部分，这类错误通常不会触发澄清问题。
- 普通追问通常只会抽样检查三四个点；回声审计会检查整次信息传递。你只需要阅读结果，通过“这是不是我真正想表达的”来识别错误。

## 验证

行为规范位于：
[`agent_skills/evals/thinking-out-loud/`](../evals/thinking-out-loud/)。

安装 Skill 后，在独立的新 Session 中逐条运行 `evals.json` 中的 Prompt，并检查对应预期结果。`trigger-cases.json` 用于验证 Skill 应该在什么情况下触发、什么情况下不应触发。

该 Skill 完全基于 Prompt 实现：没有脚本、不会进行网络请求，并且在没有先询问用户的情况下不会保存任何内容。

## 文件结构

```text
thinking-out-loud/
|-- SKILL.md
|-- README.md
`-- references/echo-format.md
```

属于 [awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps) 项目的一部分。
Apache-2.0。最后验证：2026 年 7 月。
