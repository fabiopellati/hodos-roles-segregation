# Questioni — Segregazione Ruoli LLM

## Indice

| ID | Titolo | Stato |
|---|---|---|
| QUESTIONE-004 | Lacune nel knowledge base Hodos che inducono l'agente a redigere RFC non conformi | pending-rfc |
| QUESTIONE-003 | Registrazione sorgente llm-roles-segregation in hodos-mcp | pending-rfc |
| QUESTIONE-002 | Incongruenze nel server mcp-operations rilevate durante l'uso | open |
| QUESTIONE-001 | RFC inbound Q133: progettazione arricchimento segregazione ruoli LLM | in-progress |


> Ultima questione inserita: QUESTIONE-004 — 2026-04-24.
> Ultima questione chiusa: —

## QUESTIONE-001 — RFC inbound Q133: progettazione arricchimento segregazione ruoli LLM

**Tipo**: revisione
**Stato**: in-progress

**Storia**

- 2026-04-23 in-progress — RFC inbound accettata, si avvia la fase di progettazione dell'arricchimento
- 2026-04-23 open — RFC inbound Q133: progettazione arricchimento segregazione ruoli LLM

**Descrizione**

Ricevuta RFC dalla QUESTIONE-133 del progetto Hodos. Si richiede la progettazione e realizzazione di un arricchimento indipendente che implementi la segregazione dei ruoli operativi dell'LLM (governance, progettista, operations). L'obiettivo è far emergere lacune documentali che resterebbero invisibili quando lo stesso attore ha visibilità totale sul contesto. L'arricchimento deve definire i tre ruoli con perimetri di lettura e scrittura espliciti, un meccanismo di dichiarazione del ruolo attivo e un comportamento di segnalazione quando le informazioni nel perimetro non sono sufficienti.

**Domande aperte**

- [ ] La segregazione è praticabile a livello di istruzioni comportamentali o richiede un meccanismo di enforcement (ad esempio mount di volumi diversi per ruolo)?
- [ ] La dichiarazione del ruolo avviene per sessione o può cambiare durante la sessione?
- [ ] Il modello è in grado di ignorare efficacemente informazioni che ha già in contesto, o la segregazione funziona solo con sessioni fisicamente separate?
- [ ] Questo arricchimento è compatibile con il modello a sessione singola (un LLM, tutti i ruoli) o richiede sessioni separate per ruolo?

**Impatto**

- rfc/rfc-Q133-segregazione-ruoli.md — RFC di origine, la sezione Response RFC andrà compilata al completamento del lavoro


**Commenti**

COMMENTO-001 — 2026-04-23
Risposte alle domande aperte della RFC, emerse dalla
discussione di progettazione.

D1 — Istruzioni comportamentali o enforcement strutturale?
L'arricchimento opera a livello di protocollo tramite
istruzioni comportamentali. In modalità sessioni separate
la segregazione è di fatto strutturale (il modello non
vede ciò che non ha); in modalità sessione singola è
comportamentale. L'enforcement infrastrutturale esula dal
perimetro di un arricchimento Hodos.

D2 — Ruolo per sessione o cambio in sessione?
Entrambe le modalità sono supportate. In modalità
segregata il ruolo è dichiarato per sessione; in modalità
unificata il ruolo può cambiare durante la sessione. La
scelta spetta all'operatore.

D3 — Il modello può ignorare informazioni già in contesto?
Non in modo perfettamente affidabile. Con modelli
sufficientemente evoluti i risultati sono comunque
soddisfacenti, come dimostrato da sperimentazione
recente. La discriminante è la vastità del contesto e la
complessità del dominio. Il rischio è noto e accettato
dall'operatore quando sceglie la modalità unificata.

D4 — Sessione singola o sessioni separate?
Compatibile con entrambi gli scenari. Sessioni separate
per ruolo come modalità di riferimento (più affidabile);
sessione singola con cambio di ruolo come modalità
alternativa. La scelta è lasciata all'operatore.


