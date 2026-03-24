# 03 Training

This repo follows an MLX LoRA workflow on Apple Silicon.

## Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install mlx-lm
```

## Base Model

Example base model:

```text
mlx-community/Qwen2.5-0.5B-Instruct-4bit
```

This model choice was not arbitrary. Earlier experiments with smaller base-style models were less stable, while the instruct variant gave the first usable rewrite behavior.

## Training Command

```bash
mlx_lm.lora \
  --model mlx-community/Qwen2.5-0.5B-Instruct-4bit \
  --train \
  --data ./data \
  --adapter-path ./qwen25_v8 \
  --iters 300 \
  --batch-size 1 \
  --grad-accumulation-steps 8 \
  --num-layers 6 \
  --learning-rate 3e-6 \
  --max-seq-length 512
```

## Notes

- `--data ./data` uses the canonical dataset folder in this repo.
- `./qwen25_v8` matches the adapter path already referenced by the app.
- If your local `mlx-lm` version uses a different training entrypoint, keep the same arguments and adapt the command name only.

## Training Lessons

- keep the task narrow and consistent
- do not expect hyperparameter changes to rescue a weak dataset
- validation behavior matters more than the lowest possible train loss
- the best checkpoint may appear before the most overfit checkpoint
