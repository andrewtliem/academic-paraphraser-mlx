# 02 Dataset Design

The dataset should contain rewrite pairs, not question-answer pairs.

Target pattern:

- user message: rough, plain, or less formal technical writing
- assistant message: rewritten version in the target academic style

Recommended data types:

- paraphrase pairs
- rewrite pairs
- paragraph improvement pairs

## Why Dataset Design Mattered Most

The main lesson from the project was that dataset design mattered more than just changing hyperparameters.

The model improved when the data matched the actual task:

- input: weaker or rougher technical writing
- output: the same idea rewritten in the target academic style

When the dataset drifted toward explanation or unrelated answer generation, the model behavior also drifted.

## Dataset Evolution

The tutorial is stronger if the dataset changes are shown as a progression:

- `v1`: explanation-only dataset
- `v2`: more examples, but still limited task alignment
- `v3`: paragraph-style data
- `v6` and `v7`: rewrite/paraphrase style became the main focus
- `v8`: cleaner split into train, validation, and test sets

That progression explains why the final setup worked better. The data became more aligned with the real inference task.

## Current Format

Each JSONL row uses a `messages` array:

```json
{
  "messages": [
    {
      "role": "user",
      "content": "Rewrite the following in my academic EPON paper style..."
    },
    {
      "role": "assistant",
      "content": "Rewritten output in the target style..."
    }
  ]
}
```

## Current Split

- train: 180 examples
- valid: 30 examples
- test: 30 examples

## Quality Rules

Every pair should satisfy all of these:

1. Preserve the same technical meaning.
2. Improve the wording or tone.
3. Stay on the same topic.

Some existing rows are noisy and should be cleaned before retraining because mismatched pairs will teach unstable behavior.

## What to Clean Before Retraining

Remove or rewrite rows with these problems:

- the output changes topic
- the output adds claims not present in the input
- the output keeps the same wording without meaningful improvement
- the output sounds formal but does not preserve the original meaning
