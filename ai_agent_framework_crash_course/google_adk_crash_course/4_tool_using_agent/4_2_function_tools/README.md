# ⚡ 函数工具

函数工具是由你自行创建并集成到 Agent 中的**自定义 Python 函数**。这是为 Agent 添加特定能力时最灵活、也最常用的方式之一。

## 🎯 你将学到什么

- **创建函数工具**：将自定义 Python 函数构建为工具
- **工具注册**：了解如何将函数注册到 Agent
- **参数处理**：管理工具的输入和输出
- **错误处理**：为工具实现稳健的异常管理
- **最佳实践**：掌握高效函数工具的设计模式

## 🧠 核心概念：函数工具

函数工具是具有一些特殊特征的 **Python 函数**：
- **清晰的 Docstring**：帮助 Agent 判断何时调用工具
- **类型注解**：明确输入和输出规范
- **返回字典**：提供结构化、信息充分的结果
- **错误处理**：能够优雅地处理失败情况

### 主要优势
- ✅ **高度灵活**：可以实现几乎任何所需功能
- ✅ **易于集成**：本质上就是普通 Python 函数
- ✅ **完全可控**：可以精确控制工具行为
- ✅ **易于调试**：方便独立测试和排查问题

## 🔧 函数工具要求

### 1. **描述清晰的 Docstring**
```python
def calculate_compound_interest(principal: float, rate: float, years: int) -> dict:
    """
    Calculate compound interest for an investment.

    Use this function when users ask about investment growth,
    compound interest calculations, or future value of investments.

    Args:
        principal: Initial investment amount
        rate: Annual interest rate (as decimal, e.g., 0.05 for 5%)
        years: Number of years to compound

    Returns:
        Dictionary with calculation results and breakdown
    """
```

### 2. **类型注解**
- 始终指定参数类型
- 包含返回值类型注解
- 使用合适的 Python 类型，如 `str`、`int`、`float`、`dict`、`list`

### 3. **结构化返回值**
```python
return {
    "result": final_amount,
    "calculation_breakdown": {
        "principal": principal,
        "rate": rate,
        "years": years,
        "total_interest": total_interest
    },
    "status": "success"
}
```

### 4. **错误处理**
```python
try:
    # Tool logic here
    return {"result": result, "status": "success"}
except ValueError as e:
    return {"error": str(e), "status": "error"}
```

## 🚀 教程示例

本节包含两个实际实现：

### 📍 **计算器 Agent**
**位置**：`./calculator_agent/`
- **数学运算**：基础算术、复利、百分比计算
- **单位转换**：摄氏度、华氏度、开尔文温度转换
- **统计分析**：数据集的平均数、中位数、众数、标准差
- **金融计算**：投资增长和复利预测
- **数字工具**：舍入、格式化以及数学表达式处理

### 📍 **实用工具 Agent**
**位置**：`./utility_agent/`
- **文本处理**：字数统计、大小写转换、文本变换
- **数据提取**：提取电子邮件、URL，以及词频分析
- **日期/时间操作**：格式转换、日期差、年龄计算
- **数据工具**：UUID 生成、文本哈希、Base64 编解码
- **验证工具**：URL 验证、JSON 格式化和验证

## 📁 项目结构

```text
4_2_function_tools/
├── README.md                    # 本文件：函数工具指南
├── requirements.txt             # 函数工具依赖
├── .env.example                 # 共享环境变量模板
├── calculator_agent/            # 数学工具实现
│   ├── __init__.py
│   ├── agent.py                 # 使用自定义工具的计算器 Agent
│   └── tools.py                 # 数学函数工具
└── utility_agent/               # 实用工具实现
    ├── __init__.py
    ├── agent.py                 # 使用多种工具的实用 Agent
    └── tools.py                 # 文本、日期/时间和数据工具
```

## 🎯 学习目标

