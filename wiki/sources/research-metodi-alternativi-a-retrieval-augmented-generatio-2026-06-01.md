---
type: source
title: "Research: Metodi alternativi a RAG per grounding LLM su domini specifici"
created: 2026-06-01
updated: 2026-06-01
tags: [research, rag, grounding, llm]
related: [retrieval-augmented-generation, fine-tuning, continued-pretraining, graphrag, raft]
sources: ["research-metodi-alternativi-a-retrieval-augmented-generatio-2026-06-01.md"]
source_path: "raw/sources/research-metodi-alternativi-a-retrieval-augmented-generatio-2026-06-01.md"
---

# Research: Metodi alternativi a RAG per grounding LLM su domini specifici

Ricerca deep-research (25 fonti, 5 query) che esplora alternative e complementi al [[retrieval-augmented-generation|RAG]] per ancorare LLM a conoscenza di dominio. Copre: fine-tuning, continued pretraining, GraphRAG, long-context, in-context RALM, RAFT e approcci ibridi.

## Metodi coperti

- **[[Fine-Tuning]]** — addestramento supervisionato su dati di dominio
- **[[continued-pretraining|Continued Pretraining]]** — pre-training aggiuntivo non supervisionato su corpus di dominio
- **[[GraphRAG]]** — RAG aumentato con knowledge graph per reasoning multi-hop
- **[[RAFT]]** — approccio ibrido che addestra il modello a integrare retrieval e generazione
- **[[long-context-grounding|Long-Context Grounding]]** — alternativa che sfrutta finestre di contesto estese senza retrieval
- **[[in-context-ralm|In-Context RALM]]** — grounding via prompt senza modificare l'architettura del modello

## Finding principali

1. L'ibrido FT+RAG è la strategia più performante nella maggior parte dei benchmark
2. Il CPT è superiore per deep domain knowledge ma ha costi elevati
3. GraphRAG eccelle nel multi-hop reasoning rispetto al RAG vettoriale puro
4. Long-context non è ancora un sostituto completo del retrieval
5. Non esiste un metodo universalmente superiore — la scelta dipende dal caso d'uso

Vedi la [[research-metodi-alternativi-a-retrieval-augmented-generatio-2026-06-01|ricerca completa]] per dettagli e riferimenti.