(fp8)=

# FP8量化

## W8A8-FP8 Dynamic量化

activation和weight均采用per-tensor量化方式，weight离线做fp8量化，activation做动态在线量化。

运行示例如下：

```shell
python3 tools/run.py -c configs/qwen2_5/fp8_dynamic/qwen2_5-7b_instruct_fp8_dynamic.yaml
```

该配置文件中，量化相关参数如下：
- `name`：压缩策略，选填量化`quantization`。
- `quantization.name`：压缩算法选填`fp8_dynamic`。
- `quantization.bits`：fp8量化对应填写8bit。
- `quantization.quant_method`：主要指定权重和激活的量化粒度为`per-tensor`。
- `quantization.ignore_layers`：需要忽略不进行量化的线性层。

```yaml
compression:
  name: quantization
  quantization:
    name: fp8_dynamic     # Supported: fp8_static, fp8_dynamic, int4_awq, int4_gptq, int8_dynamic
    bits: 8               # Quantization bits
    quant_method:
      weight: "per-tensor"
      activation: "per-tensor"
    ignore_layers:         # Skip quantization for these layers
      - "lm_head"
      - "model.embed_tokens"
```


## W8A8-FP8 Static量化

同样activation和weight均采用per-tensor量化方式，weight和activation均为离线做fp8量化。

运行示例如下：

```shell
python3 tools/run.py -c configs/qwen2_5/fp8_static/qwen2_5-7b_instruct_fp8_static.yaml
```

该配置文件中，压缩算法`quantization.name`选填`fp8_static`，其余量化配置与fp8动态量化相同。

激活静态量化需要指定校准数据集，例如使用`sharegpt`数据集：

```yaml
dataset:
  name: TextDataset
  data_path: ./dataset/sharegpt_gpt4/sharegpt_gpt4_256.jsonl
  max_seq_length: 4096
  num_samples: 256
  batch_size: 1
```

数据集相关参数如下：
- `name`：校准数据集类型，文生文任务选择`TextDataset`。
- `data_path`：校准数据JSONL文件位置。
- `max_seq_length`：校准截断最大上下文长度。
- `num_samples`：校准最大样本个数。
- `batch_size`：校准批量大小。

支持数据格式详见[数据准备文档](../design/prepare_dataset.md)。


## 产出模型

配置文件`config.json`：

```json
"quantization_config": {
    "activation_scheme": "dynamic",
    "ignored_layers": ["lm_head"],
    "quant_method": "fp8"
}
```

可参阅[vLLM FP8文档](https://docs.vllm.ai/en/stable/features/quantization/fp8.html)加载FP8量化模型配置要求。