---
type: concept
title: Continued Pretraining
created: 2026-06-01
updated: 2026-06-01
tags: [ai, llm, training, grounding]
related: [fine-tuning, retrieval-augmented-generation]
sources: ["research-metodi-alternativi-a-retrieval-augmented-generatio-2026-06-01.md"]
---

# Continued Pretraining

Il **Continued Pretraining (CPT)**, o continual pre-training, è il processo di addestrare ulteriormente un modello linguistico già pre-addestrato su un corpus non etichettato del dominio target. A differenza del [[Fine-Tuning]] (che usa dati etichettati per task specifici), il CPT trasferisce conoscenza di dominio profonda nei pesi del modello in modo non supervisionato.

## Vantaggi

- **Nessun bisogno di dati etichettati**: richiede solo testo grezzo del dominio
- **Adattamento profondo**: il modello impara terminologia, relazioni e pattern del dominio
- **Efficienza**: strategie di data selection (novelty + diversity) permettono performance comparabili con solo il 10% del corpus
- **Mantenimento capacità generaliste**: con tecniche di mitigazione del catastrophic forgetting

## Esempi concreti

- **FinPythia-6.9B**: CPT sul dominio finanziario partendo da Pythia. Miglioramenti consistenti su task finanziari senza degradazione su benchmark generalisti (ICLR 2024)
- **Domyn Colosseum 355B**: CPT su larga scala per settori regolati (finanza, sanità) con NVIDIA DGX Cloud, training in FP8
- **Unsloth**: framework open-source che semplifica il CPT supportando diverse strategie anti-forgetting

## CPT vs LoRA

**LoRA è inferiore al full fine-tuning per CPT** perché apprende meno e dimentica meno — un trade-off indesiderabile quando l'obiettivo è iniettare nuova conoscenza. Alternative come LoftQ, PiSSA e DoRA possono migliorare la situazione.

## Limiti

- Costo computazionale elevato (giorni di GPU)
- Richiede expertise ML per hyperparameter tuning
- Meno flessibile del RAG: per aggiornare la conoscenza serve un nuovo ciclo di CPT
- Rischio di specializzazione eccessiva se non bilanciato con dati generalisti

## Vedi anche

- [[Fine-Tuning]] — alternativa supervisionata per adattamento al dominio
- [[retrieval-augmented-generation|Retrieval-Augmented Generation]] — approccio basato su retrieval esterno