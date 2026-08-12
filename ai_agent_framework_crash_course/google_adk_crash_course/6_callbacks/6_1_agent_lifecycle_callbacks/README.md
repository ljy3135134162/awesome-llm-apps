# 6.1 Agent 生命周期回调

本教程演示如何使用 `before_agent_callback` 和 `after_agent_callback` 监控 Agent 的完整执行生命周期。

## 🎯 学习目标

- 理解 Agent 生命周期回调
- 学习如何监控 Agent 执行耗时
- 了解如何在回调之间共享状态
- 实践性能监控的实现方式

## 📁 项目结构

```text
6_1_agent_lifecycle_callbacks/
├── agent.py          # 配置生命周期回调的 Agent
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

## 🧠 核心概念：Agent 生命周期监控

Agent 生命周期回调允许你监控 Agent 执行的开始与结束，从而明确 Agent 何时开始处理任务、何时完成任务。

### **Agent 生命周期流程**

```text
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│    用户输入     │───▶│ Agent 开始回调  │───▶│ Agent 结束回调  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                              │                       │
                              ▼                       ▼
                       ┌─────────────────┐    ┌─────────────────┐
                       │ Agent 逻辑执行  │    │    性能指标     │
                       └─────────────────┘    └─────────────────┘
```

### **回调执行时间线**

```text
时间线：──────────────────────────────────────────────────────────▶

用户消息
    │
    ▼
┌─────────────────┐
│ before_agent    │ ← 记录开始时间和 Agent 信息
│ _callback       │
└─────────────────┘
    │
    ▼
┌─────────────────┐
│ Agent 逻辑执行  │ ← Agent 核心处理过程
└─────────────────┘
    │
    ▼
┌─────────────────┐
│ after_agent     │ ← 计算耗时并记录完成信息
│ _callback       │
└─────────────────┘
    │
    ▼
返回用户响应
```

## 📖 代码解析

### **1. 回调函数**

两个回调配合使用，以监控完整的 Agent 生命周期。

**执行前回调（`before_agent_callback`）：**
- 记录执行开始时间戳
- 将开始时间存入 Session State，供结束回调读取
- 记录 Agent 开始执行的信息，包括 Agent 名称和时间
- 返回 `None`，允许 Agent 正常继续执行

**执行后回调（`after_agent_callback`）：**
- 从 Session State 获取开始时间
- 计算总执行耗时
- 记录完成状态和性能指标
- 返回 `None`，继续使用原始执行结果

### **2. 回调之间的状态管理**

```text
Session State 流程：
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ before_callback │───▶│  Session State  │───▶│ after_callback  │
│ 写入：          │    │                 │    │ 读取：          │
│ - start_time    │    │ - request_start │    │ - start_time    │
│                 │    │   _time         │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### **3. Agent 配置**

Agent 同时配置两个生命周期回调：
- `before_agent_callback`：监控 Agent 开始执行
- `after_agent_callback`：监控 Agent 执行完成
- 使用 `InMemoryRunner`，确保回调能够正确触发

## 🧪 测试示例

### **示例输出格式**

```text
🚀 Agent LifecycleDemoAgent started at 19:15:30
⏰ Start time: 2024-01-15 19:15:30

✅ Agent LifecycleDemoAgent completed
⏱️ Duration: 1.23s
⏰ End time: 2024-01-15 19:15:31
📊 Performance: 1.23s | LifecycleDemoAgent
```

### **各项指标的含义**

- **🚀 开始时间**：Agent 开始处理请求的时间
- **✅ 完成时间**：Agent 完成处理的时间
- **⏱️ 持续时间**：总执行耗时，单位为秒
- **📊 性能**：格式化后的性能摘要

## 🔍 关键概念

### **Agent 生命周期监控**
- **执行开始**：跟踪 Agent 何时开始处理
- **执行结束**：跟踪 Agent 何时完成任务
- **性能计时**：计算总执行耗时
- **状态共享**：在回调之间传递计时数据

### **CallbackContext**
- **agent_name**：当前执行的 Agent 名称
- **invocation_id**：本次执行的唯一标识符
- **state**：在回调之间持续存在的 Session State

### **状态管理**
- 使用 `callback_context.state.to_dict()` 获取当前状态
- 使用 `callback_context.state.update()` 修改状态
- before 与 after 回调之间共享同一个状态

## 🎯 应用场景

- **性能监控**：跟踪执行耗时
- **日志记录**：记录 Agent 活动
- **数据分析**：收集使用统计信息
- **调试**：监控 Agent 行为
- **自定义逻辑**：添加前置或后置处理

## 🚨 常见错误

1. **忘记等待 Session 创建完成：**
   ```python
   # ❌ 错误
   session_service.create_session(...)

   # ✅ 正确
   await session_service.create_session(...)
   ```

2. **使用错误的回调函数签名：**
   ```python
   # ❌ 错误
   def after_agent_callback(context, result):

   # ✅ 正确
   def after_agent_callback(callback_context: CallbackContext):
   ```

3. **没有使用 InMemoryRunner：**
   ```python
   # ❌ 错误：回调不会触发
   agent.run(message)

   # ✅ 正确
   runner.run_async(...)
   ```

## ⚠️ 关键实现说明

**事件循环必须执行完成**：如果在收到 `is_final_response()` 后立即中断事件循环，`after_agent_callback` 将不会触发。

**正确模式**：让事件循环自然结束：
```python
# ❌ 错误：提前退出循环，after_agent_callback 不会执行
if event.is_final_response() and event.content:
    response_text = event.content.parts[0].text.strip()
    break  # 这会阻止 after_agent_callback 执行

# ✅ 正确：让循环自然结束
if event.is_final_response() and event.content:
    response_text = event.content.parts[0].text.strip()
    # 不要 break，确保事件循环结束并执行回调
```

这是 ADK 的一个已知行为：提前中断循环会导致清理阶段的回调无法执行。

## 🔗 后续步骤

- 继续教程 6.2：LLM 交互回调
- 尝试在不同回调之间管理状态
- 添加自定义日志或分析功能
- 为响应过慢的情况实现性能告警
