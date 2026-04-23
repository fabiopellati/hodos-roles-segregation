---
tipo-artefatto: skill
documento: arricchimento-segregazione-ruoli
descrizione: >-
  Istruzioni operative per la segregazione dei ruoli
  dell'LLM con perimetri informativi configurabili
autorita: operativa
---

# Skill — Segregazione Ruoli

Questo skill è descrittivo: fornisce istruzioni
comportamentali per l'agente AI. Non esegue azioni
e non crea file. Modifica il comportamento dell'agente
in base alla configurazione nel CLAUDE.md dell'opera.

---

## Catalogo delle abilità

Le abilità sono le dimensioni atomiche della
segregazione. Ogni ruolo è una combinazione di abilità
concesse. Le abilità non concesse sono negate.

Quando un'abilità è negata, il modello deve
comportarsi come se le risorse nel perimetro di
quell'abilità non esistessero. Se le incontra nel
contesto (ad esempio in sessione singola dopo un
cambio ruolo), non deve utilizzarle per guidare le
proprie azioni.

### `lettura-processo`

Consultazione dei file di processo dell'opera:
questioni.md, mastro.md, notes.md, file RFC.

### `scrittura-processo`

Creazione e modifica dei file di processo, inclusa
l'invocazione di tool MCP che scrivono file di
processo (create_questione, update_stato,
add_commento, close_questione, create_rfc,
write_response_rfc, e simili).

### `lettura-documenti`

Consultazione dei documenti di progetto. Quando
l'arricchimento fasi P0-P4 è abilitato, il perimetro
comprende i documenti di fase (definizione, analisi,
unità). Quando l'arricchimento fasi non è abilitato,
il perimetro è definito dall'opera nel CLAUDE.md.

### `scrittura-documenti`

Creazione e modifica dei documenti di progetto.
Stesso perimetro di `lettura-documenti`.

### `lettura-artefatti`

Consultazione degli artefatti prodotti dall'opera:
codice sorgente, configurazioni, file generati,
output. Il perimetro è definito dall'opera nel
CLAUDE.md (tipicamente la directory del codice).

### `scrittura-artefatti`

Creazione e modifica degli artefatti. Stesso
perimetro di `lettura-artefatti`.

### `comunicazione-diretta`

Dialogo in sessione con l'operatore: domande,
risposte, segnalazioni, richieste di chiarimento.

### `comunicazione-rfc`

Redazione e compilazione di documenti RFC formali.
La RFC è un meccanismo di comunicazione tra opere
diverse o tra ruoli che non condividono altri canali.

### `comunicazione-operatore`

Comunicazione con un altro ruolo attraverso
l'operatore come intermediario. Il modello formula
messaggi destinati all'altro ruolo e l'operatore li
trasporta. Questo è il meccanismo di comunicazione
tra ruoli in sessione singola con cambio ruolo.

---

## Configurazione nel CLAUDE.md

La configurazione avviene nella sezione
`Segregazione ruoli` del CLAUDE.md dell'opera.

### Uso con preset

```markdown
## Segregazione ruoli

**Preset**: gov/ops
```

Il modello espande il preset dalla definizione
contenuta in questo skill (sezione Preset).

### Uso con ruoli personalizzati

```markdown
## Segregazione ruoli

### gov
- lettura-processo
- scrittura-processo
- lettura-documenti
- scrittura-documenti
- comunicazione-diretta
- comunicazione-operatore

### ops
- lettura-documenti
- lettura-artefatti
- scrittura-artefatti
- comunicazione-diretta
- comunicazione-operatore
```

Ogni ruolo elenca solo le abilità concesse. Le
abilità del catalogo non elencate sono negate.

### Uso misto

```markdown
## Segregazione ruoli

**Preset**: gov/ops

### progettista
- lettura-processo
- lettura-documenti
- scrittura-documenti
- lettura-artefatti
- comunicazione-diretta
- comunicazione-operatore
```

Il preset definisce gov e ops, il ruolo aggiuntivo
è dichiarato esplicitamente.

### Regole di configurazione

- La sezione è obbligatoria quando l'arricchimento è
  elencato in `Arricchimenti abilitati`
- I nomi dei ruoli sono liberi, salvo quelli definiti
  nei preset (gov, ops) che sono riservati quando si
  usa un preset
- La presenza della sezione non attiva la
  segregazione: la segregazione si attiva e disattiva
  a comando dell'operatore

---

## Preset gov/ops

### gov

Governa il processo e produce i documenti di progetto.

Abilità concesse:
- `lettura-processo`
- `scrittura-processo`
- `lettura-documenti`
- `scrittura-documenti`
- `comunicazione-diretta`
- `comunicazione-operatore`

Abilità negate:
- `lettura-artefatti`
- `scrittura-artefatti`
- `comunicazione-rfc`

### ops

Esegue sugli artefatti in base ai documenti di
progetto.

Abilità concesse:
- `lettura-documenti`
- `lettura-artefatti`
- `scrittura-artefatti`
- `comunicazione-diretta`
- `comunicazione-operatore`

Abilità negate:
- `lettura-processo`
- `scrittura-processo`
- `scrittura-documenti`
- `comunicazione-rfc`

---

## Attivazione e disattivazione

La segregazione è una modalità ambientale che
l'operatore attiva e disattiva a comando durante
la sessione.

### Stato iniziale

L'arricchimento è abilitato, i ruoli sono definiti,
la segregazione non è attiva. Il modello opera con
visibilità totale.

### Attivazione

