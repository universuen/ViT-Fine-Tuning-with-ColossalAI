# ViT Fine-Tuning with ColossalAI
- Model: `google/vit-base-patch16-224`
- Dataset: `beans`
- Parallel Strategy: `gemini`

## Experiment Environment
- Python 3.9.19
- NVIDIA Tesla V100-SXM2-32GB * 4
- CUDA 11.0

## Steps to Run
1. Install `torch` on conda
```
conda install pytorch==1.12.0 torchvision==0.13.0 torchaudio==0.12.0 cudatoolkit=11.3 -c pytorch
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
6. Run with the following command (The terminal may look like stuck at this step, because all outputs are redirected to `output.txt`)
```
sh run_demo.sh > output.txt 2>&1
```
7. Find the results in `output.txt`

## Experiment Results
| Epoch | Avg Loss | Accuracy |
|-------|----------|----------|
| 1     | 1.2134   | 0.8828   |
| 2     | 0.3782   | 0.9844   |
| 3     | 0.3759   | 0.9688   |

