---
type: concept
title: RAFT (Retrieval Augmented Fine-Tuning)
created: 2026-06-01
updated: 2026-06-01
tags: [ai, rag, fine-tuning, hybrid]
related: [fine-tuning, retrieval-augmented-generation]
sources: ["research-metodi-alternativi-a-retrieval-augmented-generatio-2026-06-01.md"]
---

# RAFT (Retrieval Augmented Fine-Tuning)

**RAFT** (Retrieval Augmented Fine-Tuning) è un approccio ibrido introdotto da UC Berkeley che combina [[Fine-Tuning]] e [[retrieval-augmented-generation|RAG]] in un unico processo di training. Il modello viene addestrato a integrare retrieval e generazione, imparando a distinguere documenti rilevanti da distrattori.

## Come funziona

Durante il training, il modello riceve:
- Una query
- Un insieme di documenti (alcuni rilevanti, altri no)
- Deve generare la risposta corretta citando le fonti appropriate

Questo insegna al modello a:
1. Distinguere documenti rilevanti da distrattori
2. Citare le fonti nelle risposte
3. Ragionare sui documenti recuperati

## Vantaggi

- Combina l'expertise di dominio del FT con l'ancoraggio fattuale del RAG
- Maggiore accuratezza rispetto a FT o RAG da soli
- Migliore capacità di ragionamento su contesti multipli
- Adattabile a esigenze di dominio specifiche

## Vedi anche

- [[Fine-Tuning]] — l'approccio di training supervisionato
- [[retrieval-augmented-generation|Retrieval-Augmented Generation]] — l'architettura di retrieval esterno
- [[continued-pretraining|Continued Pretraining]] — alternativa non supervisionata