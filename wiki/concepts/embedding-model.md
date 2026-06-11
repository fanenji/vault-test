---
type: concept
title: "Embedding Model"
created: 2026-05-31
updated: 2026-05-31
tags: [ai, nlp, retrieval, embeddings]
related: [vector-database, retrieval-augmented-generation, hybrid-search]
sources: ["_inbox/clippings/retrieval-augmented-generation.md"]
---

# Embedding Model

Un **embedding model** è un modello che trasforma testo in vettori densi (embedding) che catturano il significato semantico. È il primo componente della pipeline [[retrieval-augmented-generation|RAG]].

## Funzione

Dato un input testuale (una query utente o un documento), l'embedding model produce un vettore di numeri reali in uno spazio ad alta dimensionalità. Testi semanticamente simili producono vettori vicini in questo spazio, rendendo possibile la ricerca per similarità.

## Ruolo nella pipeline RAG

Nell'architettura [[retrieval-augmented-generation|RAG]], l'embedding model viene usato due volte:

1. **Indicizzazione**: tutti i documenti della knowledge base vengono trasformati in embedding e archiviati nel [[vector-database|Vector Database]]
2. **Query**: la domanda dell'utente viene trasformata in un embedding per cercare i documenti più simili

## Modelli comuni

Tra i modelli di embedding più diffusi: text-embedding-3 (OpenAI), Cohere Embed, E5, BGE (BAAI), e modelli multilingua come multilingual-e5.