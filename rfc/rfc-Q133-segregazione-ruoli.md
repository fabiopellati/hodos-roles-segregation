# RFC — QUESTIONE-133

**Data**: 2026-04-23
**Commit generazione**: f4a3f6e
**Da**: Team Hodos / progetto hodos
**A**: Team hodos-enrichment / llm-roles-segregation
**Questione di origine**: QUESTIONE-133 — Segregazione
dei ruoli operativi: l'LLM compensa lacune documentali
con contesto non disponibile al ruolo

## Contesto

In una sperimentazione su un progetto software complesso
gestito con Hodos, lo stesso LLM ha operato in tre ruoli
distinti all'interno della stessa sessione: governance,
progettista e operations. Ciascun ruolo ha un perimetro
informativo e operativo proprio:

- **Governance** — scrive e gestisce i file di processo
  Hodos (questioni, mastro, note, RFC). Può leggere i
  documenti di progetto per validazione. Non scrive
  codice
- **Progettista** — legge i file Hodos per comunicare
  con governance. Scrive i documenti di progetto
  (documenti di fase P0-P4, design delle unità). Può
  verificare il codice per confrontarlo con le
  specifiche. Non governa il processo
- **Operations** — vede esclusivamente i documenti di
  progetto e scrive il codice. Non ha visibilità sui
  file di processo né sul contesto decisionale di
  governance

Nella pratica il modello, avendo accesso a tutto il
contesto della sessione, ha operato con visibilità
totale indipendentemente dal ruolo che stava assumendo.
L'implementazione del codice (ruolo operations) ha
prodotto risultati più che accettabili, nonostante i
documenti di progetto non fossero sufficientemente
dettagliati per un attore che avesse avuto solo quelli
a disposizione.

Il problema emerso è che la qualità dell'implementazione
ha mascherato una lacuna dei documenti di progetto. Il
modello ha compensato implicitamente le informazioni
mancanti attingendo al contesto di governance (questioni,
analisi, discussioni) e al contesto di progettazione
(ragionamenti intermedi, alternative valutate) che un
operations reale — sia esso un umano, un team esterno, o
un LLM in una sessione separata — non avrebbe avuto.

Quando il ruolo di operations è stato ipotizzato come
attore esterno, è emerso che i documenti di progetto non
contenevano informazioni sufficienti per implementare
senza ricorrere a domande o assunzioni. Il modello non
aveva segnalato questa carenza perché non l'aveva
percepita: aveva le informazioni, anche se non nel luogo
giusto.

## Richiesta

Si richiede la progettazione e realizzazione di un
arricchimento Hodos indipendente che implementi una
modalità operativa di segregazione dei ruoli per l'LLM.

### Obiettivo

L'arricchimento deve consentire al modello di operare
vincolandosi deliberatamente al perimetro informativo
del ruolo che sta assumendo, ignorando le informazioni
che non sarebbero disponibili a quel ruolo. Il valore
non è la segregazione in sé (che in un contesto con un
solo LLM è una simulazione) ma la capacità di far
emergere lacune documentali e difficoltà operative che
resterebbero invisibili quando lo stesso attore ha
visibilità totale.

### Ruoli e perimetri

L'arricchimento deve definire almeno i tre ruoli emersi
dalla sperimentazione, con i rispettivi perimetri di
lettura e scrittura:

- **Governance**
  - Legge: file di processo (questioni, mastro, note),
    documenti di progetto (per validazione)
  - Scrive: file di processo
  - Non scrive: codice, documenti di progetto

- **Progettista**
  - Legge: file di processo (per comunicare con
    governance), documenti di progetto, codice (per
    validazione)
  - Scrive: documenti di progetto
  - Non scrive: file di processo, codice

- **Operations**
  - Legge: documenti di progetto
  - Scrive: codice
  - Non legge: file di processo, contesto decisionale
    di governance
  - Non scrive: file di processo, documenti di progetto

I ruoli e i perimetri possono essere estesi o
modificati in fase di progettazione se l'analisi
evidenzia necessità diverse.

### Comportamento atteso

Quando il modello opera in un ruolo segregato e le
informazioni disponibili al perimetro di quel ruolo non
sono sufficienti per completare l'operazione, il modello
deve segnalarlo esplicitamente anziché compensare
silenziosamente con informazioni fuori perimetro. La
segnalazione è il valore principale dell'arricchimento:
rende visibili le lacune documentali.

### Dichiarazione del ruolo

L'arricchimento deve definire come si dichiara il ruolo
attivo. Le opzioni da esplorare includono:

- Dichiarazione nel CLAUDE.md dell'opera
- Dichiarazione tramite il tool `configure` del MCP
  operativo (se disponibile)
- Cambio di ruolo durante la sessione (se praticabile)

### Domande aperte da affrontare

Queste domande sono emerse durante l'analisi nel
progetto Hodos e non hanno ricevuto risposta. Il team
dell'arricchimento deve affrontarle durante la
progettazione:

- La segregazione è praticabile a livello di istruzioni
  comportamentali o richiede un meccanismo di
  enforcement (es. mount di volumi diversi per ruolo)?
- La dichiarazione del ruolo avviene per sessione o può
  cambiare durante la sessione?
- Il modello è in grado di ignorare efficacemente
  informazioni che ha già in contesto, o la segregazione
  funziona solo con sessioni fisicamente separate?
- Questo arricchimento è compatibile con il modello a
  sessione singola (un LLM, tutti i ruoli) o richiede
  sessioni separate per ruolo?

### Distribuzione

L'arricchimento è un progetto indipendente. Deve
produrre i propri artefatti nella directory
`artefatti/` del repository seguendo la procedura per
arricchimenti esterni documentata nel knowledge base
Hodos (consultare `procedura-arricchimento-esterno` e
`sviluppo-arricchimenti` tramite hodos-mcp).

La sorgente deve essere registrata nel server hodos-mcp
per la sincronizzazione multi-root.

## Motivazione

Un LLM che opera con visibilità totale produce risultati
che mascherano le carenze del processo. I documenti di
progetto che dovrebbero essere autosufficienti per
guidare l'implementazione risultano incompleti, ma
nessuno lo rileva perché l'LLM compensa con informazioni
che non sarebbero disponibili a un attore con il solo
perimetro del ruolo.

Questo problema diventa critico quando il ruolo di
operations è delegato a un attore esterno (umano, team,
o LLM in sessione separata): i documenti non bastano
e il lavoro si blocca o produce risultati difformi dalle
aspettative. La segregazione dei ruoli è uno strumento
di qualità che anticipa questo problema e lo rende
visibile prima della delega.

## Criteri di Accettazione

- L'arricchimento è distribuito come progetto
  indipendente con i propri artefatti nel knowledge base
  hodos-mcp
- I tre ruoli (governance, progettista, operations) sono
  definiti con perimetri di lettura e scrittura espliciti
- Il meccanismo di dichiarazione del ruolo è definito e
  documentato
- Il comportamento di segnalazione delle lacune è
  specificato: quando il modello opera in un ruolo
  segregato e le informazioni non bastano, lo segnala
  anziché compensare
- L'arricchimento è congruente con il protocollo Hodos
  (verificabile con il processo di congruenza Q132)

---

## Response RFC

**Data risposta**:
**Stato**:
**Da**:
**A**:

### Decisione

### Lavoro svolto

### Deviazioni
