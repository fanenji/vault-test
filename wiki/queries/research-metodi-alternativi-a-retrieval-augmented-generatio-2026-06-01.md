---
type: query
title: "Research: Metodi alternativi a Retrieval Augmented Generation per grounding di LLM su domini specifici"
created: 2026-06-01
updated: 2026-06-01
origin: deep-research
tags: [research]
sources: []
related: []
---

# Research: Metodi alternativi a Retrieval Augmented Generation per grounding di LLM su domini specifici

# Metodi alternativi a Retrieval Augmented Generation per il grounding di LLM su domini specifici

## Panoramica

Il **Retrieval-Augmented Generation (RAG)** [1] — architettura che combina un LLM con un sistema di retrieval esterno (si veda [[retrieval-augmented-generation|Retrieval-Augmented Generation]] nella wiki) — è oggi il metodo dominante per ancorare i modelli linguistici a conoscenza specifica di dominio, con un tasso di adozione enterprise dell'85% nel 2026 [1]. Tuttavia, RAG non è l'unica strada: esistono diverse alternative e complementi, ciascuno con trade-off distinti in termini di costo computazionale, latenza, qualità e aggiornabilità.

Questa pagina passa in rassegna le principali alternative: **fine-tuning**, **continued pretraining**, **knowledge graph augmented generation**, **long-context**, **in-context learning avanzato** e approcci **ibridi**, confrontandone vantaggi e limiti.

---

## 1. Fine-Tuning

Il **fine-tuning** (FT) è l'alternativa più matura al RAG. Consiste nell'addestrare ulteriormente un modello pre-addestrato su dati etichettati del dominio target, adattandone i pesi alla terminologia e ai task specifici [5][6][7].

### Full Fine-Tuning vs PEFT

Per modelli sotto i 2 miliardi di parametri, il **full fine-tuning** (aggiornamento di tutti i pesi) produce performance downstream superiori. Per modelli più grandi, le tecniche **Parameter-Efficient Fine-Tuning (PEFT)** come **LoRA** e **QLoRA** preservano meglio la capacità di ragionamento e funzionano bene in combinazione con RAG [6].

### RAG vs Fine-Tuning: trade-off

| Dimensione | RAG | Fine-Tuning |
|---|---|---|
| **Aggiornabilità** | Immediata (basta reindicizzare) | Richiede un nuovo ciclo di training |
| **Costo iniziale** | Basso (solo indicizzazione) | Alto (GPU, dataset etichettati) |
| **Costo a runtime** | Più alto (retrieval + inferenza) | Più basso (solo inferenza) |
| **Accuratezza fattuale** | Alta (ancorata a fonti) | Dipendente dalla qualità del training |
| **Stile/Formato** | Generico | Specializzato sul dominio |
| **Trasparenza** | Fonti citabili | Scatola nera |

Secondo IBM [7], il fine-tuning eccelle per task che richiedono formattazione consistente ed expertise di dominio, mentre RAG è superiore per informazioni aggiornate e sourcing trasparente. Un paper arXiv [10] mostra che il FT da solo produce miglioramenti medi del 78.3% in exact match, ma non raggiunge il limite superiore di RAG con embedding ottimali.

### Limiti del solo FT

- **Allucinazioni**: senza grounding esterno il modello può generare risposte errate con sicurezza
- **Conoscenza datata**: il modello riflette lo stato della conoscenza al momento del training
- **Catastrofic forgetting**: rischio di perdere capacità generaliste

---

## 2. Approcci Ibridi: FT + RAG

La combinazione **fine-tuning + RAG** è emersa come la strategia più performante. Un paper arXiv [10] mostra che modelli fine-tuned con RAG ottengono miglioramenti di 3.67, 2.48 e 4.21 punti percentuali assoluti (EM, ES, BLEU) rispetto a modelli base con RAG, usando BM25 come retrieval.

### RAFT (Retrieval Augmented Fine-Tuning)

Introdotto da UC Berkeley [8], **RAFT** addestra il modello a integrare retrieval e generazione in un unico processo. Il modello impara a:
- Distinguere documenti rilevanti da distrattori
- Citare le fonti nelle risposte
- Ragionare sui documenti recuperati

Questo crea un LLM più potente e adattabile, combinando l'expertise di dominio del FT con l'ancoraggio fattuale del RAG [8].

---

## 3. Continued Pretraining (CPT)

Il **Continued Pretraining** (o continual pre-training) è il processo di addestrare ulteriormente un modello già pre-addestrato su un corpus non etichettato del dominio target, prima di qualsiasi fine-tuning supervisionato [11][12][13].

