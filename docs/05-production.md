# 05 Production

This repo includes a local inference and demo layer.

The production goal here is narrow and practical: take a fixed rewrite task and expose it through a small local API.

## API

Start the FastAPI server:

```bash
uvicorn app:app --reload
```

Default local endpoint:

```text
http://127.0.0.1:8000
```

Useful routes:

- `GET /`
- `GET /health`
- `POST /rewrite`
- `GET /docs`

## Why Use a Custom FastAPI Wrapper

For this project, a custom wrapper is easier to reason about than a generic chat-serving layer.

It gives direct control over:

- the fixed rewrite instruction
- adapter path selection
- request validation
- generation limits
- the exact response shape used by the frontend

## Environment Variables

The app reads:

- `MLX_BASE_MODEL`
- `MLX_ADAPTER_PATH`
- `MLX_BIN`
- `MAX_INPUT_CHARS`
- `DEFAULT_MAX_TOKENS`
- `DEFAULT_TEMP`
- `DEFAULT_TOP_P`

Example:

```bash
export MLX_BASE_MODEL=mlx-community/Qwen2.5-0.5B-Instruct-4bit
export MLX_ADAPTER_PATH=/absolute/path/to/your/adapter
uvicorn app:app --reload
```

## Frontend

Use the browser UI at [`frontend/rewrite_frontend.html`](/Users/andrewtannyliem/Documents/qwen-lora/frontend/rewrite_frontend.html) after the API is running.

The page provides:

- input/output comparison
- generation controls
- sample EPON inputs
- a short tutorial flow for local testing

## Production Lessons

- keep the task narrow
- keep the prompt consistent with training
- expose only the parameters you actually want to tune
- prefer simple infrastructure first, then optimize later
