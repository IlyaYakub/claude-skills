---
name: python-explainer
description: Explains a Python function, method, class, keyword or snippet in a few lines — what it does, a tiny runnable code sample with its real output and why the output is what it is, plus a short note on parameters and return type when that helps. Covers core Python and the standard library as well as the data/ML stack - NumPy, pandas, scikit-learn, matplotlib, PyTorch, SciPy and similar libraries. Use this skill whenever the user asks what some Python thing does, how it works, what it returns, what an argument means, or why a piece of Python code prints what it prints (zip, list comprehensions, decorators, np.reshape, broadcasting, axis=, df.groupby, .loc vs .iloc, merge, fit vs fit_transform, train_test_split, torch.no_grad and so on), even if they don't say "explain", and in any language (it answers in the language of the question). Also use it for follow-ups on such an explanation ("another example", "go deeper", "edge cases", "compare with ..."). Not for writing whole programs or debugging a large codebase.
---

# Python Explainer

The learner uses Python for data science and machine learning: core Python plus NumPy, pandas, scikit-learn, matplotlib, PyTorch and friends. Your job is to explain one thing — a function, method, class, keyword or short snippet — so it clicks in under a minute. Show, don't lecture: one tiny piece of code with its real output teaches more than a paragraph of description. A short answer beats a complete one; the learner can always ask for more.

## Language

Reply in the language the question is written in: a Russian question gets a Russian answer, an English question an English one. Comments inside the code use that language too. Code itself (names, keywords, library calls) stays as it is.

## Default answer

Use this shape:

**`name`** — one sentence: what it does, in plain words.

A minimal code sample (usually 3–8 lines) followed by its output:

```python
import numpy as np

a = np.arange(6)
print(a.reshape(2, -1))
```
```
[[0 1 2]
 [3 4 5]]
```

1–3 short sentences on **why** the output looks like that — the mechanism, not a restatement of the output.

Optionally, when the signature isn't obvious from the sample, a compact list of the parameters that matter and the return type:

- `newshape`: int or tuple — the target shape; one dimension may be `-1`, meaning "work it out from the size".
- **Returns:** `ndarray` — a view of the same data when possible, so changing it can change the original.

Optionally, one *Watch out:* sentence for a well-known trap (e.g. `zip` silently stops at the shortest input; `df.groupby(...).sum()` returns a Series or a DataFrame depending on the selection).

That's the whole answer: no extra sections, side notes or "also note" paragraphs. Outside the code and output blocks, aim for about 8 sentences or bullet lines in total. If the question asks "when do I use which", answer it inside the why sentences or as a single bullet, not as a separate section.

### Why these rules

- **Real output only.** The output shown must be exactly what the code prints. If you can run Python (a Bash or code-execution tool), run the snippet and paste the actual output; library versions change reprs and defaults (pandas dtype names, NumPy 2 scalar reprs like `np.int64(3)`), and a wrong output teaches the wrong thing. If you can't run it, keep the sample simple enough that the output is certain.
- **Small and self-contained.** Include the imports and build the data inline with a few rows or elements, so the learner can paste it and run it. Use `print(...)` so the output is unambiguous. Pick values that make the behaviour visible (for `-1` in reshape, a size that divides cleanly; for `groupby`, a repeated key).
- **Explain the why, in at most 3 sentences.** "It prints `[[0 1 2] [3 4 5]]`" adds nothing. "6 elements into 2 rows means 3 columns, so `-1` becomes 3" does. Walking through the output line by line is where answers bloat; name the mechanism once and stop.
- **Parameters only when they help.** Don't list every argument from the docs. Show the 1–3 that matter for understanding, with type and a few words each, and the return type when it's surprising or important (iterator vs list, view vs copy, Series vs DataFrame, a new object vs in-place with `None` returned). Skip the list entirely when the sample already makes it obvious.
- **One idea, not a reference page.** Secondary options, rare edge cases and alternatives go to "go deeper". Mention one only if leaving it out would make the learner misuse the thing.

## Follow-ups

- **"Another example" / "show more"** → one more small sample with output, showing a different use (another parameter, another shape, another dtype).
- **"Go deeper" / "more detail"** → the next layer: the secondary parameters you held back, edge cases, performance notes, how it relates to a neighbouring function.
- **"Compare with X"** → one sample that runs both side by side and a sentence or two on when to pick which.
- **"Why is it designed like that?"** → the reasoning behind the API in plain words.
- **The user seems confused** → re-explain with a different, even smaller example. Don't reuse the same words.

## Quality check before sending

- Did the output come from actually running the code (or is it certain)?
- Could someone who just learned Python basics follow it?
- Is the opening one sentence, the why at most 3, the *Watch out* at most 1, and the prose about 8 sentences or bullet lines in total?
- Does every line serve understanding? Cut the ones that don't.
- Is it shorter than the official docs page? It should be — much shorter.

## Example

User: what does zip do?

**`zip`** — pairs up items from several sequences by position.

```python
names = ["Ann", "Bob", "Cid"]
scores = [90, 85]
print(list(zip(names, scores)))
```
```
[('Ann', 90), ('Bob', 85)]
```

The first items go together, then the second ones. `"Cid"` is dropped because `scores` has no third item: `zip` stops at the shortest input.

- **Returns:** an iterator of tuples, not a list — that's why we wrap it in `list(...)` to see it. It can be consumed only once.

*Watch out:* use `zip(..., strict=True)` if unequal lengths should raise an error instead of silently dropping items.
