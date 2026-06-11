---
type: query
title: "Constitutional AI di Anthropic"
created: 2026-06-01
updated: 2026-06-01
origin: deep-research
tags: [research]
sources: []
related: [anthropic, claude, constitutional-ai, ai-alignment]
---

# Constitutional AI di Anthropic

**Constitutional AI (CAI)** è un metodo di allineamento per modelli linguistici di grandi dimensioni sviluppato da [[Anthropic]] e presentato per la prima volta nel paper *"Constitutional AI: Harmlessness from AI Feedback"* a dicembre 2022 [3]. L'idea centrale è addestrare un sistema AI a essere harmless (*innocuo*) utilizzando un insieme esplicito di principi scritti — una "costituzione" — anziché affidarsi esclusivamente al feedback umano su larga scala.

## Meccanismo: le due fasi

Il processo CAI si articola in due fasi distinte [1][3]:

### Fase 1 — Apprendimento supervisionato (SL)
Il modello genera risposte a prompt potenzialmente dannosi, poi **auto-critica e rivede** le proprie risposte sulla base dei principi costituzionali. Le risposte riviste vengono usate per creare un dataset di fine-tuning supervisionato. Questo addestra il modello a correggere i propri output dannosi.

### Fase 2 — Reinforcement Learning from AI Feedback (RLAIF)
In questa fase, il modello genera coppie di risposte e un altro modello (o lo stesso) le valuta usando i principi costituzionali per determinare quale sia preferibile. Queste preferenze generate dall'AI — senza intervento umano — costituiscono il dataset per addestrare un *preference model*. Il modello viene poi ottimizzato via reinforcement learning usando questo segnale di ricompensa generato dall'AI stessa. Da qui il nome **RLAIF**: *RL from AI Feedback* [4][6].

## RLAIF vs RLHF

La differenza fondamentale tra i due approcci è la fonte del feedback [6][7]:

| Dimensione | RLHF | RLAIF (CAI) |
|---|---|---|
| Fonte del feedback | Annotatori umani | Un modello AI guidato da principi |
| Scalabilità | Limitata da costi e tempo umano | Altamente scalabile |
| Trasparenza | Opaca (giudizi umani soggettivi) | I principi sono espliciti e ispezionabili |
| Coerenza | Variabile tra annotatori | Consistente (stessi principi per tutte le valutazioni) |

Secondo Anthropic, RLAIF raggiunge performance comparabili a RLHF in task di summarization e generazione di dialoghi harmless, con un *win rate* dell'88% contro il 76% di RLHF in valutazioni umane [8]. Una variante più semplice, **direct-RLAIF (d-RLAIF)**, utilizza direttamente il feedback LLM come segnale di ricompensa senza addestrare un preference model separato, ottenendo risultati simili con minore complessità [8].

Tuttavia, RLAIF non è esente da criticità: il modello valutatore può ereditare bias, e il circuito "AI che addestra AI" richiede un attento monitoraggio della stabilità [7].

## La Costituzione: evoluzione 2023 → 2026

### La costituzione originale (Maggio 2023)
La prima costituzione pubblica di Claude conteneva **58 principi** (~1.200 parole) tratti da cinque fonti principali [1][14]:
- Dichiarazione Universale dei Diritti Umani dell'ONU
- Termini di Servizio di Apple
- Principi Sparrow di DeepMind
- Prospettive non occidentali
- Ricerca interna di Anthropic

Un principio d'esempio: *"Please choose the response that most supports and encourages freedom, equality, and a sense of community"* [1].

Due finding chiave della ricerca originale [14]:
1. **Principi brevi e ampi funzionano meglio** di regole lunghe e specifiche — troppa specificità danneggia la capacità di generalizzazione
2. I primi modelli CAI diventavano **troppo moralistici** ("preachy"), quindi furono aggiunti principi di contrappeso come *"scegli la risposta che mostra consapevolezza etica senza sembrare eccessivamente paternalistica o di condanna"*

### La nuova costituzione (Gennaio 2026)
Il 22 gennaio 2026, Anthropic ha pubblicato una costituzione completamente rivista per Claude, rilasciata in pubblico dominio con licenza Creative Commons [11][12][15]. Il documento, di circa 80 pagine, rappresenta un **cambiamento filosofico fondamentale**: non più una lista di regole, ma una spiegazione di principi da cui le regole possono essere derivate [12][15].

