# 04 Evaluation

The evaluation target is not factual knowledge. It is style quality with meaning preservation.

## Compare

Compare:

- base model output
- fine-tuned adapter output

Check for:

- more technical tone
- less generic wording
- preserved meaning
- more consistent EPON-style phrasing
- fewer unstable or off-topic completions

## Practical Method

1. Use held-out inputs from [`data/test.jsonl`](/Users/andrewtannyliem/Documents/qwen-lora/data/test.jsonl).
2. Generate outputs with the base model only.
3. Generate outputs again with the adapter enabled.
4. Record examples in [`examples/base_vs_finetuned.md`](/Users/andrewtannyliem/Documents/qwen-lora/examples/base_vs_finetuned.md).

## Main Evaluation Lesson

Lower train loss is not automatically better.

One of the main lessons from the project is that the best qualitative behavior appeared before deep overfitting. A later checkpoint can have lower loss while producing less reliable rewrites.

That means checkpoint selection should be based on side-by-side output review, not only training metrics.

## Failure Signs

- topic drift
- unrelated claims
- added details not present in the source
- no visible style improvement

## What Did Not Work

- assuming lower loss always meant better style transfer
- evaluating only on training metrics
- ignoring small topic shifts because the output sounded academic
