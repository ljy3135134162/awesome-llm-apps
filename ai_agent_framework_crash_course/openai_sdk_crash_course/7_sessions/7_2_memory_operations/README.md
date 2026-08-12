# Memory 操作

本教程演示高级 Session Memory 操作，包括会话项管理、对话修正以及 Session 生命周期控制。

## 🎯 本示例展示的内容

- **get_items()**：以编程方式读取对话历史
- **add_items()**：手动向会话中添加对话项
- **pop_item()**：移除并修正最近的对话轮次
- **clear_session()**：清空当前会话历史

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **配置环境变量**：
   ```bash
   cp ../env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行示例**：
   ```python
   import asyncio
   from agent import basic_memory_operations, conversation_corrections

   # 测试基础 Memory 操作
   asyncio.run(basic_memory_operations())

   # 测试对话修正
   asyncio.run(conversation_corrections())
   ```

## 💡 核心概念

- **Memory 检查**：理解 Session 中保存的对话结构
- **手动项管理**：向会话中加入自定义对话条目
- **对话修正**：撤销或调整已经发生的交互
- **Session 清理**：重置对话历史并重新开始

## 🧪 可用操作

### 基础 Memory 操作
- 获取已有对话项
- 添加自定义对话内容
- 检查 Session 当前保存的数据

### 对话修正
- 使用 `pop_item()` 撤销最近一条记录
- 修正用户问题
- 调整后续对话流程

### Session 管理
- 清空对话历史
- 创建新的干净对话状态
- 管理 Session 生命周期

### Memory 检查
- 分析对话项结构
- 限制返回的历史条目数量
- 理解 Session Memory 的组织方式

## 🔗 后续步骤

- [基础 Session](../7_1_basic_sessions/README.md) —— 学习 Session 基础
- [多 Session 管理](../7_3_multi_sessions/README.md) —— 管理多个独立对话