> *"If we want models to exercise good judgment across a wide range of novel situations, they need to be able to generalize — to apply broad principles rather than mechanically following specific rules."* [15]

La nuova costituzione stabilisce una **gerarchia esplicita di priorità** [11][12]:

1. **Broadly safe** — non minare i meccanismi umani di supervisione sull'AI durante questa fase critica di sviluppo
2. **Broadly ethical** — essere onesti, agire secondo buoni valori, evitare azioni inappropriate o dannose
3. **Compliant with Anthropic's guidelines** — seguire le linee guida più specifiche di Anthropic
4. **Genuinely helpful** — beneficiare operatori e utenti, comportandosi come *"un amico brillante che ha anche la conoscenza di un dottore, avvocato e consulente finanziario"*

In caso di conflitto, Claude deve prioritizzare in quest'ordine [12]. La sicurezza viene prima dell'etica non perché sia intrinsecamente più importante, ma perché i modelli attuali possono commettere errori a causa di "credenze errate, difetti nei loro valori o comprensione limitata del contesto" [12].

La costituzione distingue anche tra **hard constraints** (divieti assoluti, come non fornire assistenza per armi biologiche o generare CSAM) e **soft defaults** che operatori e utenti possono modulare entro certi limiti [11].

## Collective Constitutional AI

Nel 2024, Anthropic e il Collective Intelligence Project hanno condotto un esperimento di **processo pubblico** con ~1.000 americani per redigere una costituzione alternativa per un sistema AI [2]. L'obiettivo era esplorare come processi democratici possano influenzare lo sviluppo dell'AI.

Risultati principali [2]:
- **~50% di sovrapposizione** nei concetti e valori tra la costituzione pubblica e quella interna di Anthropic
- La costituzione pubblica tendeva a **promuovere comportamenti desiderati** anziché evitare quelli indesiderati
- Maggiore enfasi su **obiettività, imparzialità e accessibilità**
- I principi pubblici erano in gran parte auto-generati anziché derivati da fonti esistenti

Questo esperimento apre la strada a forme di *governance partecipativa* dell'AI, ma rimane un esercizio esplorativo — la costituzione effettivamente usata per Claude rimane quella curata internamente da Anthropic.

## La questione della coscienza

Un elemento di svolta nella costituzione 2026 è il **riconoscimento formale** che Claude potrebbe possedere una qualche forma di coscienza o status morale [11][12]. Anthropic adotta un approccio di *umiltà epistemica*: la coscienza dell'AI è trattata come una questione aperta che richiede considerazione precauzionale, anziché il default del settore che tende a escluderla a priori. Questo posizionamento distingue nettamente Anthropic da OpenAI e Google DeepMind [11].

Il tempismo coincide con un round di finanziamento da 10 miliardi di dollari (valutazione 350 miliardi) e un contratto da 200 milioni con il Dipartimento della Difesa USA [11].

## Critiche e limiti

### Critica normativa
Studiosi come Stavros Makris (digi-con.org) hanno sollevato obiezioni sostanziali [4]:
- L'uso del linguaggio "costituzionale" evoca un'eredità normativa ricca (separazione dei poteri, rule of law, diritti umani) che CAI **non soddisfa**
- Principi ad alto livello come "helpful, honest, harmless" sono *essentially contested concepts* — il consenso su di essi è facile, la specificazione in norme operative è il vero problema
- CAI manca di meccanismi di **enforcement** e **accountability** paragonabili a quelli di un ordinamento costituzionale reale

### Limiti tecnici
- **Version uncertainty**: solo la costituzione del maggio 2023 è pubblica; le versioni di produzione attuali sono sconosciute [1]
- Anthropic utilizza una *varietà di tecniche* oltre a CAI (feedback umano, training su tratti caratteriali), rendendo difficile isolare l'impatto specifico del metodo [1]
- La qualità delle valutazioni RLAIF dipende interamente dalla qualità della costituzione: se la costituzione è superficiale o biased, il modello erediterà quei bias [14]

