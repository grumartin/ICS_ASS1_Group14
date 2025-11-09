# Vision-Language Model Quantization Benchmark  

This project benchmarks **quantized vision-language models (VLMs)** by comparing the **full-precision FP16 baseline** to quantized variants.  
The goal is to measure **accuracy, efficiency, and energy trade-offs** on the **COCO 2017 validation split**.

---

## Experiment Configuration

Configuration is automatically generated and saved as a YAML file in `results/experiment_config_<timestamp>.yaml`.

Example configuration:
```yaml
experiment_name: VLM_Quantization
model_id: Qwen/Qwen2.5-VL-3B-Instruct
quantizer: bitsandbytes_linear8bitlt
precision:
  language_model: fp16
dataset:
  hf_parquet_prefix: hf://datasets/phiyodr/coco2017/
  split_file: data/validation-00000-of-00001-e3c37e369512a3aa.parquet
  limit: 5000
seed: 42
inference:
  max_new_tokens: 40
  img_size: 1024
  batch_size: 1000
```

##    Output

All results are saved under the results/ directory:

| File                                 | Description                                   |
| ------------------------------------ | --------------------------------------------- |
| `fp16_batch_<id>.csv`                   | Batched Per-image metrics for FP16 model              |
| `int8_batch_<id>.csv`                   | Batched Per-image metrics for full INT8 model         |
| `int8_vit_batch_<id>.csv`               | Batched Per-image metrics for vision-tower INT8 model |
| `summary_<timestamp>.json`           | Aggregate metrics summary                     |
| `experiment_config_<timestamp>.yaml` | Experiment configuration                      |
| `plots/`                             | Comparison plots of all metrics               |

##    Metrics

| Metric                 | Description                                                                                                                                             |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CIDEr**              | Measures caption quality against human references. |
| **Latency (s)**        | Average time to generate a caption for one image. Lower is better.                                                                                      |
| **Throughput (img/s)** | Number of images processed per second. Higher is better.                                                                                                |
| **Peak VRAM (MiB)**    | GPU memory usage. Lower values indicate better memory efficiency.                                                                                       |
| **Model Size (MB)**    | Disk footprint of each model variant. Smaller is better for deployment.                                                                                 |

# Notebook Execution Instructions

### 1. Run the Notebook
Execute **all cells in order**. The inference will automatically process the dataset in batches of approximately **`limit / 5` images** per batch.

### 2. Results Storage
All outputs are saved in the **`results`** folder. You can download this folder to keep a local copy of your results.

### 3. Resuming Inference
If you want to continue inference at a later time:

1. Re-upload the previously saved batched results into the **`results`** folder.
2. The notebook will automatically detect already computed batches and **skip them**, so only missing batches will be processed.

### 4. Notes
- Each model variant (FP16, INT8, INT8-VIT) is processed separately.
- Partial results are saved as CSV files for convenience.
- You can safely stop and restart the notebook without losing progress.

