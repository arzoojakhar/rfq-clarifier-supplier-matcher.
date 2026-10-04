# Evaluation results

> TEMPLATE. Run `python app/main.py --eval`, open `evals/run_log.csv`, and fill this by hand.
> Do not copy numbers you did not measure.

Model: [CHAT_MODEL] | Embeddings: [EMBED_MODEL] | Catalog: 60 synthetic suppliers | Date: [date]

## How I scored

- **Correct match:** at least one of the top 3 is a supplier that truly fits (check against `expected_behavior` in `queries.csv`).
- **Hallucination:** a supplier ID or spec in the answer that is not in the retrieved rows.
- **Right behavior:** for out-of-catalog, adversarial, and ambiguous queries, did it do what `expected_behavior` says.

## Per-query results

| ID | Type | Result (pass/fail) | What happened | Latency (s) | Tokens in/out |
|---|---|---|---|---|---|
| Q01 | vague | | | | |
| Q02 | vague | | | | |
| Q03 | detailed | | | | |
| Q04 | detailed | | | | |
| Q05 | misspelt | | | | |
| Q06 | hinglish | | | | |
| Q07 | synonym | | | | |
| Q08 | vague | | | | |
| Q09 | detailed | | | | |
| Q10 | detailed | | | | |
| Q11 | detailed | | | | |
| Q12 | detailed | | | | |
| Q13 | detailed | | | | |
| Q14 | multi-category | | | | |
| Q15 | out-of-catalog | | | | |
| Q16 | out-of-catalog | | | | |
| Q17 | impossible-spec | | | | |
| Q18 | adversarial | | | | |
| Q19 | adversarial | | | | |
| Q20 | ambiguous | | | | |

## Summary

| Metric | Result |
|---|---|
| Queries passed | [x / 20] |
| Top-3 relevance (queries with a real match) | [x%] |
| Hallucinated supplier IDs | [x] |
| Avg latency | [x s] |
| Avg tokens per query | [x in / x out] |
| Approx cost per query | [x, or free tier] |

## Failure cases

1. **[Query ID]:** [what went wrong] -> cause: [guess or finding] -> fix: [what I changed or would change]
2. **[Query ID]:** ...
3. **[Query ID]:** ...

## What I changed after the first run

- [e.g. raised MIN_SCORE from 0.5 to 0.6 because out-of-catalog queries still returned matches]
- [e.g. tightened the clarifier prompt because it asked for details already given]

## Caveats

- Eval mode runs the clarifier once without answering its questions, so it tests the "best guess" path. Test the interactive flow by hand for 3-4 queries and note what you saw.
- Synthetic catalog, 20 queries, one run each. Treat results as directional, not statistically solid.
