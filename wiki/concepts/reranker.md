---
type: concept
title: Reranker
created: 2026-05-31
updated: 2026-05-31
tags: [ai, retrieval, nlp]
related: [hybrid-search, retrieval-augmented-generation, vector-database]
sources: ["_inbox/clippings/retrieval-augmented-generation.md"]
---

# Reranker

Il **reranker** è un modello che riordina i candidati recuperati nella fase di retrieval della pipeline [[retrieval-augmented-generation|RAG]], migliorando la precisione finale.

## Funzione

Dopo che il retriever ha recuperato k documenti candidati (tipicamente tramite [[hybrid-search|Hybrid Search]]), il reranker assegna un punteggio di rilevanza più accurato a ciascun candidato, spesso usando un modello cross-encoder che valuta la coppia (query, documento) congiuntamente.

## Perché è importante

La source evidenzia che un buon sistema di reranking è **spesso più decisivo della dimensione del modello** generativo. Un reranker efficace può:
- Filtrare documenti irrilevanti che il retriever ha erroneamente incluso
- Riordinare i risultati in modo che i documenti più pertinenti appaiano nel contesto fornito all'LLM

## Modelli comuni

Tra i reranker più usati: Cohere Rerank, BGE-Reranker (BAAI), cross-encoder basati su architetture BERT-like.

## Posizione nella pipeline

```
Retriever (BM25 + vettoriale) → Reranker → Generator (LLM)
```