COMMENTO-002 — 2026-04-23
Decisioni di design emerse dalla discussione di
progettazione, che estendono e modificano il perimetro
rispetto alla RFC originale.

Generalizzazione dei ruoli.
La RFC propone tre ruoli specifici (governance,
progettista, operations) emersi dalla sperimentazione.
L'arricchimento non deve definire ruoli predefiniti ma
un meccanismo generico. I ruoli concreti sono
configurati dall'opera che abilita l'arricchimento.
I tre ruoli della RFC diventano un esempio di
configurazione possibile.

Catalogo delle abilità discriminabili.
L'arricchimento definisce un catalogo di abilità che
possono essere assegnate o negate a un ruolo. Le
abilità sono le dimensioni della segregazione:
- lettura dei file di processo
- scrittura dei file di processo
- lettura dei documenti di progetto
- scrittura dei documenti di progetto
- lettura degli artefatti
- scrittura degli artefatti
- comunicazione diretta con l'operatore
- comunicazione tramite RFC
L'elenco potrà essere esteso in fase di progettazione
dettagliata.

Configurazione dei ruoli.
Ogni opera dichiara i propri ruoli come combinazioni
di abilità dal catalogo. L'arricchimento fornisce il
meccanismo, non i ruoli.

Preset consigliato a due ruoli.
L'arricchimento propone una combinazione pronta
all'uso:
- gov — gestisce il processo (legge e scrive file di
  processo, legge documenti di progetto per validazione,
  non scrive artefatti)
- ops — esegue sugli artefatti (legge documenti di
  progetto, scrive artefatti, non vede i file di
  processo, non governa)

Interazione con l'arricchimento a fasi.
Quando l'opera abilita anche l'arricchimento fasi
P0-P4, i documenti di fase diventano l'interfaccia di
comunicazione tra gov e ops. Gov produce i documenti
di fase, ops li legge e agisce in base a quelli. La
segregazione verifica che quei documenti siano
sufficienti per guidare l'esecuzione.

Segregazione come modalità attivabile.
La segregazione non è una proprietà della sessione né
un obbligo dell'arricchimento. È una modalità
operativa che l'operatore attiva e disattiva a
piacere durante la sessione. L'arricchimento abilitato
rende disponibile la capacità di segregare, non la
impone. Flusso tipico:
- arricchimento abilitato, segregazione non attiva:
  il modello opera con visibilità totale
- l'operatore attiva e dichiara il ruolo: il modello
  si vincola al perimetro
- l'operatore cambia ruolo: il modello assume il nuovo
  perimetro
- l'operatore disattiva: il modello torna a visibilità
  totale
La modalità sessioni separate resta la più affidabile
ma non è un vincolo dell'arricchimento.

Rettifica del COMMENTO-001, D2 e D4.
La risposta alla D2 (ruolo per sessione o cambio in
sessione) è superata: la scelta è sempre a discrezione
dell'operatore. La D4 (sessione singola o separate) è
anch'essa superata: entrambe le modalità sono
supportate, la segregazione è attivabile in qualsiasi
momento indipendentemente dal tipo di sessione.


COMMENTO-003 — 2026-04-24
Risposte alle domande aperte del piano di lavoro,
emerse dalla discussione.

D1 — Comunicazione diretta sempre concessa?
No, è un'abilità concedibile come le altre. Inoltre
il catalogo si arricchisce di una terza modalità di
comunicazione: comunicazione-operatore, in cui il
ruolo comunica con un altro ruolo attraverso
l'operatore come intermediario. Le modalità di
comunicazione sono quindi tre: comunicazione-diretta,
comunicazione-rfc, comunicazione-operatore.

D2 — Formato verbose o compatto?
Compatto: l'opera dichiara solo le abilità concesse,
le altre sono negate per default. Lo skill contiene il
catalogo completo delle abilità, così il modello sa
sempre quali sono tutte le dimensioni e può
determinare per differenza quali sono negate nel ruolo
corrente.

