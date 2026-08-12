# 🩻 医学影像诊断 Agent

这是一个基于 Agno 构建、由 Gemini 2.0 Flash 驱动的医学影像诊断 Agent，可对多种医学扫描影像进行 AI 辅助分析。该 Agent 以医学影像诊断专家的角色分析不同类型的医学图片和视频，并提供详细的诊断洞察与解释。

## 功能特性

- **全面的影像分析**
  - 识别影像类型（X 光、MRI、CT、超声）
  - 检测解剖区域
  - 提取关键发现与观察结果
  - 检测潜在异常
  - 评估影像质量
  - 提供相关研究与参考信息

## 运行方式

1. **配置环境**

```bash
# 克隆仓库
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd starter_ai_agents/ai_medical_imaging_agent

# 安装依赖
pip install -r requirements.txt
```

2. **配置 API Key**
   - 从 [Google AI Studio](https://aistudio.google.com) 获取 Google API Key。

3. **运行应用**

```bash
streamlit run ai_medical_imaging.py
```

## 分析内容

- **影像类型与区域**
  - 识别医学影像模态
  - 指定对应解剖区域

- **关键发现**
  - 系统列出观察结果
  - 详细描述影像表现
  - 标记可能存在的异常

- **诊断评估**
  - 对潜在诊断进行排序
  - 提供鉴别诊断
  - 评估可能的严重程度

- **面向患者的解释**
  - 使用更易理解的术语
  - 提供基于基本原理的详细说明
  - 指出影像中的视觉参考位置

## 说明

- 使用 Gemini 2.0 Flash 进行分析
- 需要稳定的互联网连接
- Google 提供一定额度的免费 API 请求；具体额度请以当前官方政策为准
- 仅用于教育和开发用途
- 不能替代专业医学诊断

## 免责声明

本工具仅用于教育和信息参考。所有分析结果都应由具备资质的医疗专业人员复核。请勿仅依据本工具的分析结果作出医疗决策。
