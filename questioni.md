# Questioni — Segregazione Ruoli LLM

## Indice

| ID | Titolo | Stato |
|---|---|---|
| QUESTIONE-004 | Lacune nel knowledge base Hodos che inducono l'agente a redigere RFC non conformi | pending-rfc |
| QUESTIONE-002 | Incongruenze nel server mcp-operations rilevate durante l'uso | open |



> Ultima questione inserita: QUESTIONE-004 — 2026-04-24.
> Ultima questione chiusa: QUESTIONE-001 — 2026-04-24.


## QUESTIONE-004 — Lacune nel knowledge base Hodos che inducono l'agente a redigere RFC non conformi

**Tipo**: rilievo
**Stato**: pending-rfc

**Storia**

- 2026-04-24 pending-rfc — Le lacune sono nel knowledge base Hodos, serve intervento del team Hodos tramite RFC
- 2026-04-24 open — Lacune nel knowledge base Hodos che inducono l'agente a redigere RFC non conformi

**Descrizione**

Durante la redazione di RFC outbound l'agente ha commesso errori ripetuti di conformità al protocollo: documento a forma libera anziché da template, RFC senza questione di origine, naming del file non convenzionale, questione non portata a pending-rfc prima della generazione. L'analisi delle cause ha evidenziato lacune nel knowledge base che rendono il workflow difficile da seguire per l'agente: assenza della convenzione di naming dei file RFC, assenza di una checklist operativa per il flusso outbound, assenza di una norma che imponga il retrieval del template prima della redazione.

**Impatto**

- knowledge base hodos — template RFC, guide operative e norme AI potrebbero essere integrati per prevenire errori di conformità dell'agente

**Questioni collegate**: QUESTIONE-003

---




## QUESTIONE-002 — Incongruenze nel server mcp-operations rilevate durante l'uso

**Tipo**: anomalia
**Stato**: open

**Storia**

- 2026-04-24 open — Incongruenze nel server mcp-operations rilevate durante l'uso

**Descrizione**

Durante i primi utilizzi del server mcp-operations in un'opera Hodos reale sono emerse incongruenze nel comportamento dei tool. Le anomalie vengono raccolte progressivamente e saranno inoltrate al team mcp-operations tramite RFC quando il quadro sarà sufficientemente maturo.

**Impatto**

- rfc/rfc-Q002-incongruenze-mcp-operations.md — RFC outbound da compilare con le incongruenze raccolte e da inoltrare al team mcp-operations

---


