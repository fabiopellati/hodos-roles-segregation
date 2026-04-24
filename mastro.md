# Mastro — Segregazione Ruoli LLM

## 2026-04-24 — Chiusura QUESTIONE-001: RFC inbound Q133: progettazione arricchimento segregazione ruoli LLM

**Questione**: QUESTIONE-001 — RFC inbound Q133: progettazione arricchimento segregazione ruoli LLM

**Percorso**

Aperta per accogliere la RFC inbound QUESTIONE-133 dal progetto Hodos. La progettazione ha attraversato più fasi: analisi delle domande aperte della RFC, discussione di design con l'operatore che ha esteso il perimetro (generalizzazione dei ruoli, catalogo abilità, preset gov/ops, segregazione come modalità attivabile), risoluzione di sei domande aperte emerse dal piano di lavoro, redazione degli artefatti (guida e skill), verifica di congruenza con il protocollo base e gli arricchimenti esistenti. La sorgente è stata registrata in hodos-mcp con tag v0.1.0. La Response RFC è stata compilata e restituita.

**Decisioni prese**

L'arricchimento è stato progettato come meccanismo generico con catalogo di abilità discriminabili, anziché con tre ruoli predefiniti come suggerito dalla RFC. Il preset consigliato usa due ruoli (gov/ops) anziché tre. La segregazione è una modalità ambientale attivabile e disattivabile a comando, non un obbligo. La segnalazione è bloccante. Le segnalazioni restano nel flusso conversazionale e il prodotto durevole sono le direttive nel CLAUDE.md.

**Impatto**

Creati artefatti/it_IT/guide/arricchimento-segregazione-ruoli.md e artefatti/it_IT/skills/arricchimento-segregazione-ruoli.md. Compilata la sezione Response RFC in rfc/rfc-Q133-segregazione-ruoli.md. Sorgente llm-roles-segregation registrata e indicizzata in hodos-mcp.

---


## 2026-04-24 — Chiusura QUESTIONE-003: Registrazione sorgente llm-roles-segregation in hodos-mcp

**Questione**: QUESTIONE-003 — Registrazione sorgente llm-roles-segregation in hodos-mcp

**Percorso**

Aperta per richiedere la registrazione della sorgente nel server hodos-mcp. Generata RFC outbound Q003. Il team hodos-mcp ha accettato e completato la registrazione, con una deviazione sul path degli artefatti (project/artefatti/ corretto in artefatti/).

**Decisioni prese**

Sorgente registrata in hodos-mcp. Il path corretto degli artefatti è artefatti/, non project/artefatti/ come indicato nella RFC.

**Impatto**

La sorgente llm-roles-segregation è operativa nel server hodos-mcp con 2 artefatti indicizzati (guida e skill). L'arricchimento è ora distribuibile e utilizzabile nelle opere Hodos.

---


---
