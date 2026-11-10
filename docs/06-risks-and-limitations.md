# Risks and Limitations

| Risk | Impact | Mitigation |
|------|--------|------------|
| Latency exceeds 6 s p95 | Fails ship gate | Measure early; limit to 2 or 3 cropped images; smaller reranker; fewer candidates |
| SigLIP weak on dense document pages | Visual-only questions score badly | Ablate ColQwen2 for manual pages (index on Colab/Kaggle given 6 GB VRAM) |
| 6 GB VRAM | Cannot co-load all models | Run Whisper and any ColQwen2 indexing offline; keep query-time models under 3 GB |
| Small corpus (about 15 manuals vs 300) | Results look better than production would | State scale in README; do not extrapolate |
| Author-written eval questions | Unconscious bias toward what the system finds | Freeze questions first; external review of a sample; blind grading |
| Only 15 visual-only questions | High variance | Report counts, not just percentages; do not claim significance |
| Synthetic audio | Not representative of real calls | Label clearly as synthetic |
| API model version drift | Results not reproducible later | Pin and record model version and date in each run |
| Copyright of manuals and photos | Legal exposure | Do not redistribute; download script and license log |
| Scope creep | Unfinished repo | Hold to the brief; log new ideas as future work |

## Stated limitations (to copy into README)
- Query-path latency only; indexing is offline
- Final answers depend on a third-party API model
- Evaluated on a small, hand-built corpus