A differenza del fine-tuning (che usa dati etichettati per task specifici), il CPT usa dati non etichettati per trasferire conoscenza di dominio profonda nei pesi del modello, similmente a quanto fa il pre-training iniziale [7].

### Vantaggi

- **Nessun bisogno di dati etichettati**: richiede solo testo grezzo del dominio
- **Adattamento profondo**: il modello impara terminologia, relazioni e pattern del dominio
- **Efficienza**: strategie di data selection permettono di ottenere performance comparabili con solo il 10% del corpus [11]
- **Mantenimento capacità generaliste**: con tecniche di mitigazione del forgetting [12]

### Esempi concreti

- **FinPythia-6.9B**: CPT sul dominio finanziario partendo da Pythia. Miglioramenti consistenti su task finanziari senza degradazione su benchmark generalisti [11]
- **Domyn Colosseum 355B**: CPT su larga scala per settori regolati (finanza, sanità) usando infrastruttura NVIDIA DGX Cloud. Addestramento in FP8 per efficienza [16]
- **Unsloth**: framework open-source che semplifica il CPT, con attenzione a strategie anti-catastrophic forgetting [15]

### CPT vs LoRA

Un paper recente [15] mostra che **LoRA è inferiore al full fine-tuning per CPT** perché apprende meno e dimentica meno — un trade-off che può essere indesiderabile quando l'obiettivo è iniettare nuova conoscenza. Alternative come LoftQ, PiSSA, DoRA possono migliorare la situazione [15].

### Limiti del CPT

- Costo computazionale elevato (giorni di GPU)
- Richiede expertise ML per hyperparameter tuning
- Meno flessibile del RAG: per aggiornare la conoscenza serve un nuovo ciclo di CPT
- Rischio di specializzazione eccessiva se non bilanciato con dati generalisti

---

## 4. Knowledge Graph Augmented Generation (GraphRAG / KG-RAG)

Mentre il RAG tradizionale si basa su [[vector-database|Vector Database]] e [[hybrid-search|Hybrid Search]] per recuperare chunk di testo, il **GraphRAG** (o KG-RAG) integra un **knowledge graph** per arricchire il contesto con relazioni strutturate tra entità [17][18][19].

### Come funziona

1. **Estrazione entità**: un LLM (es. LLMGraphTransformer) estrae entità e relazioni dai documenti
2. **Costruzione grafo**: nodi = entità, archi = relazioni. Storage in Neo4j o simile
3. **Retrieval ibrido**: query Cypher per traversare il grafo + ricerca vettoriale per contesto non strutturato
4. **Generazione**: l'LLM fonde contesto strutturato e non strutturato

### Vantaggi sul RAG tradizionale

- **Multi-hop reasoning**: il grafo connette fatti correlati, permettendo di rispondere a domande che richiedono più passaggi logici [18]
- **Tracciabilità**: ogni risposta è riconducibile a entità e relazioni specifiche nel grafo
- **Minor rumore**: il retrieval basato su grafo è più preciso del puro retrieval vettoriale per query relazionali [19]

### Varianti

- **GraphRAG puro**: solo knowledge graph, senza componente vettoriale
- **Hybrid GraphRAG**: combina ricerca vettoriale e graph traversal — l'approccio più performante secondo benchmark nel dominio finanziario e nelle telecomunicazioni [17]
- **KG-RAG su GCP**: integrazione con Vertex AI, BigQuery e Neo4j Aura [20]

### Limiti

- Complessità di costruzione e manutenzione del grafo
- Latenza più alta per il retrieval multi-step
- Richiede dati già strutturati o pipeline di estrazione affidabili

---

## 5. Long-Context come alternativa

Con l'espansione delle finestre di contesto dei LLM moderni (128K token e oltre), un'alternativa emergente al RAG è semplicemente **includere l'intero set di documenti nel prompt**, senza retrieval [2]:

### Vantaggi teorici

- Nessuna pipeline di retrieval da costruire e mantenere
- Nessuna perdita di informazione per chunking o retrieval imperfetto
- Il modello ha accesso a tutto il contesto

### Problemi reali

- **Token-inefficienza**: processare grandi contesti è costoso e lento
- **"Lost in the middle"**: i modelli faticano a prestare attenzione alle parti centrali di contesti molto lunghi
- **Distrazione**: troppa informazione irrilevante degrada la qualità dell'output [2]

### LDAR (Learning Distraction-Aware Retrieval)