D3 — Preset richiamabili per nome?
Sì. Lo skill contiene la definizione completa dei
preset, il CLAUDE.md dell'opera li richiama con una
riga. Chi vuole personalizzare dichiara la matrice,
chi vuole la combinazione standard usa il preset.

D4 — Chi scrive i documenti di progetto nel preset
a due ruoli?
Gov ha scrittura-documenti. Nel preset a due ruoli
gov è anche responsabile della progettazione. La
segregazione verifica che i documenti prodotti da gov
siano sufficienti per ops. Quando l'opera ha bisogno
di separare anche la responsabilità progettuale,
definisce un terzo ruolo nella propria configurazione.

D5 — Segnalazione bloccante o informativa?
Bloccante. La simulazione deve riprodurre il caso
reale in cui ops sia un attore remoto (umano, team
esterno, LLM in sessione separata) che non ha le
informazioni e si ferma per chiedere chiarimenti.

D6 — Registrazione delle segnalazioni?
Le segnalazioni restano nel flusso della
conversazione. Non vanno nei file di processo perché
sono artefatti della simulazione, non decisioni del
progetto, e riempirebbero il mastro di rumore. Il
prodotto durevole della simulazione sono le direttive
per il CLAUDE.md: quando una lacuna rivela un pattern
ricorrente (ad esempio che i file attività devono
riportare il dettaglio delle query GraphQL per guidare
il frontend), diventa una direttiva che guida la
produzione dei documenti futuri.


COMMENTO-004 — 2026-04-24
Completamento della progettazione, Passi 1-5 del piano.

Passo 1 — Catalogo abilità: definite nove abilità
discriminabili (lettura-processo, scrittura-processo,
lettura-documenti, scrittura-documenti,
lettura-artefatti, scrittura-artefatti,
comunicazione-diretta, comunicazione-rfc,
comunicazione-operatore).

Passo 2 — Configurazione: formato compatto nel
CLAUDE.md, solo abilità concesse, le altre negate
per default. Supporto per preset richiamabili per
nome e ruoli personalizzati.

Passo 3 — Preset gov/ops definito con matrice
completa. Gov: lettura e scrittura processo e
documenti, comunicazione diretta e operatore.
Ops: lettura documenti e artefatti, scrittura
artefatti, comunicazione diretta e operatore.

Passo 4 — Segnalazione bloccante. Il modello si
ferma e dichiara cosa manca, dove si aspetta di
trovarlo, e che non può procedere. La segnalazione
è bidirezionale: ops segnala lacune nei documenti,
gov durante la verifica può scoprire che un
disallineamento è causato da lacune nei propri
documenti (conoscenza implicita non trasferita dal
contesto di processo).

Ciclo completo:
1. gov scrive documenti di progetto
2. ops realizza artefatti basandosi sui documenti
3. gov verifica il lavoro e confronta con le
   aspettative
4. disallineamento da lacuna documentale: gov
   integra i documenti, ops reitera
5. disallineamento da errore ops: gov segnala,
   ops corregge

Passo 5 — Attivazione e disattivazione a comando
dell'operatore in linguaggio naturale. Conferma
di stato ad ogni transizione. La segregazione è
una modalità ambientale, non un obbligo.

---

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


## QUESTIONE-003 — Registrazione sorgente llm-roles-segregation in hodos-mcp

**Tipo**: revisione
**Stato**: pending-rfc

**Storia**

- 2026-04-23 pending-rfc — La registrazione della sorgente richiede intervento del team hodos-mcp, si genera RFC outbound
- 2026-04-24 open — Registrazione sorgente llm-roles-segregation in hodos-mcp

**Descrizione**

L'arricchimento segregazione-ruoli è stato progettato e i suoi artefatti sono pubblicati nel repository con tag v0.1.0. Per renderlo disponibile nel knowledge base hodos-mcp è necessario che il team hodos-mcp registri la sorgente nel server. Questa operazione richiede intervento esterno tramite RFC.

**Questioni collegate**: QUESTIONE-001

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


