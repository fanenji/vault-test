---
type: concept
title: Fine-Tuning
created: 2026-06-01
updated: 2026-06-01
tags: [ai, llm, training, grounding]
related: [retrieval-augmented-generation, continued-pretraining, raft]
sources: ["research-metodi-alternativi-a-retrieval-augmented-generatio-2026-06-01.md"]
---

# Fine-Tuning

Il **Fine-Tuning (FT)** è il processo di addestrare ulteriormente un modello linguistico pre-addestrato su un dataset etichettato del dominio target, adattandone i pesi alla terminologia e ai task specifici. È l'alternativa più matura al [[retrieval-augmented-generation|RAG]] per il grounding di LLM su domini specifici.

## Full Fine-Tuning vs PEFT

- **Full Fine-Tuning**: aggiornamento di tutti i parametri. Più efficace per modelli sotto i 2B parametri, ma costoso per modelli grandi.
- **Parameter-Efficient Fine-Tuning (PEFT)**: tecniche come **LoRA** (Low-Rank Adaptation) e **QLoRA** che aggiornano solo una frazione dei pesi. Preservano meglio la capacità di ragionamento e funzionano bene in combinazione con RAG.

## RAG vs Fine-Tuning: trade-off

| Dimensione | RAG | Fine-Tuning |
|---|---|---|
| **Aggiornabilità** | Immediata (reindicizzazione) | Lenta (nuovo training) |
| **Costo iniziale** | Basso | Alto (GPU, dati etichettati) |
| **Costo runtime** | Più alto (retrieval + inferenza) | Più basso (solo inferenza) |
| **Accuratezza** | Alta (ancorata a fonti) | Dipende dalla qualità del training |
| **Stile** | Generico | Specializzato sul dominio |
| **Trasparenza** | Fonti citabili | Scatola nera |

Secondo IBM, il FT eccelle per task che richiedono formattazione consistente ed expertise di dominio, mentre RAG è superiore per informazioni aggiornate e sourcing trasparente.

## Limiti

- **Allucinazioni**: senza grounding esterno, il modello può generare risposte errate con sicurezza
- **Conoscenza datata**: il modello riflette lo stato della conoscenza al momento del training
- **Catastrophic forgetting**: rischio di perdere capacità generaliste acquisite nel pre-training

## Vedi anche

- [[continued-pretraining|Continued Pretraining]] — alternativa non supervisionata per deep domain knowledge
- [[RAFT]] — approccio ibrido che combina FT e RAG
- [[retrieval-augmented-generation|Retrieval-Augmented Generation]] — l'approccio dominante di grounding