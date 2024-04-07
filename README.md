# ViT Fine-Tuning with ColossalAI

- Model: `google/vit-base-patch16-224`
- Dataset: `beans`
- Parallel Strategy: `gemini`

## Experiment Environment

- Python 3.9.19
- NVIDIA Tesla V100-SXM2-32GB * 4
- CUDA 11.0

## Steps to Run

1. Install `torch` in `conda`

```
conda install pytorch==2.2.2 torchvision==0.17.2 torchaudio==2.2.2 pytorch-cuda=11.8 -c pytorch -c nvidia
```

2. Update `gcc`

```
conda install -c conda-forge gcc=9.5.0 gxx=9.5.0
```

3. Install `ColossalAI` from scratch

```
cd ColossalAI
CUDA_EXT=1 pip install .
cd ..
```

4. Clear `torch_extensions` cache

```
rm -r  ~/.cache/colossalai/torch_extensions/
```

5. Install dependencies

```
pip install -r requirements.txt
```

6. Run demo with the following command (The terminal may look like stuck at this step, because all outputs are redirected to `output.txt`)

```
sh run_demo.sh > output.txt 2>&1
```

7. Find the results in `output.txt`
8. Run benckmark with the following command

```
sh run_benchmark.sh > benchmark_output.txt 2>&1
```

9. Find the results in `benchmark_output.txt`

## Experiment Results

### `run_demo.sh`

| Epoch | Avg Loss | Accuracy |
| ----- | -------- | -------- |
| 1     | 1.1380   | 0.8828   |
| 2     | 0.2651   | 0.9844   |
| 3     | 0.1170   | 0.9922   |

### `run_benchmark.sh`

- `batch_size = 8`

| Plugin          | Throughput | Maximum Memory Usage per GPU |
| --------------- | ---------- | ---------------------------- |
| torch_ddp       | 120.5087   | 1.75 GB                      |
| torch_ddp_fp16  | 146.9361   | 1.75 GB                      |
| low_level_zero  | 90.8185    | 696.72 MB                    |
| gemini          | 92.5829    | 331.88 MB                    |
| hybrid_parallel | 72.6727    | 417.03 MB                    |

- `batch_size = 32`

| Plugin          | Throughput | Maximum Memory Usage per GPU |
| --------------- | ---------- | ---------------------------- |
| torch_ddp       | 191.3859   | 2.13 GB                      |
| torch_ddp_fp16  | 463.2753   | 2.05 GB                      |
| low_level_zero  | 300.5291   | 890.97 MB                    |
| gemini          | 385.2396   | 523.21 MB                    |
| hybrid_parallel | 99.5983    | 431.50 MB                    |

