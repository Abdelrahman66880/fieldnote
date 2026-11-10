# Requirements

## Functional requirements

| ID | Requirement |
|----|-------------|
| FR-1 | Ingest manual PDFs, rendering each page to an image and extracting its text. |
| FR-2 | Ingest photos and index them with an image encoder. |
| FR-3 | Transcribe audio clips with timestamps and index the segments. |
| FR-4 | Every indexed chunk carries a modality tag (`page_text`, `page_image`, `photo`, `audio`). |
| FR-5 | Given a question, search the relevant modalities in parallel and merge candidates. |
| FR-6 | Rerank the merged candidate set before synthesis. |
| FR-7 | Return an answer with citations: source file, page number and a cropped image of the evidence. |
| FR-8 | Provide a caption-only baseline pipeline using the same synthesis model for a fair comparison. |
| FR-9 | Expose the pipeline through an HTTP API and a minimal UI. |

## Non-functional requirements

| ID | Requirement |
|----|-------------|
| NFR-1 | p95 latency under 6 s at k=20 (query path only). |
| NFR-2 | Every answer carries a valid citation (page number and cropped image). |
| NFR-3 | All model names, k and thresholds live in `configs/default.yaml`. |
| NFR-4 | Results reproducible from a single command given the same config and data. |
| NFR-5 | Query-time models fit in 6 GB VRAM together. |
| NFR-6 | No secrets in the repo; API key via `.env`. |

## Acceptance criteria (ship gate)

- [ ] 40-question eval set exists, 15 flagged `visual_only`
- [ ] Multimodal pipeline beats caption-only baseline on the 15 visual-only questions, shown in a table with counts
- [ ] Every answer in the eval run has a citation with page number and cropped image
- [ ] p95 latency under 6 s at k=20, measured and reported
- [ ] README contains the results table and a written tradeoff recommendation
