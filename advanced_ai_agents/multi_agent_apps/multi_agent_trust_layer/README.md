# 🤝 多智能体信任层——安全的智能体间通信

本项目演示如何为多智能体系统构建一层“信任层”，用于实现智能体之间的安全任务委派、信任评分与策略执行。

## 功能特性

- **智能体身份**：每个智能体都拥有可验证身份，并关联一个人类责任人
- **信任评分**：通过行为监控维护 0-1000 的信任分数
- **委派链**：在任务委派过程中通过加密机制逐级缩小权限范围
- **策略执行**：在智能体交互过程中统一执行合规与安全规则
- **审计记录**：完整记录智能体之间的通信与操作，便于追踪与审计

## 工作原理

```
┌─────────────────┐         ┌─────────────────┐
│   智能体 A       │◀───────▶│     信任层       │
│   （协调器）      │   TLS   │                 │
└─────────────────┘         │  • 身份           │
                            │  • 信任评分       │
┌─────────────────┐         │  • 委派           │
│   智能体 B       │◀───────▶│  • 策略           │
│   （专家）        │   TLS   │  • 审计           │
└─────────────────┘         └─────────────────┘
```

1. **注册**：智能体使用经过验证的身份和人类责任人进行注册
2. **建立信任**：初始信任分数根据责任人的信誉确定
3. **任务委派**：父级智能体可以将任务委派给其他智能体，并限制其权限范围
4. **监控**：系统持续记录所有操作，并动态更新信任分数
5. **策略执行**：策略引擎决定每个智能体允许执行哪些操作

## 环境要求

- Python 3.8+
- OpenAI API 密钥（也可替换为其他 LLM 提供商）
- 所需 Python 依赖包（参见 `requirements.txt`）

## 安装

1. 克隆仓库：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd advanced_ai_agents/multi_agent_apps/multi_agent_trust_layer
   ```

2. 安装所需依赖：
   ```bash
   pip install -r requirements.txt
   ```

## 使用方法

1. 设置 API 密钥：
   ```bash
   export OPENAI_API_KEY=your-openai-api-key
   ```

2. 运行信任层示例：
   ```bash
   python multi_agent_trust_layer.py
   ```

3. 观察智能体通过信任层进行交互，并查看完整的可观测记录。

## 示例：智能体委派链

```python
# 协调器智能体为专家智能体创建委派
delegation = trust_layer.create_delegation(
    from_agent="orchestrator-001",
    to_agent="researcher-002",
    scope={
        "allowed_actions": ["web_search", "summarize"],
        "max_tokens": 10000,
        "time_limit_minutes": 30,
        "allowed_domains": ["arxiv.org", "github.com"]
    },
    task_description="Research recent papers on AI safety"
)

# 研究智能体只能执行委派范围内允许的操作
result = researcher.execute_with_delegation(
    delegation=delegation,
    action="web_search",
    params={"query": "AI safety papers 2024"}
)
```

## 信任评分系统

信任分数范围为 0-1000：

| 分数范围 | 等级 | 权限 |
|-------------|-------|-------------|
| 900-1000 | 可信 | 在角色权限范围内拥有完整访问权限 |
| 700-899 | 标准 | 可执行常规操作 |
| 500-699 | 观察期 | 操作受限，并启用额外日志记录 |
| 300-499 | 受限 | 需要人工批准 |
| 0-299 | 暂停 | 不允许自主执行操作 |

### 分数变化规则

```python
# 正向行为会提高信任分数
+10: 成功完成被委派的任务
+5:  始终保持在权限范围内
+2:  提供准确的信息

# 负向行为会降低信任分数
-50: 尝试执行权限范围外的操作
-30: 提供不准确的信息
-20: 超出资源限制
-100: 安全违规
```

## 示例输出

```
🤝 多智能体信任层演示
================================

📋 正在注册智能体...
✅ 已注册：orchestrator-001（人类责任人：alice@company.com）
✅ 已注册：researcher-002（人类责任人：bob@company.com）
✅ 已注册：writer-003（人类责任人：carol@company.com）

🔐 正在创建委派链...
✅ 委派：orchestrator-001 → researcher-002
   权限范围：web_search, summarize
   时间限制：30 分钟

🤖 智能体 researcher-002 正在执行：web_search
   查询："AI safety papers 2024"
✅ 操作已允许（位于委派范围内）
   信任分数：850 → 860 (+10)

🤖 智能体 researcher-002 正在执行：send_email
❌ 操作被拒绝（不在委派范围内）
   信任分数：860 → 810 (-50)

📊 信任分数：
   orchestrator-001: 900（可信）
   researcher-002: 810（标准）
   writer-003: 850（标准）
```

## 核心概念

### 1. 智能体身份

每个智能体都有一个与人类责任人绑定的加密身份：

```python
@dataclass
class AgentIdentity:
    agent_id: str
    public_key: str
    human_sponsor: str  # 负责该智能体的人类
    organization: str
    roles: List[str]
    created_at: datetime
```

### 2. 委派链

委派可以形成链式结构，并且每一层只能进一步缩小权限范围：

```python
@dataclass  
class Delegation:
    delegation_id: str
    parent_agent: str
    child_agent: str
    scope: DelegationScope
    signature: str  # 由父级智能体签名
    parent_delegation: Optional[str]  # 链接到父级委派
```

### 3. 策略执行

策略根据智能体的信任分数和角色定义其允许执行的操作：

```python
policies:
  researcher:
    base_trust_required: 500
    allowed_actions:
      - web_search
      - read_document
      - summarize
    denied_actions:
      - execute_code
      - send_email
    resource_limits:
      max_tokens_per_hour: 100000
      max_api_calls_per_minute: 60
```

## 系统架构

```
┌────────────────────────────────────────────────────┐
│                      信任层                         │
├─────────────┬─────────────┬─────────────┬──────────┤
│  身份注册表   │  信任评分    │  委派管理器   │ 策略引擎  │
├─────────────┴─────────────┴─────────────┴──────────┤
│                    审计日志器                       │
└────────────────────────────────────────────────────┘
         ▲              ▲              ▲
         │              │              │
    ┌────┴────┐    ┌────┴────┐    ┌────┴────┐
    │ 智能体 A │    │ 智能体 B │    │ 智能体 C │
    └─────────┘    └─────────┘    └─────────┘
```

## 可扩展方向

- 为委派验证加入加密签名机制
- 实现跨组织的信誉系统
- 增加实时信任分数可视化
- 接入外部身份提供商（OAuth、SAML）
- 实现安全通信通道（mTLS）

## 相关项目

- [LangGraph](https://github.com/langchain-ai/langgraph) - 多智能体编排
- [CrewAI](https://github.com/joaomdmoura/crewAI) - 多智能体框架
- [AutoGen](https://github.com/microsoft/autogen) - 多智能体对话框架
