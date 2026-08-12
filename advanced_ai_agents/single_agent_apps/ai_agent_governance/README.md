# 🛡️ AI Agent 治理 - 基于策略的沙盒控制

学习如何为 AI Agent 构建治理层，通过确定性的策略约束，在危险操作真正执行之前将其拦截。

## 功能特性

- **基于策略的沙盒控制**：使用声明式策略定义 AI Agent 可以做什么、不能做什么
- **动作拦截**：在执行前捕获并验证 Agent 的操作
- **审计日志**：完整记录 Agent 行为，便于合规与调试
- **文件系统防护**：限制只允许在指定目录中读写
- **网络防护**：仅允许访问白名单中的外部 API
- **速率限制**：通过可配置限制防止 Agent 失控执行

## 工作原理

1. **策略定义**：使用 YAML 格式定义安全策略
2. **动作封装**：使用治理层包装 Agent 的工具
3. **操作拦截**：任何工具执行前，由策略引擎验证该动作
4. **决策**：动作可以被允许、拒绝，或要求人工批准
5. **审计**：所有决策都会被记录，便于合规和调试

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│   Agent     │────▶│  Governance  │────▶│    Tool     │
│  (LLM)      │     │    Layer     │     │  Execution  │
└─────────────┘     └──────────────┘     └─────────────┘
                           │
                    ┌──────▼──────┐
                    │   Policy    │
                    │   Engine    │
                    └─────────────┘
```

## 环境要求

- Python 3.8+
- OpenAI API Key（或其他 LLM Provider）
- 所需 Python 依赖（详见 `requirements.txt`）

## 安装

1. 克隆仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_agent_governance
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

## 使用方式

1. 设置 API Key：

```bash
export OPENAI_API_KEY=your-openai-api-key
```

2. 运行治理示例：

```bash
python ai_agent_governance.py
```

3. 尝试不同操作，观察策略引擎如何处理这些请求。

## 策略配置示例

```yaml
policies:
  filesystem:
    allowed_paths: ["/workspace", "/tmp"]
    denied_paths: ["/etc", "/home", "~/.ssh"]
    
  network:
    allowed_domains: ["api.openai.com", "api.github.com"]
    block_all_others: true
    
  execution:
    max_actions_per_minute: 60
    require_approval_for: ["delete_file", "execute_shell"]
    
  tools:
    allowed: ["read_file", "write_file", "web_search"]
    denied: ["execute_code", "send_email"]
```

## 输出示例

```
🛡️ AI Agent Governance Demo
============================

📋 Loading policy: workspace_sandbox.yaml

🤖 Agent request: "Read the contents of /etc/passwd"
❌ DENIED: Path '/etc/passwd' is outside allowed directories

🤖 Agent request: "Write analysis to /workspace/report.md"  
✅ ALLOWED: Action permitted by policy

🤖 Agent request: "Make HTTP request to unknown-api.com"
❌ DENIED: Domain 'unknown-api.com' not in allowlist

🤖 Agent request: "Delete /workspace/temp.txt"
⏸️ PENDING: Action requires human approval
   [Y/n]: 
```

## 技术细节

### 策略引擎

策略引擎会根据一组规则评估每个动作：

```python
class PolicyEngine:
    def evaluate(self, action: Action) -> Decision:
        # Check each policy rule
        for rule in self.rules:
            result = rule.evaluate(action)
            if result.is_terminal:
                return result
        return Decision.ALLOW
```

### 动作拦截

工具会被治理检查逻辑包装：

```python
def governed_tool(func):
    def wrapper(*args, **kwargs):
        action = Action(name=func.__name__, args=args, kwargs=kwargs)
        decision = policy_engine.evaluate(action)
        
        if decision == Decision.DENY:
            raise PolicyViolation(decision.reason)
        elif decision == Decision.REQUIRE_APPROVAL:
            if not get_human_approval(action):
                raise PolicyViolation("Human denied the action")
        
        # Log the action
        audit_log.record(action, decision)
        
        return func(*args, **kwargs)
    return wrapper
```

### 审计日志

所有动作都会连同完整上下文一起记录：

```python
{
    "timestamp": "2024-01-15T10:30:00Z",
    "action": "write_file",
    "args": {"path": "/workspace/report.md"},
    "decision": "ALLOW",
    "policy_matched": "filesystem.allowed_paths",
    "agent_id": "research-agent-001"
}
```

## 核心概念

1. **确定性安全 vs 概率性安全**：为什么策略强制执行通常比单纯依赖 Prompt Engineering 更可靠
2. **纵深防御**：通过多层验证机制增强整体安全性
3. **审计轨迹**：日志记录对于合规与调试的重要性
4. **最小权限原则**：只授予 Agent 完成任务真正需要的权限

## 扩展方向

- 为实际业务场景添加自定义策略规则
- 实现 Human-in-the-loop 人工审批工作流
- 对接外部策略管理系统
- 增加实时监控与告警机制

## 相关项目

- [LangChain](https://github.com/langchain-ai/langchain) - LLM 应用框架
- [Guardrails AI](https://github.com/guardrails-ai/guardrails) - 面向 LLM 的输入/输出校验工具
