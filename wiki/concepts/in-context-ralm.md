---
type: concept
title: In-Context RALM
created: 2026-06-01
updated: 2026-06-01
tags: [ai, rag, prompt-engineering, grounding]
related: [retrieval-augmented-generation, long-context-grounding]
sources: ["research-metodi-alternativi-a-retrieval-augmented-generatio-2026-06-01.md"]
---

# In-Context RALM

L'**In-Context Retrieval-Augmented Language Modeling (RALM)** è un approccio al grounding di LLM proposto da AI21 Labs che evita modifiche architetturali: i documenti di grounding vengono semplicemente preposti al prompt del modello, che rimane frozen.

## Caratteristiche

- **Modello frozen**: nessun retraining necessario
- **Retriever off-the-shelf**: si usano retriever general-purpose esistenti
- **Adattamento della query**: la query del retriever viene adattata al task specifico del language model
- **Adattamento della lettura**: il meccanismo di lettura dei documenti viene ottimizzato per la generazione

## Vantaggi

- Guadagni sostanziali in accuratezza senza complessità architetturale
- Deployment semplificato rispetto a RAG con modifiche al modello
- Flessibilità: si può cambiare il retriever senza toccare il modello

## Emulazione RAG via Prompt Engineering

Un'estensione di questo approccio è l'emulazione completa di RAG via prompt engineering e chain-of-thought, dove il modello stesso funge da retriever e reasoner:
1. Riceve l'intero contesto lungo
2. Prompt specializzati guidano all'identificazione di segmenti rilevanti (tagging)
3. Esegue reasoning multi-hop sui segmenti taggati

Nei benchmark BABILong, questo approccio supera le pipeline RAG tradizionali in scenari multi-fatto.

## Vedi anche

- [[retrieval-augmented-generation|Retrieval-Augmented Generation]] — l'approccio architetturale completo
- [[long-context-grounding|Long-Context Grounding]] — alternativa che sfrutta finestre di contesto estese