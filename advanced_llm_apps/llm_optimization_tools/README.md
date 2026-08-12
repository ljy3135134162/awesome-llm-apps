# 🎯 LLM 优化工具

一组用于优化 LLM 应用的工具与方法：降低成本、提升性能，并最大化整体效率。

---

## 📚 可用工具

### 🎯 [Toonify Token 优化](toonify_token_optimization/)

使用 TOON（Token-Oriented Object Notation，面向 Token 的对象表示法）格式，**将 LLM API 成本降低 30–60%**。

#### 功能
- 将 JSON 数据转换为紧凑的 TOON 格式
- 显著减少 Token 使用量
- 保留数据结构和可读性
- 降低 API 调用成本

#### 主要特性
- ✅ 与 JSON 相比，**平均减少 63.9% Token**
- ✅ 表格数据最高可**节省 73.4%**
- ✅ 人类可读格式
- ✅ 支持双向转换（JSON ↔ TOON）
- ✅ 支持 Schema 校验
- ✅ 提供交互式 Streamlit 应用

#### 快速示例
```python
from toon import encode, decode

# 原始数据（JSON 大小为 247 字节）
data = {
  "products": [
    {"id": 101, "name": "Laptop Pro", "price": 1299},
    {"id": 102, "name": "Magic Mouse", "price": 79}
  ]
}

# 转换为 TOON（98 字节，减少约 60%）
toon_str = encode(data)
# products[2]{id,name,price}:
#   101,Laptop Pro,1299
#   102,Magic Mouse,79

# 以更低成本传给 LLM
response = llm.complete(f"Analyze: {toon_str}")
```

#### 使用场景
- 📊 向 LLM 传递大型数据集
- 💰 显著降低 API 成本
- 🔄 优化上下文窗口利用率
- 📈 提升响应速度

#### 快速开始
```bash
cd toonify_token_optimization/
pip install -r requirements.txt
python quick_test.py
```

**📖 [完整文档 →](toonify_token_optimization/README.md)**

---

## 💡 为什么要优化？

### 降低成本
LLM API 的费用通常按 Token 数量计算。减少 Token，就意味着直接节省成本。

**示例节省金额（GPT-4）**：
- 1,000 次 API 调用：**节省 $2.15**
- 100,000 次 API 调用：**节省 $214.70**
- 100 万次 API 调用：**节省 $2,147.00** 💰

### 提升性能
Token 越少，处理速度通常越快，整体效率也越高。

### 上下文窗口
通过更紧凑的数据格式，可以在有限的上下文窗口中容纳更多内容。

---

## 🎯 最佳实践

### 1. 结构化数据使用紧凑格式
向 LLM 传递数据时，应优先采用高效序列化格式：
- ✅ 表格/结构化数据使用 TOON
- ✅ 简单数据集使用 CSV
- ❌ 避免包含大量无意义空白的冗长 JSON

### 2. 优化 Prompt
- 保持简洁、明确
- 删除不必要的示例
- 使用结构化格式

### 3. 批处理
- 合并相似请求
- 尽可能复用上下文
- 缓存高频响应

### 4. 选择合适的模型
- 简单任务使用较小模型
- 复杂推理任务再使用 GPT-4 等更强模型
- 根据场景考虑微调模型

---

## 📊 对比表

| 格式 | 大小 | Token | 成本（每 100 万次调用） | 最适合 |
|--------|------|--------|---------------------|----------|
| **JSON（冗长）** | 247 B | 85 | $2,550 | 兼容性 |
| **JSON（紧凑）** | 189 B | 67 | $2,010 | 常规使用 |
| **TOON** | 98 B | 39 | $1,170 | 结构化数据 |
| **CSV** | 112 B | 42 | $1,260 | 简单表格 |

*基于 GPT-4 输入价格 $0.03 / 1K Token 计算。*

---

## 🚀 后续工具（计划中）

### 计划新增

#### 📦 Prompt 压缩
在尽量保留原始语义的前提下，自动压缩长 Prompt。

#### 🗜️ 上下文优化
针对长对话进行智能上下文窗口管理。

#### 📈 Token 分析
跟踪并分析应用中的 Token 使用情况。

#### 💾 响应缓存
通过智能缓存避免重复 API 调用。

---

## 🤝 贡献

如果你有值得分享的优化方法，欢迎加入这个集合。

**贡献方式：**
1. Fork 本仓库
2. 为你的工具创建一个新目录
3. 提供 README、代码和示例
4. 提交 Pull Request

**要求：**
- 必须能够显著降低成本或提升性能
- 提供 Benchmark 和对比数据
- 提供清晰文档
- 包含使用示例

---

## 📖 其他资源

### 学习资源
- [LLM Token 基础](https://platform.openai.com/tokenizer)
- [成本优化指南](https://openai.com/pricing)
- [生产环境最佳实践](https://platform.openai.com/docs/guides/production-best-practices)

### 相关项目
- [TOON 格式规范](https://github.com/toon-format/toon)
- [Toonify 库](https://github.com/ScrapeGraphAI/toonify)

---

## 💬 支持

- 📧 有问题？可在 GitHub 提交 Issue
- 💡 有建议？欢迎分享新的优化方法
- 🌟 如果觉得有用，可以给仓库点个 Star

---

## 📄 许可证

本集合中的不同工具可能采用不同许可证，请查看各工具目录中的具体许可证信息。

---

**省成本、提速度、构建更高效的 LLM 应用。🚀💰**
