# Retrieval-Augmented Generation (RAG)

Retrieval-Augmented Generation (RAG) è un'architettura che combina un modello linguistico di grandi dimensioni (LLM) con un sistema di recupero di informazioni esterne. Invece di affidarsi solo alla conoscenza parametrica appresa durante l'addestramento, il modello recupera documenti rilevanti da una base di conoscenza al momento della query e li usa come contesto per generare la risposta.

## Componenti principali

Un sistema RAG tipico è composto da:

- **Embedding model**: trasforma testo in vettori densi che catturano il significato semantico.
- **Vector database**: archivia gli embedding e permette ricerche per similarità (es. FAISS, Qdrant, sqlite-vec).
- **Retriever**: dato un query embedding, recupera i k documenti più vicini. Spesso combina ricerca lessicale (BM25) e vettoriale in modalità *hybrid search*.
- **Reranker**: un secondo modello che riordina i candidati recuperati per rilevanza, migliorando la precisione finale.
- **Generator**: l'LLM che produce la risposta condizionata sui documenti recuperati.

## Vantaggi

RAG riduce le allucinazioni perché ancora le risposte a fonti verificabili, permette di aggiornare la conoscenza senza ri-addestrare il modello, e abilita la citazione delle fonti. È la base di molte applicazioni di question-answering su knowledge base private.

## Relazione con gli LLM

RAG non sostituisce l'LLM: lo potenzia. La qualità della generazione dipende sia dalla capacità del modello linguistico sia dalla qualità del retrieval. Un buon sistema di reranking è spesso più decisivo della dimensione del modello.
