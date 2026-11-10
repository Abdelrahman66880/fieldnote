# Project Brief: Fieldnote

## One-line summary
A multimodal retrieval assistant that answers HVAC technicians' questions using evidence from manual pages (including charts and diagrams), job-site photos and support-call recordings, and returns the source page image so the answer can be verified.

## Problem
Field technicians need answers that often live inside a chart or wiring diagram, not in a paragraph. Text-only search misses axis values, table cells and visual symptoms.

## Target user
A field service technician standing at a unit, asking questions like "Why does this unit trip on high pressure at 95°F ambient?"

## Goal
Show that retrieval over native modalities beats a caption-only text pipeline on questions whose evidence only exists in an image, and prove it with an eval.

## In scope
- Equipment focus: residential split AC and heat pumps (TODO: confirm)
- 10 to 20 public manuals (PDF), 150 to 300 photos, 10 to 15 audio clips
- Per-modality indexing, parallel retrieval, reranking, answer with page-image citation
- Caption-only baseline for comparison
- 40-question hand-written eval set (15 visual-only)
- Simple API and a minimal UI

## Out of scope
- Real company data, user accounts, authentication
- Production deployment, scaling to 300+ manuals
- Fine-tuning any model
- Mobile app, offline mode

## Success criteria
Defined in `02-requirements.md` (ship gate) and measured per `03-eval-plan.md`.

## Constraints
- Hardware: laptop, 16 GB RAM, RTX 3060 with 6 GB VRAM
- Final answer synthesis via API (no local VLM large enough for chart reading)
- Solo project, built in roughly two weekends of focused work
- Only legally redistributable or self-made data is committed to the repo
