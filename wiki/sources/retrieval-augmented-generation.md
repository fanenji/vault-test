---
type: source
title: "Retrieval-Augmented Generation (RAG)"
created: 2026-05-31
updated: 2026-05-31
tags: [ai, llm, retrieval, rag]
related: [retrieval-augmented-generation, embedding-model, vector-database, hybrid-search, reranker, claude]
sources: ["_inbox/clippings/retrieval-augmented-generation.md"]
source_path: "_inbox/clippings/retrieval-augmented-generation.md"
---

# Retrieval-Augmented Generation (RAG)

Fonte introduttiva sull'architettura **Retrieval-Augmented Generation (RAG)**, che combina un LLM con un sistema di recupero di informazioni esterne.

## Contenuti principali

- **Definizione e motivazione**: RAG recupera documenti rilevanti da una base di conoscenza al momento della query, riducendo la dipendenza dalla sola conoscenza parametrica
- **Pipeline a 5 componenti**: [[embedding-model|Embedding Model]] → [[vector-database|Vector Database]] → Retriever (con [[hybrid-search|Hybrid Search]]) → [[Reranker]] → Generator (LLM)
- **Vantaggi**: riduzione delle allucinazioni, aggiornamento della conoscenza senza ri-addestramento, citazione delle fonti
- **Relazione con gli LLM**: RAG potenzia l'LLM senza sostituirlo; la qualità del retrieval e del reranking è spesso più decisiva della dimensione del modello

## Note

Documento divulgativo introduttivo. Assenti benchmark, confronti tra vector database o dettagli implementativi. Utile come panoramica dell'architettura.