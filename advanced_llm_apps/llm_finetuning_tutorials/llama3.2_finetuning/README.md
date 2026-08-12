## 🦙 用 30 行 Python 微调 Llama 3.2

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/fine-tune-llama-3-2-for-free-in-30-lines-of-python-code)，通过详细的代码讲解、说明和最佳实践，从零开始完成这个项目。**

该脚本演示如何使用 [Unsloth](https://unsloth.ai/) 库微调 Llama 3.2 模型。Unsloth 可以让微调流程更加简单、高效。你可以在 Google Colab 中免费运行该示例，用于微调 1B 和 3B 规模的 Llama 模型。

### 功能

- 使用 Unsloth 库微调 Llama 3.2 模型
- 使用低秩适配（LoRA）实现高效微调
- 使用 FineTome-100k 数据集进行训练
- 支持配置不同模型规模（1B 和 3B）

### 安装

1. 克隆仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/llm_finetuning_tutorials/llama3.2_finetuning
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

## 使用方法

1. 在 Google Colab 或你常用的 Python 环境中打开脚本。

2. 运行脚本开始微调：

```bash
# 运行完整脚本
python finetune_llama3.2.py
```

3. 微调后的模型会保存在 `finetuned_model` 目录中。

## 工作原理

1. **模型加载**：脚本使用 Unsloth 的 `FastLanguageModel` 加载 Llama 3.2 3B Instruct 模型。

2. **LoRA 配置**：对模型中的特定层应用低秩适配，以更高效地进行微调。

3. **数据准备**：加载 FineTome-100k 数据集，并使用聊天模板进行预处理。

4. **训练配置**：使用指定训练参数配置 `SFTTrainer`。

5. **微调**：在处理后的数据集上训练模型。

6. **模型保存**：将微调后的模型保存到磁盘。

## 配置

你可以在脚本中修改以下参数：

- `model_name`：如需使用 1B 模型，可改为 `unsloth/Llama-3.1-1B-Instruct`
- `max_seq_length`：调整最大序列长度
- `r`：LoRA Rank
- `TrainingArguments` 中的训练超参数

## 自定义

- 如需使用其他数据集，可以将 `load_dataset` 调用替换为目标数据集。
- 可以调整 LoRA 配置中的 `target_modules`，选择微调模型中的不同层。
- 如果采用不同的对话格式，可以修改 `get_chat_template` 中的聊天模板。

## 在 Google Colab 中运行

1. 新建一个 Google Colab Notebook。
2. 将完整脚本复制到代码单元格中。
3. 在最前面添加一个单元格安装依赖：

```
!pip install torch transformers datasets trl unsloth
```

4. 依次运行单元格即可开始微调。

## 注意事项

- 该脚本针对 Google Colab 免费套餐进行了优化，可以利用其提供的 GPU。
- 微调所需时间取决于模型规模和可用计算资源。
- 请确保 Colab 实例有足够的存储空间保存微调后的模型。
