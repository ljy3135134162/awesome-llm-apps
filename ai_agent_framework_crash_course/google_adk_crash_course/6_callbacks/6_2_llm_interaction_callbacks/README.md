# 6.2 LLM 交互回调

本教程演示如何使用 `before_model_callback` 和 `after_model_callback` 监控 LLM 请求与响应。

## 🎯 学习目标

- 理解 LLM 交互回调
- 学习如何监控 LLM 请求和响应
- 跟踪 Token 使用量和响应时间
- 估算 API 成本
- 监控 LLM 性能指标

## 📁 项目结构

```text
6_2_llm_interaction_callbacks/
├── agent.py          # 配置 LLM 交互回调的 Agent
├── app.py            # Streamlit Web 界面
├── requirements.txt  # Python 依赖
└── README.md         # 本文件
```

## 🔧 配置

1. **安装依赖：**
   ```bash
   pip install -r requirements.txt
   ```

2. **配置 API Key：**
   ```bash
   # 创建 .env 文件
   echo "GOOGLE_API_KEY=your_api_key_here" > .env
   ```

## 🚀 运行演示

### 命令行演示
```bash
python agent.py
```

### Web 界面
```bash
streamlit run app.py
```

## 🧠 核心概念：LLM 交互监控

LLM 交互回调允许你监控 Agent 与底层语言模型之间的通信，从而获得请求、响应以及性能指标方面的可观测性。

### **LLM 交互流程**

```text
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│    用户输入     │───▶│  LLM 请求回调   │───▶│  LLM 响应回调   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                              │                       │
                              ▼                       ▼
                       ┌─────────────────┐    ┌─────────────────┐
                       │   模型 API      │    │ Token 使用量与  │
                       │   （Gemini）    │    │    性能指标     │
                       └─────────────────┘    └─────────────────┘
```

### **回调执行时间线**

```text
时间线：──────────────────────────────────────────────────────────▶

用户消息
    │
    ▼
┌─────────────────┐
│ before_model    │ ← 记录开始时间和模型信息
│ _callback       │
└─────────────────┘
    │
    ▼
┌─────────────────┐
│ LLM API 调用    │ ← 实际向 Gemini 发起请求
│ (Gemini 3 Flash)│
└─────────────────┘
    │
    ▼
┌─────────────────┐
│ after_model     │ ← 计算耗时、Token 和成本
│ _callback       │
└─────────────────┘
    │
    ▼
返回用户响应
```

## 📖 代码解析

### **1. 回调函数**

两个回调配合使用，用于监控完整的 LLM 交互过程。

**请求前回调（`before_model_callback`）：**
- 从 `llm_request` 提取模型信息
- 记录请求时间戳
- 将数据存入 Session State，供响应回调读取
- 记录请求详情，包括模型、时间和 Agent

**响应后回调（`after_model_callback`）：**
- 从 Session State 获取已保存的请求数据
- 计算响应耗时
- 从 `llm_response.usage_metadata` 提取 Token 使用量
- 根据 Token 数量估算 API 成本
- 记录性能指标

### **2. 回调之间的状态管理**

```text
Session State 流程：
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ before_callback │───▶│  Session State  │───▶│ after_callback  │
│ 写入：          │    │                 │    │ 读取：          │
│ - start_time    │    │ - llm_request_  │    │ - start_time    │
│ - model         │    │   time          │    │ - model         │
│ - prompt_length │    │ - llm_model     │    │ - prompt_length │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### **3. Agent 配置**

Agent 同时配置两个模型回调：
- `before_model_callback`：监控请求开始
- `after_model_callback`：监控响应完成
- 使用 `InMemoryRunner`，确保回调正确触发

## 🧪 测试示例

### **示例输出格式**

```text
🤖 LLM Request to gemini-3-flash-preview
⏰ Request time: 19:15:30
📋 Agent: llm_monitor_agent

📝 LLM Response from gemini-3-flash-preview
⏱️ Duration: 1.45s
🔢 Tokens: 156
💰 Estimated cost: $0.0004
```

### **各项指标的含义**

- **⏰ 请求时间**：LLM 请求开始的时间
- **⏱️ 持续时间**：从请求发出到响应返回的总耗时
- **🔢 Token**：总 Token 消耗量，包括输入和输出
- **💰 估算成本**：根据 Token 使用量计算的近似 API 成本

## 🔍 关键概念

### **LLM 请求监控**
- **模型信息**：跟踪当前使用的模型
- **时间记录**：记录请求时间戳
- **状态管理**：保存请求数据，供响应分析使用

### **LLM 响应监控**
- **响应时间**：计算从请求到响应的耗时
- **Token 使用量**：跟踪总 Token 消耗
- **成本估算**：估算 API 调用成本

### **Usage Metadata**
- **Token 数量**：`llm_response.usage_metadata.total_token_count`
- **模型信息**：可从请求和响应对象中获取
- **时间数据**：通过 Session State 在两个回调之间传递

## 🎯 应用场景

- **性能监控**：跟踪 LLM 响应时间
- **成本管理**：监控 API 使用量和费用
- **质量保证**：分析 Prompt 与响应模式
- **调试**：排查 LLM 交互问题
- **数据分析**：收集使用统计与指标

## 🚨 常见错误

1. **回调函数签名错误：**
   ```python
   # ❌ 错误
   def before_model_callback(context, model, prompt):

   # ✅ 正确
   def before_model_callback(callback_context: CallbackContext, llm_request):
   ```

2. **错误提取 Token：**
   ```python
   # ❌ 错误
   tokens = llm_response.usage_metadata.get('total_token_count')

   # ✅ 正确
   tokens = getattr(llm_response.usage_metadata, 'total_token_count', 0)
   ```

3. **没有使用 InMemoryRunner：**
   ```python
   # ❌ 错误：回调不会触发
   agent.run(message)

   # ✅ 正确
   runner.run_async(...)
   ```

## 🔗 后续步骤

- 继续教程 6.3：工具执行回调
- 尝试不同的成本估算模型
- 添加响应质量指标
- 实现限流和配额管理
- 构建自定义分析 Dashboard
