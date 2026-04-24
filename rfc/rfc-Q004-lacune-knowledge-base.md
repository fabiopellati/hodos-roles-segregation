# RFC — QUESTIONE-004

**Data**: 2026-04-24
**Commit generazione**: bb8db23
**Da**: Team hodos-enrichment / llm-roles-segregation
**A**: Team hodos
**Questione di origine**: QUESTIONE-004 — Lacune nel
knowledge base Hodos che inducono l'agente a redigere
RFC non conformi

## Contesto

Durante la progettazione dell'arricchimento
segregazione-ruoli, l'agente AI ha dovuto redigere
più RFC outbound. In tutti i casi la prima stesura
non era conforme al protocollo Hodos e ha richiesto
correzioni manuali da parte dell'operatore.

L'analisi delle cause ha evidenziato che gli errori
non derivano da limiti del modello ma da lacune nel
knowledge base che rendono il workflow RFC difficile
da seguire per l'agente. Gli errori si sono ripetuti
in modo sistematico, il che suggerisce che il
problema sia strutturale e non episodico.

## Richiesta

Si richiede di valutare le tre lacune descritte di
seguito e, se ritenute pertinenti, di integrare il
knowledge base per prevenire la classe di errori
osservata.

### Lacuna 1 — Convenzione di naming dei file RFC

Il template RFC (`template-rfc.md`) specifica la
struttura del documento e la directory di
destinazione (`rfc/`), ma non dichiara la
convenzione di naming del file. L'agente deve
inferire il pattern (`rfc-Q{NNN}-{descrizione}.md`)
da un esempio esistente nella directory, se ne trova
uno. In assenza di esempi l'agente sceglie un nome
arbitrario.

Suggerimento: aggiungere al template una riga
esplicita con il formato atteso del nome file.

### Lacuna 2 — Assenza di checklist operativa per il flusso outbound

Il flusso outbound è descritto in più artefatti
(template RFC, guida team interno, diagramma flusso
RFC) in forma narrativa. L'agente deve ricostruire
mentalmente la sequenza corretta dai diversi
documenti. In pratica l'agente tende a saltare
direttamente alla redazione del documento senza
completare i passi preliminari.

Gli errori osservati:
- RFC creata senza questione di origine
- RFC creata con la questione ancora in stato
  `open` o `in-progress` anziché `pending-rfc`
- RFC legata a una questione esistente che copriva
  un'esigenza diversa, anziché a una questione
  dedicata

Suggerimento: aggiungere allo skill o alla guida AI
una checklist operativa per la redazione di RFC
outbound, ad esempio:

1. Verificare che esista una questione dedicata
   all'esigenza che richiede intervento esterno
2. Portare la questione a `pending-rfc`
3. Consultare il template RFC con `get_template`
4. Redigere il documento seguendo il template
5. Nominare il file `rfc-Q{NNN}-{descrizione}.md`
6. Committare e aggiornare il campo
   `Commit generazione`

### Lacuna 3 — Retrieval del template non presidiato

L'agente tende a redigere artefatti protocollari
basandosi sulla conoscenza che ha già in contesto
anziché fare retrieval del template dal knowledge
base. Questo produce artefatti che seguono la
struttura "ricordata" dall'agente, che può essere
incompleta o non aggiornata.

Il problema non è specifico delle RFC: è una
classe di errori che si applica a qualsiasi
artefatto con struttura vincolata (questioni,
note, entry del mastro, RFC).

Suggerimento: aggiungere alle norme AI una
direttiva del tipo "prima di redigere qualsiasi
artefatto protocollare, consultare il template
corrispondente con `get_template`". Il costo del
retrieval è trascurabile rispetto al costo della
correzione manuale.

### Lacuna 4 — Frontmatter skill incompleto nella procedura arricchimento esterno

La procedura per arricchimenti esterni
(`procedura-arricchimento-esterno.md`) mostra un
esempio di frontmatter per lo skill:

```yaml
---
tipo-artefatto: skill
documento: arricchimento-{nome}
descrizione: {descrizione breve, una riga}
autorita: operativa
---
```

L'esempio non include il campo `skill:`, che è
necessario affinché i tool `get_skill` e
`list_skills` trovino l'artefatto. Senza quel campo
il nome dello skill non viene indicizzato in Qdrant
e il filtro per `skillName` lo esclude dai risultati.
Il tool `search_knowledge` lo trova comunque perché
usa la ricerca vettoriale senza filtrare per
`skillName`, il che rende il problema difficile da
diagnosticare: l'artefatto sembra presente nel
knowledge base ma non è raggiungibile dai tool
specifici.

L'agente segue l'esempio documentato e produce uno
skill invisibile a `get_skill` e `list_skills`.

Suggerimento: aggiungere il campo `skill:` al
frontmatter di esempio nella procedura:

```yaml
---
tipo-artefatto: skill
skill: arricchimento-{nome}
documento: arricchimento-{nome}
descrizione: {descrizione breve, una riga}
autorita: operativa
---
```

## Motivazione

Gli errori di conformità dell'agente nella redazione
di artefatti protocollari hanno tre conseguenze:

- L'operatore deve verificare e correggere
  manualmente, il che vanifica parte del valore
  dell'automazione
- L'agente impara dalla correzione nella sessione
  corrente ma non nelle sessioni successive, perché
  la conoscenza resta nel contesto della
  conversazione e non nel knowledge base
- Il pattern si ripete a ogni nuova opera e a ogni
  nuovo agente che utilizza Hodos, perché la causa è
  nel knowledge base, non nell'agente specifico

Le integrazioni proposte sono minimali (una riga nel
template, una checklist, una norma) e prevengono una
classe di errori sistematica.

## Criteri di Accettazione

- Il template RFC dichiara la convenzione di naming
  del file
- Esiste una checklist operativa (nello skill, nella
  guida AI, o in un artefatto dedicato) che l'agente
  può seguire passo per passo per redigere una RFC
  outbound
- Le norme AI includono una direttiva sul retrieval
  del template prima della redazione di artefatti
  protocollari
- L'esempio di frontmatter nella procedura per
  arricchimenti esterni include il campo `skill:`
- Un agente senza conoscenza pregressa del progetto,
  consultando il knowledge base, è in grado di
  redigere una RFC outbound conforme al primo
  tentativo

---

## Response RFC

**Data risposta**:
**Stato**:
**Da**:
**A**:

### Decisione

### Lavoro svolto

### Deviazioni
