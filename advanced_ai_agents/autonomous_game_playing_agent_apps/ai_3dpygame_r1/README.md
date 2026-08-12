# 🎮 基于 DeepSeek R1 的 AI 3D PyGame 可视化工具

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-3d-pygame-visualizer-with-deepseek-r1)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

本项目通过 PyGame 代码生成器和基于浏览器的可视化流程展示 DeepSeek R1 的代码能力。系统使用 DeepSeek 负责推理，OpenAI 负责提取代码，并使用浏览器自动化 Agent 在 Trinket.io 上运行和展示生成的代码。

### 功能特性

- 根据自然语言描述生成 PyGame 代码
- 使用 DeepSeek Reasoner 完成代码逻辑推理和解释
- 使用 OpenAI GPT-4o 提取干净、可执行的代码
- 通过浏览器 Agent 自动在 Trinket.io 上运行和展示代码
- 提供简洁的 Streamlit 操作界面
- 使用多 Agent 系统分别处理导航、编码、执行和结果查看等任务

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/advanced_ai_agents/autonomous_game_playing_agent_apps/ai_3dpygame_r1
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 API Keys

- 注册 [DeepSeek](https://platform.deepseek.com/) 并获取 API Key
- 注册 [OpenAI](https://platform.openai.com/) 并获取 API Key

4. 运行 AI PyGame Visualizer

```bash
streamlit run ai_3dpygame_r1.py
```

5. Browser Use 会自动打开浏览器。按照控制台输出的 URL 访问页面，即可与 PyGame 生成器交互。

### 工作原理

1. **请求处理：** 用户输入希望生成的 PyGame 可视化效果的自然语言描述。
2. **代码生成：**
   - DeepSeek Reasoner 分析请求，并生成详细推理和代码
   - OpenAI Agent 从推理结果中提取干净、可执行的代码
3. **可视化：**
   - 浏览器 Agent 自动在 Trinket.io 上运行代码
   - 多个专用 Agent 分别负责：
     - 导航到 Trinket.io
     - 输入代码
     - 执行代码
     - 查看可视化结果
4. **用户界面：** Streamlit 提供直观界面，用于输入请求、查看代码以及管理可视化流程。
