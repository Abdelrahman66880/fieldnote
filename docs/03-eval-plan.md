# Eval Plan

The eval is built before the pipeline. Nothing is claimed that this plan does not measure.

## Question set
- 40 questions written by hand, stored in `eval/questions.jsonl`
- 25 `text_ok`: answerable from page text alone
- 15 `visual_only`: evidence exists only in a chart, table image, diagram or photo (e.g. "At 95°F ambient, what head pressure is expected for model X?")
- Spread across all modalities; include at least 3 audio-grounded and 5 photo-grounded questions
- Have at least one other person review 10 questions for natural phrasing

## Question schema
```json
{
  "id": "q001",
  "question": "...",
  "category": "visual_only",
  "modality_needed": ["page_image"],
  "gold_source": "carrier_xyz.pdf",
  "gold_pages": [112],
  "gold_answer": "...",
  "answer_type": "number|text|yes_no"
}
```

## Metrics

| Metric | Level | How |
|--------|-------|-----|
| Hit@k (gold page in top k) | Retrieval | Automatic, from `gold_pages` |
| MRR | Retrieval | Automatic |
| Answer correctness | End to end | Rubric: correct / partially / wrong. Numbers must match within the stated tolerance. Human-graded, blind to which pipeline produced it |
| Citation validity | End to end | Cited page equals a gold page and the crop shows the evidence |
| Latency p50 / p95 | System | 3 runs per question, query path only |
| Cost per query | System | Tokens and image count logged |

Retrieval and generation are reported separately so failures are attributable.

## Baseline (caption-only)
- Caption every page image and photo with a vision model, embed the captions with the text embedding model, transcribe audio to text
- Same reranker, same k, same synthesis model, same prompt as the multimodal pipeline
- Only the indexing and retrieval strategy differs

## Procedure
1. Freeze the question set before running either pipeline. Do not edit questions after seeing results.
2. Run baseline, then multimodal, with identical configs except the variable under test.
3. Grade answers blind (shuffle and hide the pipeline label).
4. Log every run in `docs/experiment-log.md` with config, date and results.

## Reporting
- Report counts, not just percentages (e.g. 11/15 vs 5/15)
- Per-category table: `text_ok` vs `visual_only`
- Error analysis: list the failing questions and why
- State limitations: small corpus, author-written questions, one synthesis model version (pin and record it)

## Ablations (if time)
- SigLIP vs ColQwen2 for manual pages
- With vs without reranker
- k = 10, 20, 40
