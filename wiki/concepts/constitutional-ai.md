---
type: concept
title: "Constitutional AI"
created: 2026-05-31
updated: 2026-05-31
tags: [ai, allineamento, sicurezza]
related: [anthropic, ai-alignment, claude]
sources: ["_inbox/clippings/anthropic-claude.md"]
---

# Constitutional AI

**Constitutional AI (CAI)** è un metodo di allineamento per modelli linguistici sviluppato da [[Anthropic]].

## Meccanismo

Invece di dipendere unicamente dal feedback umano ([[ai-alignment|RLHF]]), il modello viene addestrato a seguire un insieme esplicito di principi — una "costituzione". Il processo funziona in due fasi:

1. Il modello genera risposte e poi **critica e rivede** le proprie risposte secondo i principi dati
2. Questo processo riduce il bisogno di etichette umane per identificare comportamenti dannosi

## Relazione con RLHF

CAI è presentato come alternativa e complemento al *Reinforcement Learning from Human Feedback* (RLHF). Mentre RLHF richiede annotatori umani per valutare le risposte, CAI automatizza parte di questo processo usando i principi costituzionali come guida.

## Importanza

Rappresenta un contributo distintivo di Anthropic nel campo dell'[[ai-alignment|allineamento AI]], spostando il focus dalla supervisione umana diretta a vincoli espliciti e interpretabili.