Presentato a ICLR 2026 [2], **LDAR** è un retriever adattivo che impara a bilanciare copertura informativa e distrazione in base alla capacità del LLM. Raggiunge performance significativamente superiori con meno token rispetto agli approcci long-context puri [2].

---

## 6. In-Context Learning e Prompt Engineering Avanzato

### In-Context RALM

Il paper di AI21 Labs [23] propone **In-Context Retrieval-Augmented Language Modeling**: invece di modificare l'architettura del modello, i documenti di grounding vengono semplicemente prepesi al prompt. Questo approccio:
- Lascia il modello frozen (nessun retraining)
- Usa retriever general-purpose off-the-shelf
- Adatta la query del retriever al task del LM
- Adatta il meccanismo di lettura dei documenti alla generazione

Risultati: guadagni sostanziali in accuratezza senza complessità architetturale [23].

### Emulazione RAG via Prompt Engineering

Un paper arXiv [22] dimostra che è possibile **emulare RAG interamente via prompt engineering e chain-of-thought**, senza un sistema di retrieval esterno:
1. Il modello riceve l'intero contesto lungo
2. Prompt specializzati guidano il modello a identificare segmenti rilevanti (tagging)
3. Il modello esegue reasoning multi-hop sui segmenti taggati

Nei benchmark BABILong, questo approccio supera sia le baseline senza retrieval che le pipeline RAG tradizionali in scenari multi-fatto con contesti lunghi [22].

### Context Engineering

Un concetto emergente distinto dal prompt engineering è il **context engineering**: la progettazione sistematica di tutto ciò che circonda il prompt (retrieval, memoria, cronologia conversazione) per fornire al modello esattamente l'informazione giusta, nel formato giusto [25]. I fallimenti degli agenti AI sono più spesso causati da contesto inadeguato che da prompt mal progettati [25].

---

## 7. LLM Knowledge Base (approccio Karpathy)

Una variante leggera emersa nel 2025-2026 è l'**LLM Knowledge Base**: anziché usare retrieval vettoriale, si mantiene un file markdown strutturato con tutte le definizioni e conoscenze del dominio, che viene incluso interamente nel context window [1].

**Vantaggi**: semplicità estrema, curation umana di qualità, nessuna infrastruttura di retrieval.
**Limiti**: non scala a livello enterprise (migliaia di documenti), il quality ceiling dipende dalla curation manuale.

L'intuizione sottostante — che la qualità della curation conta più della sofisticazione del retrieval — è allineata con le migliori pratiche RAG [1].

---

## 8. Confronto sistematico

| Metodo | Costo iniziale | Costo runtime | Aggiornabilità | Qualità dominio | Tracciabilità |
|---|---|---|---|---|---|
| **RAG** | Basso | Medio | Immediata | Medio-alta | Alta |
| **Fine-Tuning** | Alto | Basso | Lenta (nuovo training) | Alta | Bassa |
| **FT + RAG (ibrido)** | Alto | Medio | Media | Molto alta | Alta |
| **Continued Pretraining** | Molto alto | Basso | Molto lenta | Molto alta | Bassa |
| **GraphRAG** | Medio-alto | Alto | Media | Alta (multi-hop) | Molto alta |
| **Long-Context** | Nullo | Molto alto | Immediata | Media | Media |
| **In-Context RALM** | Basso | Medio | Immediata | Media | Alta |
| **Prompt Emulation** | Nullo | Alto | Immediata | Medio-bassa | Bassa |

---

## 9. Raccomandazioni pratiche

1. **Per knowledge base dinamiche e in rapida evoluzione**: RAG tradizionale (con [[hybrid-search|Hybrid Search]] e [[Reranker]]) rimane la scelta più pragmatica.
2. **Per domini con terminologia molto specialistica e task ripetitivi**: Fine-tuning (o CPT) + RAG ibrido offre il miglior rapporto qualità/costo.
3. **Per domini con relazioni complesse multi-entità** (legale, biomedicale, supply chain): GraphRAG/KG-RAG è superiore al RAG vettoriale puro.
4. **Per prototipazione rapida o contesti ridotti**: In-context RALM o LLM Knowledge Base eliminano complessità infrastrutturale.
5. **Per massima qualità possibile**: CPT → Fine-tuning → RAG (pipeline completa) con [[embedding-model|Embedding Model]] ottimizzati e reranking.

---

## 10. Frontiere di ricerca aperte

