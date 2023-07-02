---
title: 'Modello entità relazione'
date: '2020-02-16T21:30:07+01:00'
author: Fabio Mattei
layout: page
---

Un modello entità relazione ha lo scopo di descrivere uno schema al fine di incasellare al meglio possibile l’informazione in un sistema informativo. Un modello è formato da **entità**, le cose di interesse che vogliamo ricordare, e le **relazioni** che specificano i legami logici tra queste.

Le entità vengono descritte da rettangoli, le relazioni da rombi.

![Diagramma ER scuola e studente](/images/database/er/er1.png){:class="aside-image"}
Nello schema di fianco abbiamo le entità *Studente* e *Scuola* che sono uniti dalla relazione *Frequenta*.

Con una entità vogliamo descrivere una **classe**, cioè un insieme, di oggetti che appartengono al mondo reale all’interno della quale verranno memorizzate le occorrenze degli oggetti veri e propri che si chiamano **istanze**. Ad esempio l’entità scuola conterrà l’istanza *Patini-Liberatore* e l’entità studente conterrà le istanze *Mario Rossi* ed *Enrichetta Lambertini*.

#### Cardinalità di una relazione

Per ogni entità che è legata ad una relazione viene specificata una **cardinalità di relazione**. Questa è una coppia di numeri o simboli che specifica il **numero minimo e massimo** di istanze di relazione a cui una istanza dell’entità può partecipare.

I simboli generalmente più usati sono sono:

- 0, 1, 2 (qualsiasi numero intero)
- N (nel senso di numero generico maggiore di 1)

![Cardinalità](/images/database/er/er2.png){:class="aside-image"}
La cardinalità che lega lo *studente* alla relazione *frequenta* è (1, 1) questo vuol dire che uno studente frequenta sicuramente almeno una scuola ma non ne può frequentare più di una.

La cardinalità che lega la *scuola* alla relazione *frequenta* è (0, N) questo vuol dire che una scuola potrebbe essere non frequentata da nessuno studente e al massimo potrebbe essere frequentata da N studenti.

#### Attributi

Gli attributi sono oggetti di natura semplice che non possiedono proprietà rilevanti. L’attributo non ha esistenza autonoma ma è associato ad una entità o ad una relazione.

Un attributo ha un **nome univoco** all’interno dell’insieme degli attributi della stessa entità o relazione. Dunque attributi di entità o relazioni diverse possono essere omonimi. Un attributo si rappresenta graficamente con una ellisse.

![Attributi per entità studente](/images/database/er/er3-1.png){:class="aside-image"}
Nell’esempio in alto 4 attributi sono associati all’entità Studente:

* Codice Fiscale
* Nome
* Cognome
* Data Nascita

Questo vuol dire che per ciascuna istanza studente che andremo a memorizzare all’interno di questa entità potremo memorizzare questi 4 attributi.

| **Codice Fiscale** | Nome | Cognome | Data Nascita |
|---|---|---|---|
| MRARSS04D03A345U | Mario | Rossi | 03/04/2004 |
| ENRLMB06D11A345U | Enrichetta | Lambertini | 06/11/2004 |

Analogamente per l’entità scuola potremo memorizzare gli attributi:

* Codice meccanografico
* Nome
* Indirizzo
* Telefono

| **Codice meccanografico** | Nome | Indirizzo | Telefono |
|---|---|---|---|
| AQIS002006 | Patini Liberatore | Via dei Caraceni | 0864 845856 |
| AQRH010008 | De Panfilis    Di Rocco | SS 17 | 086462420 |

Per quanto riguarda la relazione *Frequenta* memorizziamo la sola *data di iscrizione*.

| **Codice meccanografico** | **Codice Fiscale** | Data di iscrizione |
|---|---|---|
| AQIS002006 | MRARSS04D03A345U | 12/1/2019 |
| AQIS002006 | ENRLMB06D11A345U | 16/1/2019 |

Notiamo come, quando andiamo a specificare una istanza di attributo di una relazione, abbiamo bisogno di specificare gli attributi di tipo chiave di entrambe le entità cui questi si riferiscono.

#### Chiave

Al fine di riconoscere in maniera univoca ciascuna istanza, per ciascuna entità occorre specificare una chiave primaria. Una **chiave** è un insieme di uno o più attributi con due caratteristiche:

- **univocità**: i valori degli attributi identificano univocamente le istanze dell’entità *nel micro-mondo modellato*. Vale a dire che nell’universo del discorso dell’applicazione non possono esistere due istanze diverse con lo stesso valore per la chiave;
- **minimalità**: rimuovendo un qualsiasi attributo dall’insieme perdiamo il requisito di univocità.

E’ possibile che esistano più chiavi per una entità. Tali chiavi sono dette **chiavi candidate**. Tra queste occorre sceglierne una che è detta **chiave primaria**. Esistono dei criteri di decisione per la scelta della chiave primaria:

- gli attributi opzionali non possono far parte della chiave primaria. Dunque chiavi che contengono attributi opzionali non possono diventare primarie;
- sono preferibili, per motivi di efficienza, chiavi piccole, cioè con pochi attributi;
- è preferibile, per motivi di efficienza, scegliere chiavi con attributi che vengono utilizzate da molte operazioni o da operazioni molto frequenti;

Se non si riesce a trovare una chiave che soddisfa questi requisiti è possibile introdurre un ulteriore attributo *sintetico* che serve solo ad identificare le istanze dell’entità. Questi attributi sono solitamente detti **codici identificatori** o **id**. (Questo è il motivo per cui ogni medio/grande azienda, quando assume personale, associa un numero di matricola a ciascun dipendente, lo stesso vale per le università che associano un numero di matricola a ciascuno studente).

Nell’esempio precedente abbiamo utilizzato gli attributi Codice Meccanografico e Codice Fiscale come chiavi primarie.

## Esercizi

Si vuole organizzare il campionato di calcetto tra i **comuni** appartenente all’area di Castel Di Sangro. Le **squadre** che si sfideranno apparterranno ai vari comuni ma i **giocatori** avranno la possibilità di iscriversi ad una sola squadra.

1\. Predisporre una breve analisi descrittiva in cui evidenziare le proprie scelte, laddove non siano espressamente indicate dal testo del problema.

2\. Predisporre un’analisi dei dati che, motivando le scelte effettuate, individui:

- Le entità, con breve descrizione delle istanze;
- Gli attributi;
- Le relazioni.

3\. Disegnare il modello E/R
