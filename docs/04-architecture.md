# Architecture

## Overview
Two phases: offline indexing (runs once, may use heavy models) and online query (must meet the latency gate).

## Indexing flow
```
manuals/*.pdf --> render pages (PyMuPDF) --+--> page image  --> SigLIP    --> vector (image)
                  extract text -----------+--> page text   --> bge-base  --> vector (text)
photos/*.jpg ------------------------------> SigLIP --> vector (image)
audio/*.wav  --> faster-whisper --> segments --> bge-base --> vector (text)

All vectors + payload --> Qdrant
```

## Query flow
```
question
   |
   +--> embed (bge)    --> search text vectors  (page_text, audio) --+
   +--> embed (SigLIP text tower) --> search image vectors           +--> merge candidates
                         (page_image, photo)                         |
                                                                      v
                                                          rerank --> top N
                                                                      v
                                          vision LLM (API) with page image crops
                                                                      v
                                          answer + citations (file, page, crop)
```

Fusion is **late fusion**: each modality is searched with its own encoder, then results are merged.

## Qdrant design (first draft)
- One collection `fieldnote`, named vectors: `text` (768-d, bge-base) and `image` (SigLIP dim)
- Payload per point: see `05-data-spec.md`
- Modality filters applied at search time

## Components

| Component | Module |
|-----------|--------|
| PDF rendering and text extraction | `src/fieldnote/ingest/pdf_pages.py` |
| Photo ingest | `src/fieldnote/ingest/images.py` |
| Audio ingest | `src/fieldnote/ingest/audio.py` |
| Embedding | `src/fieldnote/index/embed.py` |
| Collection setup | `src/fieldnote/index/qdrant_setup.py` |
| Parallel search | `src/fieldnote/retrieve/parallel_search.py` |
| Reranker | `src/fieldnote/retrieve/rerank.py` |
| Synthesis | `src/fieldnote/answer/synthesize.py` |
| API | `src/fieldnote/api.py` |
| Baseline | `baseline/caption_pipeline.py` |

## Latency budget (target p95 < 6 s)

| Stage | Budget |
|-------|--------|
| Embed query + vector search | 0.2 s |
| Rerank | 0.3 s |
| Image crop and prep | 0.2 s |
| Vision LLM call (1 to 3 images) | 2 to 4 s |
| Margin | rest |

## Open design question
A text cross-encoder cannot score image candidates directly. Options: (a) rerank only text candidates and merge image candidates by normalised score or reciprocal rank fusion, (b) rerank image candidates through their extracted page text, (c) let the vision LLM choose among top pages. Decide after measuring baseline results, and record it as an ADR.
