---
type: concept
title: Long-Context Grounding
created: 2026-06-01
updated: 2026-06-01
tags: [ai, llm, grounding, context-window]
related: [retrieval-augmented-generation, in-context-ralm]
sources: ["research-metodi-alternativi-a-retrieval-augmented-generatio-2026-06-01.md"]
---

# Long-Context Grounding

Il **Long-Context Grounding** è un'alternativa emergente al [[retrieval-augmented-generation|RAG]] che sfrutta le finestre di contesto estese dei LLM moderni (128K token e oltre) per includere l'intero set di documenti di dominio direttamente nel prompt, senza retrieval.

## Vantaggi teorici

- Nessuna pipeline di retrieval da costruire e mantenere
- Nessuna perdita di informazione per chunking o retrieval imperfetto
- Il modello ha accesso completo a tutto il contesto

## Problemi reali

- **Token-inefficienza**: processare grandi contesti è costoso e lento
- **"Lost in the middle"**: i modelli faticano a prestare attenzione alle parti centrali di contesti molto lunghi
- **Distrazione**: troppa informazione irrilevante degrada la qualità dell'output

## LDAR (Learning Distraction-Aware Retrieval)

Presentato a ICLR 2026, **LDAR** è un retriever adattivo che impara a bilanciare copertura informativa e distrazione in base alla capacità del LLM. Raggiunge performance significativamente superiori con meno token rispetto agli approcci long-context puri, dimostrando che un approccio intermedio tra full-context e retrieval è spesso ottimale.

## Vedi anche

- [[retrieval-augmented-generation|Retrieval-Augmented Generation]] — l'approccio standard con retrieval
- [[in-context-ralm|In-Context RALM]] — grounding via prompt senza modifiche architetturali