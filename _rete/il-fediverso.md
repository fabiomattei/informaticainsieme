---
title: 'Il Fediverso: server distribuiti e proprietà dei dati'
date: '2026-08-27T09:00:00+02:00'
author: Fabio Mattei
layout: page
---

Quando ci iscriviamo a un social network come Instagram o X (ex Twitter) diamo per scontata una cosa: da qualche parte, in un data center di proprietà dell'azienda, esiste un server (o meglio, migliaia di server) su cui vengono salvati i nostri post, i nostri messaggi, la lista dei nostri contatti. Tutto ciò che scriviamo appartiene, di fatto, a un'unica infrastruttura controllata da un'unica azienda.

Il **Fediverso** (crasi di "federazione" e "universo") propone un modello radicalmente diverso, ed è proprio nella gestione dei server e in dove risiede l'informazione che si gioca la differenza principale rispetto ai social tradizionali.

## Un social, tanti server: cosa significa "federato"

Un social network classico è **centralizzato**: esiste un solo insieme di server, gestito da una sola organizzazione, che ospita i dati di tutti gli utenti del pianeta. Se quell'azienda cambia le regole, chiude un servizio o subisce un guasto, tutti gli utenti ne sono coinvolti allo stesso modo, perché non esiste alternativa: i dati vivono lì e da nessun'altra parte.

Il Fediverso funziona invece come una rete di tanti piccoli social indipendenti, chiamati **istanze** (o server), che si parlano tra loro tramite un protocollo comune, **ActivityPub**. Ogni istanza:

* è gestita da una persona, un'associazione o una piccola comunità, non da una multinazionale;
* ha un proprio database, proprie regole di moderazione e un proprio codice di condotta;
* ospita solo gli account delle persone che si sono iscritte a *quella* istanza;
* comunica con le altre istanze scambiandosi messaggi in formato standard, in modo che un utente su un server possa seguire, commentare o ricevere risposte da un utente iscritto su un server completamente diverso.

Il risultato è che non esiste "il server di Mastodon", allo stesso modo in cui non esiste "il server della posta elettronica": esistono migliaia di server Mastodon (e Pixelfed, PeerTube, Lemmy e altri software compatibili) che insieme formano una rete, esattamente come migliaia di provider di posta elettronica indipendenti fanno funzionare l'email nel suo complesso.

## Dove risiede davvero l'informazione

Questa è la differenza più importante, e la meno intuitiva per chi arriva da un social centralizzato.

Nel Fediverso, **i tuoi dati risiedono fisicamente sul server dell'istanza a cui ti sei iscritto**, non su un server "del Fediverso" in generale (perché un server unico del Fediverso non esiste). Se ti iscrivi a mastodon.uno, il tuo profilo, i tuoi post e le tue relazioni sociali sono salvati nel database di mastodon.uno, gestito da chi amministra quell'istanza.

Quando un utente di un'altra istanza (per esempio mastodon.social) ti segue, il tuo server invia una **copia** dei tuoi post al suo server, che la mostra nella sua timeline. Ogni istanza mantiene quindi una copia parziale e sparsa dei contenuti a cui i suoi utenti sono interessati, ma l'informazione "originale" e autorevole resta sempre presso il server di appartenenza dell'autore.

Le conseguenze pratiche di questo modello sono diverse:

* **Nessun punto unico di controllo**: non esiste un'azienda che possa decidere, da sola, le regole per tutta la rete. Ogni amministratore di istanza decide le proprie politiche di moderazione, e può scegliere di "defederare" (cioè smettere di scambiare dati) con istanze che non rispettano determinati standard.
* **Nessun punto unico di guasto**: se un'istanza va offline, gli utenti delle altre istanze continuano a funzionare normalmente; solo gli iscritti a quel server specifico sono temporaneamente irraggiungibili.
* **Portabilità dell'identità**: se non sei più soddisfatto delle regole della tua istanza, puoi migrare il tuo account su un'altra, portando con te follower e following (i post restano tipicamente sul server originario, salvo esportazione).
* **Fiducia distribuita, non centralizzata**: la scelta dell'istanza non è un dettaglio tecnico, ma una scelta di fiducia verso chi la amministra, perché è quella persona (o quel piccolo team) ad avere accesso al database con i tuoi dati, non una corporation con policy uniformi decise a livello globale.

Questo è l'opposto esatto della logica dei social centralizzati, dove tutti i dati confluiscono in un unico enorme "silo" controllato da un solo soggetto, che può essere acquisito, cambiare termini di servizio, monetizzare i dati o chiudere improvvisamente, portando con sé tutti i contenuti degli utenti.

## Le coordinate di mastodon.uno

Per rendere concreto il discorso, ecco un esempio reale di istanza del Fediverso, quella su cui è attivo anche l'autore di questo sito:

* **Indirizzo dell'istanza**: [https://mastodon.uno](https://mastodon.uno)
* **Software**: Mastodon (client compatibile con il protocollo ActivityPub)
* **Community**: istanza italiana generalista, aperta alla registrazione
* **Profilo dell'autore su questa istanza**: [@fabiomattei@mastodon.uno](https://mastodon.uno/@fabiomattei)

L'indirizzo completo `@fabiomattei@mastodon.uno` non è un semplice username: è la vera "coordinata di rete" dell'account, composta da due parti che rispecchiano esattamente il funzionamento del Fediverso descritto sopra:

1. `fabiomattei` identifica l'utente **all'interno** dell'istanza;
2. `mastodon.uno` identifica **quale server** ospita fisicamente quell'account e i suoi dati.

Esattamente come un indirizzo email (`utente@dominio`), questa struttura rende esplicito, fin dal nome dell'account, dove risiede davvero l'informazione: non "nel Fediverso" in astratto, ma su un server preciso, amministrato da persone precise, con regole precise.

## In sintesi

| | Social centralizzato | Fediverso |
|---|---|---|
| Numero di server | Uno (o un'infrastruttura unica) | Migliaia, indipendenti tra loro |
| Chi li gestisce | Un'unica azienda | Amministratori diversi per ogni istanza |
| Dove sono i dati | In un unico grande database aziendale | Distribuiti sui server delle singole istanze |
| Regole di moderazione | Uniche, decise centralmente | Diverse per ogni istanza |
| Punto di guasto | Singolo, coinvolge tutti | Locale, coinvolge solo un'istanza |
| Protocollo di comunicazione | Proprietario, chiuso | Aperto e standard (ActivityPub) |

Capire questa architettura distribuita è la chiave per capire perché il Fediverso viene spesso descritto non come "un social alternativo", ma come "un modo diverso di concepire i social": non un prodotto di un'azienda, ma un protocollo di rete aperto su cui chiunque può costruire e ospitare la propria comunità.
