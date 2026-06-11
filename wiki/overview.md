---
type: overview
title: "Wiki Overview"
auto_generated: true
---

# Overview

Questa wiki raccoglie conoscenze sull'intelligenza artificiale, coprendo aziende e prodotti AI, metodi di allineamento, architetture di retrieval applicate agli LLM, e metodi di grounding su domini specifici.

## Temi coperti

- **Aziende e prodotti AI**: [[Anthropic]] come azienda di ricerca focalizzata su sicurezza e interpretabilità; [[Claude]] come famiglia di LLM conversazionali, utilizzabile anche in architetture RAG
- **Metodi di allineamento**: [[constitutional-ai|Constitutional AI]] (CAI) come alternativa/complemento a RLHF, basato su principi espliciti di auto-supervisione
- **Concetti trasversali**: [[ai-alignment|AI Alignment]] (il problema di allineare AI ai valori umani) e [[ai-interpretability|AI Interpretability]] (la capacità di spiegare i meccanismi interni dei modelli)
- **Retrieval ed embeddings**: [[retrieval-augmented-generation|RAG]] come architettura che combina LLM e retrieval esterno, con la sua pipeline a 5 componenti: [[embedding-model|Embedding Model]], [[vector-database|Vector Database]], [[hybrid-search|Hybrid Search]], [[Reranker]], e Generator (LLM). RAG riduce le allucinazioni e permette l'aggiornamento della conoscenza senza ri-addestrare il modello
- **Metodi alternativi di grounding**: [[Fine-Tuning]] (addestramento supervisionato su dominio), [[continued-pretraining|Continued Pretraining]] (pre-training non supervisionato su corpus di dominio), [[GraphRAG]] (RAG con knowledge graph per multi-hop reasoning), [[RAFT]] (approccio ibrido FT+RAG), [[long-context-grounding|Long-Context Grounding]] (finestre di contesto estese senza retrieval), e [[in-context-ralm|In-Context RALM]] (grounding via prompt senza modifiche architetturali). La ricerca mostra che l'approccio ibrido FT+RAG è spesso il più performante, ma non esiste un metodo universalmente superiore: la scelta dipende da requisiti di latenza, costo, aggiornabilità e complessità del dominio.

La wiki cresce con l'ingest di nuove fonti e ricerche.