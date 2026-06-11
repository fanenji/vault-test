---
type: query
title: "Metodi di allineamento citati nella wiki"
created: 2026-05-31
origin: chat
query: "Quali sono i metodi di allineamento citati nella wiki?"
tags: [allineamento, sicurezza, ai]
---

# Metodi di allineamento citati nella wiki

La wiki documenta due metodi principali di allineamento AI:

## RLHF (Reinforcement Learning from Human Feedback)

Addestramento basato su valutazioni umane delle risposte del modello. Viene citato in [[ai-alignment|AI Alignment]] come uno degli approcci principali e usato come termine di paragone per [[constitutional-ai|Constitutional AI]]. Richiede annotatori umani per valutare le risposte.

## Constitutional AI (CAI)

Metodo di allineamento sviluppato da [[Anthropic]] che utilizza un insieme esplicito di principi (una "costituzione") per l'auto-supervisione del modello. Funziona in due fasi:

1. Il modello genera risposte e poi le **critica e rivede** secondo i principi dati
2. Questo riduce il bisogno di etichette umane per identificare comportamenti dannosi

CAI è presentato come alternativa e complemento a RLHF, spostando il focus dalla supervisione umana diretta a vincoli espliciti e interpretabili.

## AI Interpretability (complementare)

L'[[ai-interpretability|AI Interpretability]] non è un metodo di allineamento in sé, ma una disciplina complementare: capire i meccanismi interni di un modello aiuta a verificare che sia effettivamente allineato.

## Applicazione

Entrambi i metodi convergono in [[Claude]], il modello conversazionale di Anthropic addestrato con Constitutional AI.

## Fonti

- [[ai-alignment|AI Alignment]]
- [[constitutional-ai|Constitutional AI]]
- [[Anthropic]]
- [[Claude]]
- [[ai-interpretability|AI Interpretability]]
