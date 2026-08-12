# 🎯 Toonify Token 优化

使用 TOON（Token-Oriented Object Notation，面向 Token 的对象表示法）格式序列化结构化数据，可将 LLM API 成本降低约 30–60%。

## 📋 概述

这个应用演示如何使用 [Toonify](https://github.com/ScrapeGraphAI/toonify)，在向大型语言模型传递结构化数据时显著降低 Token 使用量。TOON 格式在保持明确数据结构和人类可读性的同时，可达到接近 CSV 的紧凑程度。

### 主要优势

- **💰 相比 JSON，平均减少 63.9% 的 Token**
- **🎯 在最适合的场景（表格数据）中最高可节省 73.4%**
- **💵 按 GPT-4 定价估算，每 100 万次 API 请求可节省 2,147 美元**
- **📖 人类可读**
- **⚡ 额外开销极低**（典型负载下小于 1ms）

## 🚀 功能

- **JSON 与 TOON 对比**：直观看到两种格式的体积差异
- **Token 成本计算器**：计算你的实际使用场景可以节省多少成本
- **LLM 集成示例**：将优化后的数据传递给 GPT / Claude
- **真实场景示例**：商品目录、调查问卷、分析数据
- **基准测试**：测量你自己的数据压缩率

## 📦 安装

1. 安装所需依赖：

```bash
pip install -r requirements.txt
```

2. 配置 API Key（可选，仅 LLM 集成演示需要）：

```bash
export OPENAI_API_KEY='your-api-key-here'
```

## 💻 使用方法

### 基础示例

运行基础格式对比演示：

```bash
python toonify_demo.py
```

### 交互式演示

运行 Streamlit 交互应用：

```bash
streamlit run toonify_app.py
```

## 📊 格式对比

### JSON（247 字节）

```json
{
  "products": [
    {"id": 101, "name": "Laptop Pro", "price": 1299},
    {"id": 102, "name": "Magic Mouse", "price": 79},
    {"id": 103, "name": "USB-C Cable", "price": 19}
  ]
}
```

### TOON（98 字节，减少 60%）

```
products[3]{id,name,price}:
  101,Laptop Pro,1299
  102,Magic Mouse,79
  103,USB-C Cable,19
```

## 🎯 最适合的使用场景

**适合使用 TOON 的情况：**
- ✅ 向 LLM API 传递数据，需要降低 Token 成本
- ✅ 处理结构统一的表格数据
- ✅ 上下文窗口有限
- ✅ 同时重视人类可读性

**更适合使用 JSON 的情况：**
- ❌ 需要最大化兼容性
- ❌ 数据结构高度不规则或嵌套复杂
- ❌ 必须配合现有的 JSON-only 工具使用

## 💡 示例：电商商品分析

```python
from toonify import encode
import openai

# 商品数据（实际可以有数百个商品）
products = [
    {"id": 1, "name": "Laptop", "price": 1299, "stock": 45},
    {"id": 2, "name": "Mouse", "price": 79, "stock": 120},
    # ... 更多商品
]

# 转换为 TOON 格式（可节省约 60% Token）
toon_data = encode(products)

# 以更低 Token 成本发送给 LLM
response = openai.chat.completions.create(
    model="gpt-4",
    messages=[{
        "role": "user",
        "content": f"Analyze this product data:\n{toon_data}"
    }]
)
```

## 📈 性能

**基于 50 个真实数据集的基准测试：**
- 相比 JSON，平均体积减少 63.9%
- 平均 Token 减少 54.1%
- 98% 的数据集可实现 40% 以上的节省
- 编码 / 解码额外开销极低（小于 1ms）

## 🔗 资源

- **Toonify GitHub**：https://github.com/ScrapeGraphAI/toonify
- **PyPI**：https://pypi.org/project/toonify/
- **文档**：https://docs.scrapegraphai.com/services/toonify
- **格式规范**：https://github.com/toon-format/toon

## 🤝 贡献

欢迎贡献，你可以：
- 报告 Bug
- 建议新的示例
- 添加基准测试
- 改进文档

## 📄 许可证

本示例按现状提供，仅用于学习和演示。
Toonify 库使用 MIT License。

## 🙏 致谢

本项目基于 ScrapeGraphAI 团队开发的 [Toonify](https://github.com/ScrapeGraphAI/toonify) 构建。
