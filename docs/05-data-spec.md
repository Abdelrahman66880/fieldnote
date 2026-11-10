# Data Spec

## Sources

| Modality | Source | Target size | License handling |
|----------|--------|-------------|------------------|
| Manuals | Public manufacturer PDFs (TODO: choose manufacturers) | 10 to 20 | Not committed. `scripts/download_manuals.py` plus a URL list |
| Photos | Own photos, Wikimedia Commons, Kaggle | 150 to 300 | Record license per image in `data/photos/LICENSES.csv`. No faces or house numbers |
| Audio | Self-recorded or TTS | 10 to 15 clips | Labelled **synthetic** in README |

## Directory layout
```
data/
  manuals/   <manufacturer>_<model>_<doctype>.pdf
  pages/     <manual_stem>_p<page:04d>.png
  photos/    photo_<id:04d>.jpg
  audio/     call_<id:02d>.wav
```
All of `data/` is gitignored except `.gitkeep` files.

## Chunk metadata (Qdrant payload)

| Field | Type | Notes |
|-------|------|-------|
| `id` | string | Stable, derived from source and page or segment |
| `modality` | enum | `page_text`, `page_image`, `photo`, `audio` |
| `source` | string | File name |
| `page` | int or null | Manual pages only |
| `start_s`, `end_s` | float or null | Audio only |
| `image_path` | string or null | Path to rendered page or photo |
| `text` | string or null | Extracted text or transcript segment |
| `equipment` | string or null | Brand / model if known |
| `license` | string | Source license |
| `synthetic` | bool | True for generated audio |

## Rules
- Render pages at a fixed DPI (TODO: choose, e.g. 150) and record it in config
- Never edit source files; derive everything by script
- Every derived file must be reproducible from `scripts/build_index.py`
