# 🛡️ 教程 6：Guardrail 与验证

本教程介绍如何使用 OpenAI Agents SDK 实现输入和输出 Guardrail，构建更安全、更可靠的 AI Agent。Guardrail 可以在 Agent 执行前后检查请求和响应，并在不满足要求时自动阻断流程。

## 🎯 你将学到什么

- **Input Guardrail**：在 Agent 处理前验证和过滤用户输入
- **Output Guardrail**：在返回结果前检查和过滤 Agent 输出
- **Guardrail Agent**：使用专门的 Agent 执行验证和安全检查
- **Tripwire 机制**：验证失败时自动中止执行
- **异常处理**：正确处理 Guardrail 触发异常
- **生产安全模式**：在真实应用中构建安全验证层

## 🧠 核心概念：什么是 Guardrail？

Guardrail 是用于验证输入和输出的**自动化约束机制**。可以把它理解为 Agent 工作流中的安全检查点，用于：

- 阻止不符合要求或有风险的输入
- 阻止违反安全策略的输出
- 根据业务规则验证请求
- 执行内容、安全和合规检查
- 在验证失败时自动终止流程

## 🚀 Guardrail 核心概念

### **Input Guardrail**
在 Agent 正式处理请求前验证用户输入：

```python
@input_guardrail
async def content_filter(ctx, agent, input) -> GuardrailFunctionOutput:
    if is_inappropriate(input):
        return GuardrailFunctionOutput(
            tripwire_triggered=True,
            output_info="Content blocked for safety"
        )

    return GuardrailFunctionOutput(tripwire_triggered=False)
```

### **Output Guardrail**
在结果返回给用户之前验证 Agent 输出：

```python
@output_guardrail
async def response_filter(ctx, agent, output) -> GuardrailFunctionOutput:
    if contains_sensitive_info(output):
        return GuardrailFunctionOutput(
            tripwire_triggered=True,
            output_info="Response blocked for safety"
        )

    return GuardrailFunctionOutput(tripwire_triggered=False)
```

### **Guardrail Agent**
也可以创建专门用于验证的 Agent：

```python
validation_agent = Agent(
    name="Content Validator",
    instructions="Check content for safety violations",
    output_type=SafetyCheck
)
```

## 🧪 本教程演示内容

### **1. 数学作业检测**
- 使用 Input Guardrail 检测学术作业类请求
- 根据置信度阈值决定是否阻断
- 使用 Pydantic 结构化输出进行验证

### **2. 内容安全验证**
- 使用 Output Guardrail 检查不适当内容
- 根据严重等级进行过滤
- 当输出违反规则时自动阻断

### **3. 异常处理**
- 处理 `InputGuardrailTripwireTriggered`
- 处理 `OutputGuardrailTripwireTriggered`
- 提供更友好的错误恢复逻辑

### **4. Guardrail 集成**
- 将 Guardrail 集成到现有 Agent 工作流
- 为单个 Agent 配置多个 Guardrail
- 使用自定义业务规则进行验证

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何实现输入验证和请求过滤
- ✅ 如何创建 Output Guardrail 检查响应
- ✅ 如何构建专用 Guardrail Agent
- ✅ 如何处理 Guardrail 触发异常
- ✅ 如何构建适用于生产环境的安全验证模式

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **配置环境**：
   ```bash
   cp env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行 Guardrail 示例**：
   ```python
   import asyncio
   from agent import guardrails_example, test_input_guardrail

   asyncio.run(guardrails_example())
   asyncio.run(test_input_guardrail())
   ```

## 🧪 示例场景

### Input Guardrail 测试
- `How do I reset my password?` ✅
- `Can you solve this equation: 2x + 5 = 15?` 🚫
- `What are your product features?` ✅

### Output Guardrail 测试
- 正常客户支持响应 ✅
- 包含敏感信息的响应 🚫
- 违反策略的内容 🚫

### 异常场景
- 正确处理被阻断的请求
- 返回用户可理解的错误信息
- 记录 Guardrail 触发情况

## 🔧 常见 Guardrail 模式

### 1. **输入验证模式**
```python
@input_guardrail
async def validate_input(ctx, agent, input) -> GuardrailFunctionOutput:
    validation_result = await validate_with_ai(input)

    return GuardrailFunctionOutput(
        tripwire_triggered=validation_result.is_violation,
        output_info=validation_result.details
    )
