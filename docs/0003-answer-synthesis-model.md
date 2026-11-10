# ADR 0003: Use an API vision model for final synthesis

- **Status:** Accepted
- **Context:** 6 GB VRAM; answer quality depends on reading chart values.
- **Options:** Claude via API, Qwen2.5-VL-3B 4-bit locally.
- **Decision:** API for headline results; optional local run as an ablation.
- **Why:** Local 3B models read charts poorly and would depress the numbers for reasons unrelated to retrieval.
- **Tradeoffs:** Cost, network latency, dependence on a model version (pin it).
- **Revisit if:** A larger GPU becomes available.