完成本节后，你将理解：
- ✅ 如何将自定义 Python 函数创建为工具
- ✅ 工具设计与文档编写的最佳实践
- ✅ 如何有效处理参数和返回值
- ✅ 错误处理与输入验证策略
- ✅ 何时使用函数工具以及何时选择其他方案

## 🚀 快速开始

1. **配置环境**：
   ```bash
   cd 4_2_function_tools

   # 复制环境变量模板
   cp env.example .env

   # 编辑 .env 并添加 Google AI API Key
   # API Key 获取地址：https://aistudio.google.com/
   ```

2. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

3. **运行 Agent**：
   ```bash
   # 启动 ADK Web 界面
   adk web

   # 在 Web 界面中选择：
   # - calculator_agent：数学计算和转换
   # - utility_agent：文本、日期/时间和数据工具
   ```

4. **尝试 Agent**：
   - **Calculator Agent**：`Calculate 15% of 200`、`Convert 100°F to Celsius`、`Find statistics for [1,2,3,4,5]`
   - **Utility Agent**：`Count words in this text`、`Format date 2023-12-25`、`Generate a UUID`

5. **创建自己的工具**：根据你的实际场景构建自定义函数工具。

## 💡 实用建议

- **一个工具只做一件事**：每个函数应专注于单一职责
- **完善 Docstring**：工具说明对 Agent 正确理解和调用至关重要
- **验证输入**：始终检查函数参数是否合法
- **返回字典**：结构化结果更容易被 Agent 和应用处理
- **独立测试**：先单独测试工具，再集成进 Agent

## 🔧 常见函数工具模式

### 1. **简单计算器模式**
```python
def add_numbers(a: float, b: float) -> dict:
    """Add two numbers together."""
    return {"result": a + b, "operation": "addition"}
```

### 2. **数据处理模式**
```python
def analyze_text(text: str) -> dict:
    """Analyze text for word count, sentiment, etc."""
    return {
        "word_count": len(text.split()),
        "character_count": len(text),
        "sentiment": "neutral"  # Placeholder
    }
```

### 3. **API 集成模式**
```python
def get_weather(city: str) -> dict:
    """Get weather information for a city."""
    try:
        # API call logic here
        return {"temperature": 72, "condition": "sunny"}
    except Exception as e:
        return {"error": str(e), "status": "failed"}
```

### 4. **转换工具模式**
```python
def convert_temperature(temp: float, from_unit: str, to_unit: str) -> dict:
    """Convert temperature between units."""
    # Conversion logic
    return {
        "original": {"value": temp, "unit": from_unit},
        "converted": {"value": converted_temp, "unit": to_unit}
    }
```

## 🚨 重要说明

- **不要使用默认参数**：ADK 不支持默认参数
- **返回字典**：应始终返回结构化数据
- **错误处理**：需要实现适当的异常处理
- **文档说明**：编写清晰且有帮助的 Docstring
- **测试**：添加到 Agent 前先独立测试函数

## 🔧 常见应用场景

### 数学工具（Calculator Agent）
- 基础算术运算和表达式
- 统计计算（平均数、中位数、众数、标准差）
- 金融计算（复利、百分比）
- 单位转换（温度、测量单位）
- 数字格式化和舍入

### 文本处理工具（Utility Agent）
- 单词数和字符数统计
- 大小写转换和文本变换
- 从文本中提取电子邮件和 URL
- 词频分析
- 字符串处理和格式化

### 日期/时间工具（Utility Agent）
- 日期格式转换
- 年龄计算和日期差
- 时区处理
- 持续时间计算
- 日期解析和验证

### 数据工具（Utility Agent）
- 生成 UUID 唯一标识符
- 使用多种算法进行文本哈希
- Base64 编码和解码
- URL 验证和解析
- JSON 格式化和验证

### 集成工具
- API 调用和外部服务集成
- 数据库查询和数据检索
- 文件操作和数据处理
- 自定义业务逻辑实现