- **Distraction-aware retrieval** (LDAR, ICLR 2026): retriever che imparano a bilanciare copertura e rumore [2]
- **Data selection per CPT**: strategie di novelty+diversity per massimizzare il trasferimento di conoscenza con corpora minimi [11][12]
- **GraphRAG + agenti**: integrazione di knowledge graph con agenti LLM autonomi per retrieval adattivo
- **Small LMs + RAG**: modelli piccoli (<2B) con RAG possono eguagliare modelli molto più grandi [6]

---

## Riferimenti

1. [LLM Knowledge Base vs RAG: Differences, Tradeoffs, and Use Cases](https://atlan.com/know/llm-knowledge-base-vs-rag) — atlan.com
2. [Beyond RAG vs. Long-Context: Learning Distraction-Aware Retrieval for Efficient Knowledge Grounding](https://openreview.net/forum?id=c8CZWLy4T4) — openreview.net (ICLR 2026)
3. [Three Alternative RAG Models - SQL, Knowledge Bases, & APIs](https://www.exxactcorp.com/blog/deep-learning/alternative-rag-models) — exxactcorp.com
4. [Best RAG Evaluation Tools in 2026, Compared](https://www.braintrust.dev/articles/best-rag-evaluation-tools) — braintrust.dev
5. [RAG Vs. Fine Tuning: Which One Should You Choose?](https://montecarlo.ai/blog-rag-vs-fine-tuning) — montecarlo.ai
6. [Fine Tuning vs. Retrieval Augmented Generation for Less Popular Knowledge](https://arxiv.org/html/2403.01432v3) — arxiv.org
7. [RAG vs. fine-tuning](https://www.ibm.com/think/topics/rag-vs-fine-tuning) — ibm.com
8. [RAG vs. Fine-Tuning: Which Strategy is Best for Customizing LLMs?](https://www.runpod.io/blog/rag-vs-fine-tuning-llm-customization) — runpod.io
9. [Best LLM Knowledge Base Tools in 2026: Enterprise RAG Compared](https://atlan.com/know/llm-knowledge-base-tools) — atlan.com
10. [RAG or Fine-tuning? A Comparative Study on LCMs-based ...](https://arxiv.org/pdf/2505.15179) — arxiv.org
11. [Efficient Continual Pre-training for Building Domain Specific Large Language Models](https://openreview.net/forum?id=onyGT5Nbuz) — openreview.net (ICLR 2024)
12. [Efficient Continual Pre-training for Building Domain Specific Large Language Models](https://aclanthology.org/2024.findings-acl.606.pdf) — aclanthology.org (ACL 2024 Findings)
13. [Continued Pre-Training Technique](https://www.emergentmind.com/topics/continued-pre-training-technique) — emergentmind.com
14. [Continued Pretraining of State-of-the-Art LLMs for Sovereign AI and Regulated Industries](https://developer.nvidia.com/blog/continued-pretraining-of-state-of-the-art-llms-for-sovereign-ai-and-regulated-industries-with-domyn-and-nvidia-dgx-cloud) — developer.nvidia.com
15. [Continued LLM Pretraining with Unsloth](https://unsloth.ai/blog/contpretraining) — unsloth.ai
16. [Benchmarking Vector, Graph and Hybrid Retrieval Augmented Generation (RAG) Pipelines](https://arxiv.org/html/2507.03608v1) — arxiv.org
17. [Knowledge Graph vs RAG: Know the Differences](https://www.puppygraph.com/blog/knowledge-graph-vs-rag) — puppygraph.com
18. [How to Improve Multi-Hop Reasoning With Knowledge Graphs](https://neo4j.com/blog/genai/knowledge-graph-llm-multi-hop-reasoning) — neo4j.com
19. [Conventional RAG vs Knowledge Graph–Augmented RAG](https://medium.com/@kr.amit.sri/conventional-rag-vs-knowledge-graph-augmented-rag-63814dd8176c) — medium.com
20. [Emulating Retrieval Augmented Generation via Prompt Engineering](https://arxiv.org/html/2502.12462v1) — arxiv.org
21. [In-Context Retrieval-Augmented Language Models](https://uploads-ssl.webflow.com/60fd4503684b466578c0d307/63c6c20dec4479564db21819_NEW_In_Context_Retrieval_Augmented_Language_Models.pdf) — AI21 Labs
22. [Prompt engineering with Retrieval Augmented Generation systems](https://aarontay.substack.com/p/prompt-engineering-with-retrieval) — aarontay.substack.com
23. [Prompt Engineering Patterns for Successful RAG Implementations](https://machinelearningmastery.com/prompt-engineering-patterns-successful-rag-implementations) — machinelearningmastery.com
24. [Context Engineering vs Prompt Engineering](https://abstracta.us/blog/ai/context-engineering-vs-prompt-engineering) — abstracta.us

## References

1. [LLM Knowledge Base vs RAG: Differences, Tradeoffs, and Use Cases](https://atlan.com/know/llm-knowledge-base-vs-rag) — atlan.com
2. [Beyond RAG vs. Long-Context: Learning Distraction-Aware Retrieval for Efficient Knowledge Grounding](https://openreview.net/forum?id=c8CZWLy4T4) — openreview.net
3. [Three Alternative RAG Models - SQL, Knowledge Bases, & APIs](https://www.exxactcorp.com/blog/deep-learning/alternative-rag-models) — exxactcorp.com
4. [Best RAG Evaluation Tools in 2026, Compared](https://www.braintrust.dev/articles/best-rag-evaluation-tools) — braintrust.dev
5. [RAG Vs. Fine Tuning: Which One Should You Choose?](https://montecarlo.ai/blog-rag-vs-fine-tuning) — montecarlo.ai
6. [Fine Tuning vs. Retrieval Augmented Generation for Less Popular Knowledge](https://arxiv.org/html/2403.01432v3) — arxiv.org
7. [RAG vs. fine-tuning](https://www.ibm.com/think/topics/rag-vs-fine-tuning) — ibm.com
8. [RAG vs. Fine-Tuning: Which Strategy is Best for Customizing LLMs?](https://www.runpod.io/blog/rag-vs-fine-tuning-llm-customization) — runpod.io
9. [Best LLM Knowledge Base Tools in 2026: Enterprise RAG Compared](https://atlan.com/know/llm-knowledge-base-tools) — atlan.com
10. [RAG or Fine-tuning? A Comparative Study on LCMs-based ...](https://arxiv.org/pdf/2505.15179) — arxiv.org
11. [Efficient Continual Pre-training for Building Domain Specific Large Language Models](https://openreview.net/forum?id=onyGT5Nbuz) — openreview.net
12. [Efficient Continual Pre-training for Building Domain Specific Large Language Models](https://aclanthology.org/2024.findings-acl.606.pdf) — aclanthology.org
13. [Continued Pre-Training Technique](https://www.emergentmind.com/topics/continued-pre-training-technique) — emergentmind.com
14. [Continued Pretraining of State-of-the-Art LLMs for Sovereign AI](https://developer.nvidia.com/blog/continued-pretraining-of-state-of-the-art-llms-for-sovereign-ai-and-regulated-industries-with-domyn-and-nvidia-dgx-cloud) — developer.nvidia.com
15. [Continued LLM Pretraining with Unsloth](https://unsloth.ai/blog/contpretraining) — unsloth.ai
16. [Benchmarking Vector, Graph and Hybrid RAG Pipelines](https://arxiv.org/html/2507.03608v1) — arxiv.org
17. [Knowledge Graph vs RAG: Know the Differences](https://www.puppygraph.com/blog/knowledge-graph-vs-rag) — puppygraph.com
18. [How to Improve Multi-Hop Reasoning With Knowledge Graphs](https://neo4j.com/blog/genai/knowledge-graph-llm-multi-hop-reasoning) — neo4j.com
19. [Conventional RAG vs Knowledge Graph–Augmented RAG](https://medium.com/@kr.amit.sri/conventional-rag-vs-knowledge-graph-augmented-rag-63814dd8176c) — medium.com
20. [Emulating Retrieval Augmented Generation via Prompt Engineering](https://arxiv.org/html/2502.12462v1) — arxiv.org
21. [In-Context Retrieval-Augmented Language Models](https://uploads-ssl.webflow.com/60fd4503684b466578c0d307/63c6c20dec4479564db21819_NEW_In_Context_Retrieval_Augmented_Language_Models.pdf) — AI21 Labs
22. [Prompt engineering with RAG systems - tread with caution!](https://aarontay.substack.com/p/prompt-engineering-with-retrieval) — aarontay.substack.com
23. [Prompt Engineering Patterns for Successful RAG Implementations](https://machinelearningmastery.com/prompt-engineering-patterns-successful-rag-implementations) — machinelearningmastery.com
24. [Context Engineering vs Prompt Engineering](https://abstracta.us/blog/ai/context-engineering-vs-prompt-engineering) — abstracta.us
25. [[D] what's the alternative to retrieval augmented generation?](https://www.reddit.com/r/MachineLearning/comments/1edbg0h/d_whats_the_alternative_to_retrieval_augmented) — reddit.com
