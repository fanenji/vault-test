---
type: concept
title: GraphRAG
created: 2026-06-01
updated: 2026-06-01
tags: [ai, rag, knowledge-graph, retrieval]
related: [retrieval-augmented-generation, hybrid-search, vector-database]
sources: ["research-metodi-alternativi-a-retrieval-augmented-generatio-2026-06-01.md"]
---

# GraphRAG

**GraphRAG** (o KG-RAG, Knowledge Graph–Augmented RAG) è una variante del [[retrieval-augmented-generation|RAG]] che integra un **knowledge graph** per arricchire il contesto con relazioni strutturate tra entità. A differenza del RAG tradizionale basato su [[vector-database|Vector Database]] e [[hybrid-search|Hybrid Search]] testuale, GraphRAG sfrutta un grafo di conoscenza per traversal e reasoning multi-hop.

## Come funziona

1. **Estrazione entità**: un LLM (es. LLMGraphTransformer) estrae entità e relazioni dai documenti
2. **Costruzione grafo**: nodi = entità, archi = relazioni. Storage tipicamente in Neo4j
3. **Retrieval ibrido**: query Cypher per traversare il grafo + ricerca vettoriale per contesto non strutturato
4. **Generazione**: l'LLM fonde contesto strutturato e non strutturato

## Vantaggi sul RAG tradizionale

- **Multi-hop reasoning**: il grafo connette fatti correlati, permettendo risposte a domande che richiedono più passaggi logici
- **Tracciabilità**: ogni risposta è riconducibile a entità e relazioni specifiche
- **Minor rumore**: il retrieval basato su grafo è più preciso per query relazionali

## Varianti

- **GraphRAG puro**: solo knowledge graph, senza componente vettoriale
- **Hybrid GraphRAG**: combina ricerca vettoriale e graph traversal — l'approccio più performante secondo benchmark in finanza e telecomunicazioni
- **KG-RAG su GCP**: integrazione con Vertex AI, BigQuery e Neo4j Aura

## Limiti

- Elevata complessità di costruzione e manutenzione del grafo
- Latenza più alta per retrieval multi-step
- Richiede dati strutturati o pipeline di estrazione affidabili

## Vedi anche

- [[retrieval-augmented-generation|Retrieval-Augmented Generation]] — l'architettura base
- [[hybrid-search|Hybrid Search]] — componente chiave nel retrieval ibrido
- [[vector-database|Vector Database]] — storage vettoriale del RAG tradizionale