```

### 2. **输出安全检查模式**
```python
@output_guardrail
async def safety_check(ctx, agent, output) -> GuardrailFunctionOutput:
    safety_result = await check_safety(output.response)

    return GuardrailFunctionOutput(
        tripwire_triggered=safety_result.is_unsafe,
        output_info=safety_result.reason
    )
```

### 3. **异常处理模式**
```python
try:
    result = await Runner.run(protected_agent, user_input)
    return result.final_output
except InputGuardrailTripwireTriggered:
    return "Request blocked by safety filters"
except OutputGuardrailTripwireTriggered:
    return "Response blocked for safety reasons"
```

### 4. **基于置信度的阻断**
```python
return GuardrailFunctionOutput(
    tripwire_triggered=violation_detected and confidence > 0.7,
    output_info={
        "confidence": confidence,
        "reason": reason
    }
)
```

## 💡 Guardrail 最佳实践

- **分层防护**：同时使用输入和输出 Guardrail
- **合理设置阈值**：避免过多误拦截或漏拦截
- **清晰错误信息**：向用户说明请求为何无法继续
- **性能优化**：避免使用过于昂贵的验证流程
- **监控与日志**：记录 Guardrail 触发情况用于后续分析

## 🔧 高级模式

### **多级验证**
```python
agent = Agent(
    name="Protected Agent",
    input_guardrails=[
        content_filter,
        spam_detector,
        policy_checker
    ],
    output_guardrails=[
        safety_validator,
        privacy_filter
    ]
)
```

### **上下文感知 Guardrail**
```python
@input_guardrail
async def user_context_validator(
    ctx: RunContextWrapper[UserInfo],
    agent,
    input
):
    user = ctx.context

    if user.permission_level < required_level:
        return GuardrailFunctionOutput(
            tripwire_triggered=True
        )
```

### **业务规则验证**
```python
@input_guardrail
async def business_rules(ctx, agent, input) -> GuardrailFunctionOutput:
    if violates_business_rules(input):
        return GuardrailFunctionOutput(
            tripwire_triggered=True,
            output_info="Request violates business policies"
        )
```

## 🚨 常见问题

- **过度阻断**：阈值过低可能导致正常请求被拒绝
- **阻断不足**：阈值过高可能遗漏不符合要求的内容
- **性能影响**：复杂验证可能显著增加延迟
- **误判问题**：验证模型或规则设计不佳会导致错误判断

## 💡 实用建议

- 为 Guardrail 建立完整测试集
- 监控误判率和漏判率
- 根据真实使用数据持续调整规则
- 对新增 Guardrail 分阶段上线并观察效果
- 对重要业务场景保留明确的审计记录

## 🔗 生产环境考虑

### **可扩展性**
- 使用高效的验证模型
- 对重复验证结果进行缓存
- 使用异步验证降低阻塞影响

### **监控**
- 记录所有 Guardrail 决策
- 分析违规类型和趋势
- 持续监控验证流程的性能成本

### **合规**
- 根据法规和组织要求设置 Guardrail
- 对关键验证决策建立审计日志
- 定期复查并更新规则

## 🔗 后续步骤

完成本教程后，可以继续：
- **[教程 7：Sessions](../7_sessions/README.md)** —— 将安全机制与对话记忆结合
- **[教程 8：Handoff 与委派](../8_handoffs_delegation/README.md)** —— 在多 Agent 工作流中使用验证机制
- **[教程 9：多 Agent 编排](../9_multi_agent_orchestration/README.md)** —— 在复杂 Agent 系统中应用安全控制
