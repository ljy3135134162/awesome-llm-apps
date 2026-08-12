# 🔌 教程 7：插件

## 🎯 你将学到什么
- **插件基础**：理解插件是什么以及它们如何工作
- **全局回调管理**：使用插件处理跨模块关注点
- **实用插件示例**：日志、监控和请求修改

## 💡 核心概念：插件

Google ADK 中的插件是自定义代码模块，可以通过回调钩子在 Agent 工作流生命周期的多个阶段执行。与配置在单个 Agent 或工具上的普通回调不同，插件只需要在 `Runner` 上注册一次，就会全局作用于该 Runner 管理的所有 Agent、工具和 LLM 调用。

### **插件与普通回调对比**
```text
┌─────────────────┐    ┌─────────────────┐
│    普通回调     │    │      插件       │
├─────────────────┤    ├─────────────────┤
│ • 针对单个 Agent│    │ • 全局作用域    │
│ • 针对单个工具  │    │ • Runner 级别   │
│ • 面向特定任务  │    │ • 处理横切逻辑  │
│ • 局部生效      │    │ • 可复用        │
└─────────────────┘    └─────────────────┘
```

### **插件生命周期钩子**
```text
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  用户消息回调   │───▶│ Runner 开始回调 │───▶│ Agent 执行回调  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  模型回调       │    │   工具回调      │    │   事件回调      │
│ （执行前/后）   │    │ （执行前/后）   │    │ （修改事件）    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   错误处理回调  │    │ Runner 结束回调 │    │ 清理任务与报告  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### **为什么使用插件？**
- **横切关注点**：实现作用于整个应用的通用功能
- **可复用性**：将相关回调函数封装成插件重复使用
- **全局监控**：统一跟踪所有 Agent、工具和模型交互
- **策略执行**：实现安全护栏与访问控制
- **日志与指标**：集中化日志和性能监控
- **请求/响应修改**：动态调整输入和输出

## 📖 教程概览

本教程通过一个综合示例展示如何在 Google ADK 中创建和使用插件。

### **演示插件功能**
1. **请求日志**：记录所有用户消息和时间戳
2. **请求修改**：为用户消息添加时间上下文
3. **Agent 跟踪**：统计并监控 Agent 执行次数
4. **工具监控**：跟踪工具使用情况并处理错误
5. **最终报告**：生成插件活动摘要

## 📁 项目结构

```text
7_plugins/
├── README.md                           # 本文件：概念说明
├── agent.py                            # 使用插件的 Agent 实现
├── app.py                              # Streamlit 界面
├── requirements.txt                    # 依赖
└── plugin_example.py                   # 独立插件演示
```

## 🎯 学习目标

完成本教程后，你将理解：

- ✅ **插件架构**：插件如何继承 `BasePlugin`
- ✅ **全局作用域**：插件如何影响所有 Agent 和工具
- ✅ **回调钩子**：可用的插件回调方法及其触发时机
- ✅ **实际应用**：插件在真实项目中的常见用途
- ✅ **错误处理**：如何实现优雅的故障恢复
- ✅ **简单实现**：如何组织清晰易懂的插件结构

## 🚀 快速开始

### **前置条件**
- Python 3.11+
- Google AI Studio API Key
- 已理解 Google ADK 基础内容（教程 1-6）

### **配置**
1. **获取 API Key**：访问 [Google AI Studio](https://aistudio.google.com/)
2. **创建 `.env` 文件**：在当前目录创建 `.env`，内容如下：
   ```text
   GOOGLE_API_KEY=your_google_ai_studio_api_key_here
   ```
3. **安装依赖**：`pip install -r requirements.txt`

**重要说明**：
- 确保 `.env` 与教程文件位于同一目录
- 将 `your_google_ai_studio_api_key_here` 替换为真实 API Key
- 不要将 `.env` 提交到版本控制系统

### **运行教程**
```bash
cd 7_plugins
streamlit run app.py
```

## 🔧 关键概念

### **插件类结构**
```python
from google.adk.plugins.base_plugin import BasePlugin

class MyPlugin(BasePlugin):
    def __init__(self):
        super().__init__(name="my_plugin")
        # Initialize plugin state

    async def before_agent_callback(self, *, agent, callback_context):
        # Called before each agent execution
        pass

    async def after_model_callback(self, *, callback_context, llm_response):
        # Called after each model call
        pass
```

### **插件注册**
```python
from google.adk.runners import InMemoryRunner

runner = InMemoryRunner(
    agent=my_agent,
    app_name="my_app",
    plugins=[MyPlugin()]  # 在这里注册插件
)
```

### **可用回调钩子**
- `on_user_message_callback`：修改用户输入
- `before_run_callback`：执行前初始化
- `before_agent_callback` / `after_agent_callback`：Agent 生命周期
- `before_model_callback` / `after_model_callback`：模型交互
- `before_tool_callback` / `after_tool_callback`：工具执行
- `on_model_error_callback` / `on_tool_error_callback`：错误处理
- `on_event_callback`：在流式输出前修改事件
- `after_run_callback`：执行结束后的清理工作

## 🎯 应用场景

### **常见插件用途**
1. **日志与链路追踪**：记录详细信息，用于调试与分析
2. **策略执行**：实现安全护栏和访问控制
3. **监控与指标**：进行性能跟踪和数据分析
4. **响应缓存**：缓存高成本操作
5. **请求/响应修改**：补充上下文或统一输出格式
6. **错误恢复**：优雅处理失败情况
7. **使用分析**：跟踪调用模式和使用统计

## 🚨 重要说明

- **插件优先级**：插件回调会在 **Agent 级回调之前**执行
- **全局作用域**：插件会影响 Runner 中的**所有** Agent、工具和模型
- **Web 界面**：ADK Web 界面**不支持插件**
- **错误处理**：插件错误回调可以拦截异常并提供备用结果

## 📚 后续步骤

完成本教程后，你可以继续：
- 为具体业务场景创建自定义插件
- 实现监控与分析插件
- 构建安全与策略控制插件
- 探索更高级的插件组合模式

## 🔧 故障排查

### **常见问题**

**“Missing key inputs argument” 错误**
- 确保已经创建包含 Google AI Studio API Key 的 `.env`
- 检查 API Key 是否有效且具备必要权限
- 确认 `.env` 与教程文件位于同一目录

**导入错误**
- 确保已经安装全部依赖：`pip install -r requirements.txt`
- 确认使用 Python 3.11 或更高版本

**插件没有生效**
- 注意 ADK Web 界面不支持插件
- 请通过 Streamlit 应用或 Python 脚本运行 Agent

## 🔗 其他资源

- [Google ADK 插件文档](https://google.github.io/adk-docs/plugins/)
- [插件回调钩子](https://google.github.io/adk-docs/plugins/#plugin-callback-hooks)
- [BasePlugin API 参考](https://google.github.io/adk-docs/api/python/google.adk.plugins.base_plugin.BasePlugin)
