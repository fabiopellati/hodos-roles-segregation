# RFC — QUESTIONE-003

**Data**: 2026-04-24
**Commit generazione**: bb8db23
**Da**: Team hodos-enrichment / llm-roles-segregation
**A**: Team hodos / hodos-mcp
**Questione di origine**: QUESTIONE-003 — Registrazione
sorgente llm-roles-segregation in hodos-mcp

## Contesto

Il progetto llm-roles-segregation ha completato la
progettazione dell'arricchimento Hodos per la
segregazione dei ruoli operativi dell'LLM, in risposta
alla RFC QUESTIONE-133 del progetto Hodos.

Gli artefatti (una guida informativa e uno skill
operativo) sono stati redatti, verificati per
congruenza con il protocollo base e con gli
arricchimenti esistenti, e pubblicati nel repository
con tag v0.1.0.

Il repository è disponibile su GitHub:
git@github.com:fabiopellati/hodos-roles-segregation.git

## Richiesta

Si richiede la registrazione della sorgente
`llm-roles-segregation` nel server hodos-mcp,
affinché i due artefatti vengano indicizzati nel
knowledge base e resi disponibili tramite i tool MCP.

Informazioni per la registrazione:

- **Nome sorgente**: `llm-roles-segregation`
- **Repository**:
  `git@github.com:fabiopellati/hodos-roles-segregation.git`
- **Directory artefatti**: `project/artefatti/`
- **Tag**: `v0.1.0`
- **Numero artefatti attesi**: 2
- **Locale**: `it_IT`

Struttura degli artefatti:

```
project/artefatti/
  it_IT/
    guide/
      arricchimento-segregazione-ruoli.md
    skills/
      arricchimento-segregazione-ruoli.md
```

Dopo la registrazione, la sincronizzazione dovrebbe
essere invocabile con:

```bash
curl -X POST \
  https://vps-350eddbb.vps.ovh.net/hodos/mcp/sync \
  -H "Content-Type: application/json" \
  -d '{"source": "llm-roles-segregation", \
       "tag": "v0.1.0"}'
```

## Motivazione

L'arricchimento è un progetto indipendente che produce
i propri artefatti seguendo la procedura per
arricchimenti esterni. Per essere utilizzabile nelle
opere Hodos deve essere indicizzato nel knowledge base
hodos-mcp, affinché i tool `search_knowledge`,
`get_protocol_rules`, `get_skill` e `list_skills` lo
restituiscano nelle query pertinenti.

Senza la registrazione della sorgente l'arricchimento
è completo ma non distribuibile.

## Criteri di Accettazione

- La sorgente `llm-roles-segregation` è registrata
  nel server hodos-mcp
- `check_version` mostra la sorgente con 2 artefatti
- `search_knowledge` con query "segregazione ruoli"
  restituisce la guida
- `get_skill` con name
  "arricchimento-segregazione-ruoli" restituisce lo
  skill
- `list_skills` include lo skill nella lista

---

## Response RFC

**Data risposta**:
**Stato**:
**Da**:
**A**:

### Decisione

### Lavoro svolto

### Deviazioni
