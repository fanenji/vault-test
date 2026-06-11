---
type: concept
title: "Vector Database"
created: 2026-05-31
updated: 2026-05-31
tags: [ai, retrieval, database, embeddings]
related: [embedding-model, retrieval-augmented-generation, hybrid-search]
sources: ["_inbox/clippings/retrieval-augmented-generation.md"]
---

# Vector Database

Un **vector database** è un sistema di archiviazione ottimizzato per memorizzare e cercare vettori densi (embedding) in base alla similarità. È il secondo componente della pipeline [[retrieval-augmented-generation|RAG]].

## Funzione

Il vector database indicizza milioni di embedding generati da un [[embedding-model|Embedding Model]] e permette di trovare rapidamente i vettori più vicini a un vettore di query, usando metriche come cosine similarity, dot product o distanza euclidea.

## Implementazioni

La source cita tre esempi:

- **FAISS** (Meta): libreria per similarità e clustering, non un database completo
- **Qdrant**: vector database open-source con API REST e gRPC
- **sqlite-vec**: estensione vector per SQLite

## Ruolo in RAG

Nel contesto [[retrieval-augmented-generation|RAG]], il vector database è il ponte tra l'indicizzazione dei documenti e il retrieval: archivia gli embedding e risponde alle query di similarità del retriever.