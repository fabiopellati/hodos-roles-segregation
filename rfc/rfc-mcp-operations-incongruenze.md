# RFC — QUESTIONE-002

**Data**: 2026-04-24
**Da**: Team hodos-enrichment / llm-roles-segregation
**A**: Team hodos / mcp-operations
**Questione di origine**: QUESTIONE-002 — Incongruenze
nel server mcp-operations rilevate durante l'uso

## Contesto

Durante i primi utilizzi del server mcp-operations in
un'opera Hodos reale (progetto llm-roles-segregation)
sono emerse incongruenze nel comportamento di alcuni
tool. Le incongruenze sono state rilevate verificando
manualmente il file `questioni.md` dopo ogni operazione
di scrittura effettuata dai tool MCP.

## Richiesta

Si richiede la correzione delle incongruenze elencate
di seguito. Per ciascuna viene descritto il
comportamento atteso, il comportamento osservato e la
correzione manuale applicata.

## Incongruenze rilevate

### INC-001 — update_stato non sincronizza la riga indice

**Tool**: `update_stato`

**Operazione**: aggiornamento di QUESTIONE-001 da `open`
a `in-progress`.

**Comportamento atteso**: il campo Stato nel corpo della
questione e la corrispondente riga nella tabella Indice
vengono entrambi aggiornati al nuovo stato.

**Comportamento osservato**: il campo Stato nel corpo e
la Storia sono stati aggiornati correttamente, ma la
riga nell'Indice è rimasta a `open`.

**Correzione manuale**: modifica diretta della riga
indice nel file `questioni.md`.

### INC-002 — create_questione usa data errata

**Tool**: `create_questione`

**Operazione**: creazione di QUESTIONE-002 in data
2026-04-24.

**Comportamento atteso**: la data nella Storia e nel
contatore "Ultima questione inserita" riportano la data
corrente (2026-04-24).

**Comportamento osservato**: entrambi i campi riportano
2026-04-23 anziché 2026-04-24. Il tool sembra usare una
data non aggiornata, probabilmente il fuso orario del
server o un valore non derivato dalla data corrente
dell'ambiente operativo.

**Nota**: l'operazione è avvenuta a cavallo del cambio
data (sera del 2026-04-23, ora locale già 2026-04-24).
La causa potrebbe essere il fuso orario del container
MCP non allineato con quello dell'ambiente operativo
dell'utente.

**Correzione manuale**: modifica diretta della data nel
contatore e nella riga della Storia.

**Aggiornamento**: la stessa incongruenza si riproduce
anche con il tool `add_commento` (COMMENTO-003 della
QUESTIONE-001, scritto il 2026-04-24 ma datato
2026-04-23). Il problema non è limitato a
`create_questione` ma è trasversale ai tool che
scrivono date.

## Motivazione

I tool di mcp-operations sono il meccanismo primario con
cui l'agente AI gestisce i file di processo Hodos. Se i
tool producono incongruenze silenziose, l'agente deve
verificare e correggere manualmente dopo ogni
operazione, il che vanifica il valore dell'automazione e
introduce rischio di errori non rilevati.

## Criteri di Accettazione

- Il tool `update_stato` aggiorna coerentemente sia il
  campo Stato nel corpo della questione sia la riga
  corrispondente nella tabella Indice
- Il tool `create_questione` usa la data corrente
  dell'ambiente operativo per tutti i campi che
  richiedono una data
- I fix sono verificabili creando una questione e
  aggiornandone lo stato in un'opera di test

---

## Response RFC

**Data risposta**:
**Stato**:
**Da**:
**A**:

### Decisione

### Lavoro svolto

### Deviazioni