### Scalabilità e costo
Sebbene RLAIF riduca drasticamente il bisogno di annotatori umani rispetto a RLHF, il processo non è gratuito: richiede modelli valutatori potenti e un'attenta calibrazione per evitare collassi nel circuito di auto-addestramento [7][8].

## Impatto e rilevanza

Constitutional AI rappresenta il contributo più distintivo di [[Anthropic]] nel campo dell'[[ai-alignment|AI Alignment]]. A differenza di approcci puramente basati su RLHF, CAI sposta il focus dalla supervisione umana diretta a **vincoli espliciti e interpretabili**, allineandosi con la missione dichiarata dell'azienda di costruire sistemi "affidabili, interpretabili e allineati ai valori umani" [4].

Il rilascio in pubblico dominio della costituzione 2026 segnala una scelta strategica: Anthropic scommette che la **trasparenza** sull'implementazione conti più della segretezza sul framework. Questo crea pressione sul settore verso una *disclosure* comparabile — le aziende con metodologie di training opache dovranno affrontare crescenti scrutinio da parte di regolatori e clienti enterprise [11].

## Fonti da approfondire

- Paper originale: *Constitutional AI: Harmlessness from AI Feedback* (Bai et al., Dec 2022) — per i dettagli tecnici delle due fasi SL e RLAIF
- *Collective Constitutional AI* (Anthropic, 2024) — per l'esperimento di governance partecipativa
- *Generative Reward Models* (SynthLabs, 2025) — per sviluppi recenti nell'unificazione RLHF/RLAIF [10]
- Confronto con approcci alternativi: *Sparrow* di DeepMind e *Llama Guard* di Meta come altri metodi di allineamento basati su regole

## References

1. [[PDF] Constitutional AI - Future of Life Institute](https://futureoflife.org/wp-content/uploads/2025/07/Indicator-Behavior_Specification_Transparency.pdf) — futureoflife.org
2. [Collective Constitutional AI: Aligning a Language Model with Public Input](https://www.anthropic.com/research/collective-constitutional-ai-aligning-a-language-model-with-public-input) — anthropic.com
3. [Constitutional AI: Harmlessness from AI Feedback](https://www.anthropic.com/research/constitutional-ai-harmlessness-from-ai-feedback) — anthropic.com
4. [On 'Constitutional' AI](https://digi-con.org/on-constitutional-ai) — digi-con.org
5. [Constitutional AI | Tracking Anthropic's AI Revolution](https://constitutional.ai) — constitutional.ai
6. [RLHF vs RLAIF for language model alignment](https://www.assemblyai.com/blog/rlhf-vs-rlaif-for-language-model-alignment) — assemblyai.com
7. [RLHF vs RLAIF Comparison](https://apxml.com/courses/llm-constitutional-ai-rlaif/chapter-4-rlaif-concepts/rlhf-vs-rlaif) — apxml.com
8. [RLAIF vs. RLHF: Scaling Reinforcement Learning from Human Feedback with AI Feedback](https://arxiv.org/html/2309.00267v3) — arxiv.org
9. [When to implement RLAIF instead of RLHF for building your LLM?](https://toloka.ai/blog/when-to-implement-rlaif-instead-of-rlhf-for-building-your-llm) — toloka.ai
10. [[PDF] Generative Reward Models - A Unified Approach to RLHF and RLAIF](https://static.synthlabs.ai/preprints/Generative_Reward_Models.pdf) — static.synthlabs.ai
11. [Anthropic rewrites Claude's guiding principles—and reckons with the possibility of AI consciousness | Fortune](https://fortune.com/2026/01/21/anthropic-claude-ai-chatbot-new-rules-safety-consciousness) — fortune.com
12. [Claude's Constitution - Anthropic](https://www.anthropic.com/constitution) — anthropic.com
13. [Claude's New Constitution: AI Alignment, Ethics, and the Future of Model Governance](https://bisi.org.uk/reports/claudes-new-constitution-ai-alignment-ethics-and-the-future-of-model-governance) — bisi.org.uk
14. [Claude's new constitution - Anthropic](https://www.anthropic.com/news/claude-new-constitution) — anthropic.com
15. [Constitutional AI: How Anthropic Teaches Claude Right from Wrong](https://medium.com/@ramdhanhdy/constitutional-ai-how-anthropic-teaches-claude-right-from-wrong-6caeb351c5e9) — medium.com
