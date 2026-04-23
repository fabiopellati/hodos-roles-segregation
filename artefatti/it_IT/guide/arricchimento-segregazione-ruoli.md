---
tipo-artefatto: guida
documento: arricchimento-segregazione-ruoli
descrizione: >-
  Arricchimento per la segregazione dei ruoli operativi
  dell'LLM, con perimetri informativi configurabili per
  far emergere lacune documentali
autorita: informativa
---

# Arricchimento — Segregazione Ruoli

## Cos'è

La segregazione dei ruoli è un arricchimento Hodos
che introduce una modalità operativa in cui il modello
si vincola deliberatamente al perimetro informativo
di un ruolo, ignorando le informazioni che non
sarebbero disponibili a quel ruolo.

Il valore non è la segregazione in sé ma la capacità
di far emergere lacune documentali che resterebbero
invisibili quando lo stesso attore ha visibilità
totale sul contesto. Quando il modello opera come
operations e i documenti di progetto non contengono
informazioni sufficienti per procedere, si ferma e
segnala la lacuna anziché compensare silenziosamente
con informazioni fuori perimetro.

## Quale problema risolve

In un progetto gestito con Hodos, lo stesso LLM può
operare in ruoli diversi: chi governa il processo,
chi progetta, chi esegue. Quando tutti i ruoli sono
assunti dallo stesso attore nella stessa sessione,
il modello ha accesso a tutto il contesto e compensa
implicitamente le carenze dei documenti di progetto
con informazioni che un attore con il solo perimetro
del ruolo non avrebbe.

Il risultato è che i documenti di progetto sembrano
adeguati perché l'esecuzione produce risultati
accettabili, ma in realtà sono incompleti. Il
problema emerge quando il ruolo di esecuzione viene
delegato a un attore esterno (umano, team, o LLM in
sessione separata) che non ha quella visibilità: i
documenti non bastano e il lavoro si blocca.

La segregazione anticipa questo problema e lo rende
visibile prima della delega.

## Quando è utile

L'arricchimento è utile quando:

- i documenti di progetto devono essere autosufficienti
  per guidare l'esecuzione, perché l'esecuzione sarà
  delegata o potrebbe esserlo in futuro
- si vuole verificare la qualità dei documenti di
  progetto prima di procedere con la realizzazione
- si vuole simulare il punto di vista di un attore
  esterno per individuare lacune informative
- il progetto prevede ruoli distinti con perimetri
  informativi diversi

## Come si abilita

Nel CLAUDE.md dell'opera, aggiungere l'arricchimento
alla sezione `Arricchimenti abilitati`:

```markdown
## Arricchimenti abilitati

- segregazione-ruoli
```

Aggiungere la sezione di configurazione dei ruoli.
La forma più semplice usa il preset a due ruoli:

```markdown
## Segregazione ruoli

**Preset**: gov/ops
```

Per ruoli personalizzati, dichiarare le abilità
concesse per ciascun ruolo (consultare lo skill per
il catalogo completo delle abilità e la sintassi di
configurazione).

## Come funziona

La segregazione è una modalità ambientale che
l'operatore attiva e disattiva a comando durante la
sessione. L'abilitazione dell'arricchimento rende
disponibile la capacità di segregare, non la impone.

Il ciclo tipico:

- l'operatore attiva la segregazione e dichiara il
  ruolo ("adesso sei ops")
- il modello si vincola al perimetro del ruolo
- se le informazioni nel perimetro non bastano, il
  modello si ferma e segnala la lacuna
- l'operatore risolve la lacuna (cambiando ruolo,
  integrando il documento, o disattivando la
  segregazione)
- il modello riprende

## Interazione con l'arricchimento a fasi

Quando l'opera abilita anche l'arricchimento fasi
P0-P4, i documenti di fase diventano l'interfaccia
di comunicazione tra i ruoli. Chi governa produce i
documenti di fase, chi esegue li legge e agisce in
base a quelli. La segregazione verifica che quei
documenti siano sufficienti per guidare l'esecuzione.

## Prerequisiti

Nessuno. L'arricchimento è indipendente e compatibile
con il protocollo Hodos base. L'interazione con
l'arricchimento a fasi P0-P4 è opzionale e si attiva
automaticamente quando entrambi gli arricchimenti
sono abilitati.

## Limiti noti

La segregazione è comportamentale, non strutturale.
In sessione singola con cambio ruolo, il modello ha
in contesto le informazioni dei ruoli precedenti e
deve ignorarle deliberatamente. Con modelli
sufficientemente evoluti i risultati sono
soddisfacenti, ma la fedeltà della simulazione
dipende dalla vastità del contesto e dalla
complessità del dominio.

Per una segregazione strutturalmente più affidabile,
usare sessioni separate per ruolo: ogni sessione ha
accesso solo ai file del proprio perimetro.
