---
type: concept
title: "Hybrid Search"
created: 2026-05-31
updated: 2026-05-31
tags: [ai, retrieval, search]
related: [vector-database, embedding-model, reranker, retrieval-augmented-generation]
sources: ["_inbox/clippings/retrieval-augmented-generation.md"]
---

# Hybrid Search

**Hybrid search** è una strategia di retrieval che combina ricerca lessicale e ricerca vettoriale per migliorare la qualità dei risultati. È una pratica comune nella fase di retrieval della pipeline [[retrieval-augmented-generation|RAG]].

## Componenti

- **Ricerca lessicale (BM25)**: basata su parole chiave e frequenza dei termini, efficace per match esatti
- **Ricerca vettoriale (embedding)**: basata su similarità semantica tramite [[embedding-model|Embedding Model]], efficace per match concettuali

## Vantaggi

La combinazione delle due modalità compensa i limiti di ciascuna:
- La ricerca lessicale fallisce su sinonimi e parafrasi
- La ricerca vettoriale può perdere match esatti su termini tecnici

Il risultato combinato viene poi passato al [[Reranker]] per il raffinamento finale.