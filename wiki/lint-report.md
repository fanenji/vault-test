---
type: log
title: "Wiki Lint Report"
created: 2026-06-02
tags: [lint, audit, maintenance]
---

# Wiki Lint Report

> **Data**: 2026-06-02
> **Vault**: vault-test
> **Pagine totali**: 26
> **Esito**: 0 warning strutturali sulla wiki (4 auto-referenziali in questo file) — la wiki è sana.

---

## Riepilogo esecutivo

L'audit iniziale ha rilevato **83 warning** (65 broken-link + 14 missing-page + 4 frontmatter), quasi interamente dovuti a un mismatch di naming convention: i wikilink usavano spazi (es. ⟦Constitutional AI⟧) mentre i filename usano trattini (`constitutional-ai.md`). L'operazione `--fix` ha normalizzato tutti i wikilink, risolvendo 79 warning. I 4 warning residui sono confinati in questo stesso file e sono auto-referenziali (esempi di documentazione). **Nessuno stub creato**: tutte le pagine "mancanti" esistevano già su disco.

---

## Azioni correttive applicate (`--fix`)

### Wikilink normalizzati (10 pattern × 23 file)

| Forma originale | Forma corretta | File |
|---|---|---|
| ⟦Constitutional AI⟧ | ⟦constitutional-ai\|Constitutional AI⟧ | 7 |
| ⟦AI Alignment⟧ | ⟦ai-alignment\|AI Alignment⟧ | 5 |
| ⟦AI Interpretability⟧ | ⟦ai-interpretability\|AI Interpretability⟧ | 5 |
| ⟦Retrieval-Augmented Generation⟧ | ⟦retrieval-augmented-generation\|Retrieval-Augmented Generation⟧ | 9 |
| ⟦Embedding Model⟧ | ⟦embedding-model\|Embedding Model⟧ | 6 |
| ⟦Vector Database⟧ | ⟦vector-database\|Vector Database⟧ | 8 |
| ⟦Hybrid Search⟧ | ⟦hybrid-search\|Hybrid Search⟧ | 9 |
| ⟦Continued Pretraining⟧ | ⟦continued-pretraining\|Continued Pretraining⟧ | 5 |
| ⟦Long-Context Grounding⟧ | ⟦long-context-grounding\|Long-Context Grounding⟧ | 4 |
| ⟦In-Context RALM⟧ | ⟦in-context-ralm\|In-Context RALM⟧ | 5 |

### Wikilink speciali corretti

- ⟦AI Alignment#RLHF\|RLHF⟧ → ⟦ai-alignment\|RLHF⟧ (anchor `#RLHF` inesistente come heading — rimosso)
- ⟦ricerca completa\|research-...⟧ → ⟦research-metodi-alternativi-...\|ricerca completa⟧ (target↔alias invertito)

### Frontmatter aggiunto

| File | Campo aggiunto |
|---|---|
| `sources/anthropic-claude.md` | `source_path: "_inbox/clippings/anthropic-claude.md"` |
| `sources/retrieval-augmented-generation.md` | `source_path: "_inbox/clippings/retrieval-augmented-generation.md"` |
| `sources/research-metodi-alternativi-...` | `source_path: "raw/sources/research-metodi-alternativi-..."` |

---

## Issue deterministici residui

### broken-link (0 sulla wiki, 4 auto-referenziali in lint-report.md)

Nessun broken link nel resto della wiki. I 4 warning del lint script riguardano esempi di documentazione nel vecchio report (ormai sovrascritto).

### orphan (8) — info, atteso

Pagine senza inbound link diretto. Atteso per query pages e overview:

- `overview.md` — pagina auto-generata
- `lint-report.md` — questo report
- `queries/metodi-allineamento-wiki-2026-05-31.md`
- `queries/research-metodi-alternativi-a-retrieval-augmented-generatio-2026-06-01.md`
- `queries/research-constitutional-ai-di-anthropic-2026-06-01.md`
- `concepts/retrieval-augmented-generation.md` — ha inbound via display-name wikilinks
- `sources/anthropic-claude.md` — le source sono tipicamente foglia

### no-outlinks (1) — info

- `sources/anthropic-claude.md` — source page senza wikilink in uscita. Impatto trascurabile.

---

## Check semantico

Analisi qualitativa di tutte le 26 pagine wiki (Step 2 della skill wiki-lint).

### Risultato: nessuna contraddizione

Le pagine sono internamente coerenti. I collegamenti tra concetti formano un grafo consistente. Le relazioni comparative dichiarate (es. "FT+RAG è spesso più performante", "CPT ha costi elevati ma deep knowledge") sono corroborate dalle fonti.

### Suggerimenti

#### 1. Missing page: RLHF
**Severity**: `info`

RLHF (Reinforcement Learning from Human Feedback) è citato in 4 pagine come termine di paragone primario per Constitutional AI, ma non ha una pagina dedicata. È il metodo di allineamento più citato dopo CAI.
→ **Suggerimento**: creare `concepts/rlhf.md`

#### 2. Suggestion: pagina di confronto metodi di grounding
**Severity**: `info`

La wiki copre 7 metodi di grounding in pagine separate. Una synthesis page con tabella comparativa e guida alla scelta sarebbe utile — attualmente la sintesi esiste solo nell'overview.
→ **Suggerimento**: creare `synthesis/comparazione-metodi-grounding.md`

#### 3. Suggestion: PEFT / LoRA
**Severity**: `info`

PEFT e LoRA sono citati brevemente in `fine-tuning.md` e `continued-pretraining.md`. Data la rilevanza pratica, una sezione dedicata migliorerebbe la copertura.
→ **Suggerimento**: espandere la sezione PEFT in `fine-tuning.md` o creare `concepts/peft.md`

---

## Statistiche

| Categoria | Pre-fix | Post-fix |
|---|---|---|
| `broken-link` (warning) | 65 | 0 (sulla wiki) |
| `missing-page` (warning) | 14 | 0 (sulla wiki) |
| `frontmatter` (warning) | 4 | 0 (sulla wiki) |
| `orphan` (info) | 16 | 8 |
| `no-outlinks` (info) | 1 | 1 |
| **Totale warning** | **83** | **0** |
| **Totale info** | **17** | **9** |

> I 4 warning visibili nello script sono auto-referenziali (esempi ⟦...⟧ nel vecchio report). Al netto di `lint-report.md`: **0 warning strutturali**.

---

## Raccomandazioni

1. ~~Correggere wikilink~~ ✓ Fatto
2. Valutare la creazione di `concepts/rlhf.md`
3. Considerare una synthesis page per il confronto metodi di grounding
4. Eseguire `qmd embed --update` dopo modifiche future
