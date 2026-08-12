# 💔 分手恢复 Agent 团队

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-breakup-recovery-agent-team-f29b)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个 AI 驱动的应用，通过一组专业化 AI Agent 提供支持、建议和情绪表达渠道，帮助用户从分手后的情绪中恢复。应用使用 **Streamlit** 与 **Agno** 构建，并采用 **Gemini 2.0 Flash（Google Vision Model）**。

## 🚀 功能特性

- 🧠 **多 Agent 团队：**
    - **Therapist Agent（心理支持 Agent）：** 提供共情支持和应对策略。
    - **Closure Agent（释怀 Agent）：** 为用户撰写不应真正发送、仅用于情绪宣泄的信息。
    - **Routine Planner Agent（日程规划 Agent）：** 提供有助于情绪恢复的每日生活安排。
    - **Brutal Honesty Agent（直言 Agent）：** 对分手情况提供直接、不绕弯的反馈。
- 📷 **聊天截图分析：**
    - 可上传聊天截图进行分析。
- 🔑 **API Key 管理：**
    - 可通过 Streamlit 侧边栏安全输入和管理 Gemini API Key。
- ⚡ **并行执行：**
    - 多个 Agent 以协作模式处理输入，提供更全面的结果。
- ✅ **易用界面：**
    - 简洁直观的 UI，方便用户操作并查看各 Agent 的回答。

---

## 🛠️ 技术栈

- **前端：** Streamlit（Python）
- **AI 模型：** Gemini 2.0 Flash（Google Vision Model）
- **图像处理：** PIL（用于显示截图）
- **文本提取：** 使用 Google Gemini Vision 分析聊天截图
- **环境变量：** API Key 通过 Streamlit 的 `st.session_state` 管理

---

## 📦 安装

1. **克隆仓库：**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps
cd starter_ai_agents/ai_breakup_recovery_agent
```

2. **安装依赖：**

```bash
pip install -r requirements.txt
```

3. **运行 Streamlit 应用：**

```bash
streamlit run ai_breakup_recovery_agent.py
```

---

## 🔑 环境变量

请在 Streamlit 侧边栏中提供你的 **Gemini API Key**：

```text
GEMINI_API_KEY=your_google_gemini_api_key
```

---

## 🛠️ 使用方式

1. **输入你的感受：**
    - 在文本区域描述当前的感受和情况。
2. **上传截图（可选）：**
    - 上传 PNG、JPG 或 JPEG 格式的聊天截图进行分析。
3. **运行 Agents：**
    - 点击 `Get Recovery Support` 启动多 Agent 团队。
4. **查看结果：**
    - 界面会分别显示各个 Agent 的回答。
    - 最后由 Team Leader 提供综合总结。

---

## 🧑‍💻 Agent 说明

- **Therapist Agent**
    - 提供共情支持和应对策略。
    - 使用 **Gemini 2.0 Flash（Google Vision Model）** 和 DuckDuckGo 工具获取相关信息。

- **Closure Agent**
    - 生成不会真正发送、用于情绪释放的信息。
    - 强调真诚、自然的情绪表达。

- **Routine Planner Agent**
    - 创建平衡的每日恢复计划。
    - 包括自我反思、社交活动以及健康的注意力转移方式。

- **Brutal Honesty Agent**
    - 对分手情况提供直接、客观的反馈。
    - 使用基于事实、不刻意美化的表达方式。
