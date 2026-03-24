# 01 Overview

This project fine-tunes a Qwen instruct model with MLX to learn a writing style rather than teach new knowledge.

The real goal is not "teach EPON." The goal is to teach a Qwen model to rewrite technical sentences in your own academic writing style.

In this repo, the target task is:

- take technical EPON text as input
- rewrite it in a more formal academic style
- preserve the original technical meaning

That framing matters because it changes the whole pipeline:

- the dataset should contain rewrite pairs, not fact-heavy QA
- the evaluation should focus on style quality and meaning preservation
- the production API should use a narrow rewrite prompt instead of a generic chat interface

Core components:

- [`app.py`](/Users/andrewtannyliem/Documents/qwen-lora/app.py): FastAPI wrapper around `mlx_lm.generate`
- [`frontend/rewrite_frontend.html`](/Users/andrewtannyliem/Documents/qwen-lora/frontend/rewrite_frontend.html): browser playground
- [`data/train.jsonl`](/Users/andrewtannyliem/Documents/qwen-lora/data/train.jsonl): training split
- [`data/valid.jsonl`](/Users/andrewtannyliem/Documents/qwen-lora/data/valid.jsonl): validation split
- [`data/test.jsonl`](/Users/andrewtannyliem/Documents/qwen-lora/data/test.jsonl): test split

The workflow is simple:

1. Build rewrite-style examples.
2. Train a LoRA adapter with MLX.
3. Compare the base model against the fine-tuned model.
4. Serve the adapter locally through FastAPI.
5. Test the model from the browser UI or API.

## Prerequisites

This workflow assumes:

- Apple Silicon Mac
- Python virtual environment
- `mlx-lm`
- access to the target model on Hugging Face
- a small instruct model already available in MLX format

The model used in this repo is:

```text
mlx-community/Qwen2.5-0.5B-Instruct-4bit
```

## Why an Instruct Model

One of the most important findings from the project is that model choice mattered early.

- small base models were unstable
- the instruct model was the first option that produced usable rewrite behavior
- once the base model behavior became reasonable, dataset quality became the next bottleneck

That is why this tutorial is centered on a Qwen instruct model rather than a base model.

## What Did Not Work

This tutorial is more useful if the failures are explicit.

- tiny base models produced unstable outputs
- generic QA-style data did not transfer writing style well
- pushing training loss too low led to overfitting instead of better rewrites
- deployment paths that were too generic did not fit the narrow rewrite use case as cleanly as a custom wrapper
