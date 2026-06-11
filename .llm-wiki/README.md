# .llm-wiki/

Stato interno delle skill llm-wiki. Tutto qui dentro è **rigenerabile** dalle skill stesse e ignorato da git (vedi `.gitignore` della vault root).

## Contenuto

| Path | Cosa contiene |
|---|---|
| `queue/items.json` | Coda persistente dell'ingest (pending / running / done / error) |
| `ingest-cache/<sha256>.json` | Risultato cached dello step-1 analisi per file invariati |
| `review/items.json` | Item flaggati dalle skill che richiedono decisione umana |
| `chats/<id>.json` | Storico conversazioni della skill `wiki-query` (opzionale) |
| `qmd-index.sqlite` | Indice locale QMD (BM25 + vector embeddings) |
| `config.json` | Config locale della vault (override del template) |
| `secrets.json` | Chiavi API (Tavily, ecc.) — **mai committare** |

## Configurazione

Copia `config.example.json` in `config.json` e modifica i valori. Le skill leggono in cascata:

1. Variabili ambiente (es. `TAVILY_API_KEY`)
2. `.llm-wiki/secrets.json`
3. `.llm-wiki/config.json`
4. Default hardcoded nelle skill

## Reset

Per resettare completamente lo stato (mantenendo `wiki/` e `raw/`):

```bash
rm -rf .llm-wiki/queue .llm-wiki/ingest-cache .llm-wiki/review .llm-wiki/qmd-index.sqlite*
qmd embed   # rigenera l'indice
```