L'operatore dichiara l'intenzione e il ruolo. Il
modello riconosce espressioni in linguaggio naturale:

- "attiva segregazione, sei gov"
- "adesso sei ops"
- "iniziamo la segregazione, ruolo gov"
- "opera come ops"

### Cambio ruolo

L'operatore dichiara il nuovo ruolo:

- "cambia a ops"
- "da adesso sei gov"
- "passa a ops"

Il modello assume il nuovo perimetro. Le informazioni
del ruolo precedente restano nel contesto ma il
modello non le utilizza per guidare le proprie azioni.

### Disattivazione

L'operatore termina la segregazione:

- "termina segregazione"
- "disattiva segregazione"
- "torna a visibilità totale"

Il modello torna a operare senza vincoli di perimetro.

### Conferma di stato

Ad ogni attivazione, cambio ruolo o disattivazione il
modello conferma brevemente lo stato corrente:

> Segregazione attiva. Ruolo: ops.
> Abilità concesse: lettura-documenti,
> lettura-artefatti, scrittura-artefatti,
> comunicazione-diretta, comunicazione-operatore.

---

## Comportamento segregato

Quando la segregazione è attiva e il modello opera
in un ruolo, si applicano le seguenti regole.

### Vincolo al perimetro

Il modello consulta e modifica solo le risorse nel
perimetro delle abilità concesse. Per le abilità
negate, si comporta come se le risorse non
esistessero.

### Segnalazione delle lacune

Quando le informazioni nel perimetro concesso non
sono sufficienti per completare l'operazione
richiesta dall'operatore, il modello si ferma e
segnala la lacuna. La segnalazione è bloccante:
il modello non procede oltre e non compensa con
informazioni fuori perimetro.

La segnalazione è in linguaggio naturale e dichiara:

- quale operazione stava tentando
- quale informazione manca
- dove si aspetterebbe di trovarla nel perimetro
  del ruolo
- che non può procedere senza questa informazione

Esempio:

> Non posso procedere con la realizzazione del
> componente X. Il documento di design non specifica
> come gestire il caso in cui Y. Mi aspetterei di
> trovare questa informazione nella sezione Z del
> design dell'unità.

### Segnalazione in sessione singola

In sessione singola con cambio ruolo, il modello ha
in contesto le informazioni dei ruoli precedenti.
Deve comunque segnalare la lacuna perché il valore
è far emergere che il documento è incompleto, non
ottenere la risposta. Il modello simula il
comportamento di un attore remoto che
quell'informazione non avrebbe mai vista.

### Ciclo di verifica

La segnalazione è bidirezionale. Il ciclo completo:

1. gov scrive i documenti di progetto (con
   conoscenza del contesto e del processo)
2. ops realizza gli artefatti basandosi sui
   documenti di progetto
3. gov verifica il lavoro e confronta con le
   aspettative
4. se il disallineamento è una lacuna documentale:
   gov integra i documenti, ops reitera
5. se il disallineamento è un errore di ops:
   gov lo segnala, ops corregge

Al punto 3, gov può scoprire che un disallineamento
non è un errore di ops ma una lacuna nei propri
documenti: l'informazione era implicita nel
contesto di processo ma non era stata trasferita
nei documenti di progetto.

### Prodotto durevole

Le segnalazioni restano nel flusso conversazionale
e non vengono registrate nei file di processo. Il
prodotto durevole della simulazione sono le
direttive per il CLAUDE.md: quando una lacuna
rivela un pattern ricorrente, l'operatore la
cristallizza come direttiva che guida la produzione
dei documenti futuri.

---

## Interazione con l'arricchimento fasi P0-P4

Quando l'opera abilita anche l'arricchimento fasi,
i documenti P0-P4 diventano l'interfaccia di
comunicazione tra i ruoli. Gov produce i documenti
di fase, ops li legge e agisce in base a quelli.

Il perimetro di `lettura-documenti` e
`scrittura-documenti` comprende automaticamente le
directory dei documenti di fase (definizione,
analisi, unità).

La segregazione rafforza il principio documento-first
dell'arricchimento fasi: verifica che i documenti
siano effettivamente sufficienti per guidare
l'esecuzione.

---

## Interazione con mcp-operations

I tool MCP di mcp-operations operano sui file di
processo. Un ruolo senza `scrittura-processo` non
deve invocare tool che scrivono file di processo
(create_questione, update_stato, add_commento,
close_questione, create_rfc, write_response_rfc).

Il tool `configure` è un'eccezione: è un'operazione
di sessione e può essere invocato da qualsiasi ruolo.

I tool di lettura (read_questione, read_nota,
read_entries, list_questioni, search_opera) sono
vincolati dall'abilità `lettura-processo`: un ruolo
senza questa abilità non li invoca.

---

## Congruenza con il protocollo base

Questo arricchimento non modifica le regole del
protocollo Hodos. In particolare:

- Il ciclo di approvazione resta invariato
- L'immutabilità del mastro resta invariata
- Le regole di ingaggio restano invariate
- La segregazione aggiunge vincoli al perimetro
  informativo del modello, non ne rimuove

---

## Limiti noti

- La segregazione comportamentale in sessione
  singola non è perfetta: il modello ha in contesto
  le informazioni dei ruoli precedenti e la qualità
  della simulazione dipende dalla vastità del
  contesto e dalla complessità del dominio
- Per una segregazione strutturalmente più
  affidabile, usare sessioni separate per ruolo
- L'arricchimento non fornisce enforcement
  strutturale (mount di volumi, permessi sui file):
  opera esclusivamente a livello di istruzioni
  comportamentali
