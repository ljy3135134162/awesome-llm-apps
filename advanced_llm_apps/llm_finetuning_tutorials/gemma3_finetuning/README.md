## 🦥 使用 Unsloth 微调 Gemma 3（简单 4-bit LoRA）

这是一个使用 Unsloth 对 Google Gemma 3 Instruct 模型进行微调的最小示例，采用 4-bit 加载 + LoRA。代码简洁、易读，并可在 CUDA GPU 上直接运行。

- **模型**：270M、1B、4B、12B、27B
- **数据集**：FineTome-100k（ShareGPT 风格多轮对话）
- **方法**：参数高效 LoRA（不是全参数微调）

参考：Unsloth 的 Gemma 3 说明：[unsloth.ai/blog/gemma3](https://unsloth.ai/blog/gemma3)

### 安装

```bash
pip install -r requirements.txt
# 或根据官方建议安装最新版 Unsloth
pip install --upgrade --force-reinstall --no-cache-dir unsloth unsloth_zoo
```

### 运行

```bash
python finetune_gemma3.py
```

输出会保存到 `finetuned_model/`。

### 脚本做了什么

1. 通过 Unsloth 的 `FastModel` 以 4-bit 量化方式加载 Gemma 3。
2. 将 LoRA 适配器挂载到 Attention / MLP 投影层。
3. 使用 Gemma 3 Chat Template 处理 FineTome-100k 数据集。
4. 使用 TRL 的 `SFTTrainer` 执行少量演示训练步骤。
5. 保存微调后的权重。

### 修改模型或设置

编辑 `finetune_gemma3.py` 文件开头的参数：

- `MODEL_NAME`（例如 `unsloth/gemma-3-270m-it`、`unsloth/gemma-3-1b-it`）
- `MAX_SEQ_LEN`、`LOAD_IN_4BIT`、`FULL_FINETUNING`

注意：4-bit / 8-bit 加载需要 CUDA GPU。在 Mac（M1/M2）上，需要关闭量化后使用 CPU / MPS，或者改用带 GPU 的机器。
