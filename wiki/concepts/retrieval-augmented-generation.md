---
type: concept
title: "Retrieval-Augmented Generation"
created: 2026-05-31
updated: 2026-05-31
tags: [ai, llm, retrieval, rag]
related: [embedding-model, vector-database, hybrid-search, reranker, claude]
sources: ["_inbox/clippings/retrieval-augmented-generation.md"]
---

# Retrieval-Augmented Generation (RAG)

**Retrieval-Augmented Generation (RAG)** è un'architettura che combina un modello linguistico di grandi dimensioni (LLM) con un sistema di recupero di informazioni esterne. Invece di affidarsi solo alla conoscenza parametrica appresa durante l'addestramento, il modello recupera documenti rilevanti da una base di conoscenza al momento della query e li usa come contesto per generare la risposta.

## Pipeline RAG

Un sistema RAG tipico è composto da cinque componenti in sequenza:

1. **[[embedding-model|Embedding Model]]**: trasforma il testo in vettori densi che catturano il significato semantico
2. **[[vector-database|Vector Database]]**: archivia gli embedding e permette ricerche per similarità
3. **Retriever**: dato un query embedding, recupera i k documenti più vicini, spesso usando [[hybrid-search|Hybrid Search]]
4. **[[Reranker]]**: riordina i candidati recuperati per rilevanza, migliorando la precisione finale
5. **Generator (LLM)**: produce la risposta finale condizionata sui documenti recuperati

## Vantaggi

- **Riduzione delle allucinazioni**: le risposte sono ancorate a fonti verificabili
- **Aggiornamento senza ri-addestramento**: basta aggiornare il vector database
- **Citazione delle fonti**: le risposte possono includere riferimenti ai documenti originali

## Relazione con gli LLM

RAG non sostituisce l'LLM: lo potenzia. La qualità della generazione dipende sia dalla capacità del modello linguistico sia dalla qualità del retrieval. Un buon sistema di [[Reranker|reranking]] è spesso più decisivo della dimensione del modello.

## Applicazioni

RAG è la base di molte applicazioni di question-answering su knowledge base private. Modelli come [[Claude]] possono essere integrati in architetture RAG per rispondere su basi di conoscenza private citando le fonti.