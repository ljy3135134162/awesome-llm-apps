## 📡 RouteLLM 聊天应用

> 注意：本项目受到开源 [RouteLLM](https://github.com/lm-sys/RouteLLM/tree/main) 库启发。RouteLLM 可以在不同语言模型之间进行智能路由。

这个 Streamlit 应用展示了 RouteLLM 的使用方式。RouteLLM 会根据任务复杂度，在不同语言模型之间智能分配用户请求。应用提供聊天界面，用户可以直接与 AI 模型交互，而系统会针对每个问题自动选择更合适的模型。

### 功能

- 提供与 AI 模型交互的聊天界面
- 使用 RouteLLM 自动选择模型
- 同时使用 GPT-4 和 Meta-Llama 3.1 模型
- 显示聊天历史以及每次回答所使用的模型信息

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/advanced_tools_frameworks/llm_router_app
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 配置 API Key：

```bash
os.environ["OPENAI_API_KEY"] = "your_openai_api_key"
os.environ['TOGETHERAI_API_KEY'] = "your_togetherai_api_key"
```

注意：在生产环境中，建议通过环境变量或安全的配置管理系统保存 API Key，而不是将其直接硬编码到代码中。

4. 启动 Streamlit 应用

```bash
streamlit run llm_router.py
```

### 工作原理

1. **初始化 RouteLLM**：应用使用两个模型初始化 RouteLLM 控制器：
   - 强模型：GPT-4（mini）
   - 弱模型：Meta-Llama 3.1 70B Instruct Turbo

2. **聊天界面**：用户通过聊天界面输入消息。

3. **模型选择**：RouteLLM 会根据用户问题的复杂度自动选择合适的模型。

4. **生成回答**：被选中的模型根据用户输入生成响应。

5. **展示结果**：应用显示回答，并同时标明实际使用了哪个模型。

6. **保存历史**：应用会维护并显示聊天历史，其中也包含每次回答对应的模型信息。
