# ADR 0001: Use Qdrant as the vector database

- **Status:** Accepted
- **Context:** Need multiple vector types, payload filtering by modality, simple local setup.
- **Options:** Qdrant, pgvector, Chroma.
- **Decision:** Qdrant via Docker.
- **Why:** Named vectors and payload filters fit per-modality indexing; single container; fast enough at this scale.
- **Tradeoffs:** Less SQL integration than pgvector; one more service to run.
- **Revisit if:** The project needs relational joins with other data.
