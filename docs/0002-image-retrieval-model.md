# ADR 0002: Start with SigLIP for images, treat ColQwen2 as an upgrade

- **Status:** Proposed
- **Context:** Photos and manual pages must be retrieved by visual content; chart axis values matter.
- **Options:** SigLIP/CLIP, ColPali/ColQwen2, caption-only.
- **Decision:** SigLIP first for photos and pages. Move manual pages to ColQwen2 only if the visual-only score is poor.
- **Why:** SigLIP fits easily in 6 GB; ColQwen2 is tight and adds complexity.
- **Tradeoffs:** SigLIP is weak on dense document pages; ColQwen2 is stronger but heavier.
- **Evidence needed:** Hit@k on the 15 visual-only questions.
