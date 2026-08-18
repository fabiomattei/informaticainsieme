---
title: 'Grafi'
date: '2026-08-18'
author: Fabio Mattei
layout: page
---

![Un grafo pesato non diretto: i nodi rappresentano piazze, gli archi le strade che le collegano](/images/problemsolving/grafi/grafi.svg){:class="aside-image"}

Un **grafo** è l'astrazione di una rete di collegamenti: si può pensare, per esempio, come una carta geografica semplificata. È composto da:

* **nodi** (o vertici): gli "oggetti" del grafo (città, piazze, persone, ...);
* **archi**: i collegamenti tra coppie di nodi.

## Notazione

Un grafo si può descrivere elencando i suoi archi come **termini**:

* se l'arco **non è pesato** e **non diretto** (può essere percorso in entrambe le direzioni), si usa `arco(nodo1,nodo2)`;
* se l'arco **non è pesato** ma **diretto** (può essere percorso solo dal primo al secondo nodo), si usa `freccia(nodo1,nodo2)`;
* se l'arco è **pesato**, si aggiunge un terzo argomento con il peso (detto anche *lunghezza* o *costo* dell'arco): `arco(nodo1,nodo2,peso)` oppure `freccia(nodo1,nodo2,peso)`.

Gli archi diretti si disegnano con una freccia, quelli non diretti con un semplice segmento.

## Alcune definizioni

* Due nodi si dicono **adiacenti** se sono collegati da un arco.
* Il numero di archi che toccano un nodo si dice **grado** del nodo (per i grafi diretti si distingue tra *grado entrante* e *grado uscente*).
* Un **percorso** (o **cammino**) tra due nodi è una sequenza di nodi in cui ciascuno (tranne l'ultimo) è adiacente al successivo; si può descrivere con una lista di nodi. La sua **lunghezza** (se il grafo è pesato) è la somma dei pesi degli archi attraversati.
* Un **ciclo** è un percorso che inizia e finisce nello stesso nodo.
* Un percorso si dice **semplice** se non ripete nodi (e quindi non contiene cicli).

Questi concetti, applicati alla ricerca del percorso più breve e più lungo tra due nodi, sono già stati approfonditi nella pagina su [Il problema del commesso viaggiatore]({{ site.baseurl }}{% link _problemsolving/il-problema-del-commesso-viaggiatore.md %}.html). Qui vediamo altre due applicazioni tipiche: il **problema dell'illuminazione** e un grafo **diretto** con archi pesati.

## Il problema dell'illuminazione

Un comune vuole installare dei lampioni nelle piazze del paese. Il paese è rappresentato da un grafo non diretto e non pesato, in cui i nodi sono le piazze e gli archi le strade che le collegano.

Un lampione posizionato in una piazza illumina:

* la piazza stessa;
* tutte le piazze **direttamente collegate** ad essa (cioè adiacenti).

Si vuole trovare il **numero minimo** di lampioni necessario per illuminare **tutte** le piazze del paese, e la lista delle piazze in cui posizionarli.

#### Esempio

Il paese è descritto dal seguente elenco di termini:

* arco(p1,p2) arco(p2,p3) arco(p3,p4) arco(p4,p5) arco(p5,p6) arco(p6,p1) arco(p1,p4)

Il grafo è un esagono (p1-p2-p3-p4-p5-p6-p1) con in più una diagonale che collega p1 e p4.

Trovare il numero minimo di lampioni e la lista delle piazze in cui posizionarli.

##### Soluzione

| numero minimo di lampioni | 2 |
| lista delle piazze | [p1,p4] |

##### Commenti alla soluzione

Osserviamo il grado di ogni nodo: p1 e p4 hanno grado 3 (sono quelli collegati alla diagonale), tutti gli altri hanno grado 2. Un solo lampione può illuminare al massimo se stesso più i suoi vicini, quindi al più 4 piazze (per un nodo di grado 3): con 6 piazze totali, **un solo lampione non basta mai**.

Proviamo con due lampioni, posizionati proprio nei nodi di grado più alto:

* un lampione in p1 illumina {p1, p2, p6, p4};
* un lampione in p4 illumina {p4, p3, p5, p1}.

Insieme, i due lampioni illuminano {p1, p2, p3, p4, p5, p6}, cioè **tutte** le piazze. Il numero minimo di lampioni è quindi 2, posizionati in [p1,p4].

## Un grafo diretto e pesato

Quando gli archi sono diretti, un percorso può essere seguito solo nel verso della freccia. Questo è tipico, per esempio, delle tratte aeree o dei sensi unici stradali.

#### Esempio

Una piccola compagnia aerea collega quattro città: Aurora, Belveder, Cortesano, Dorsale. I voli disponibili, con il relativo costo, sono descritti dai seguenti termini:

* volo(Aurora,Belveder,120)
* volo(Aurora,Cortesano,90)
* volo(Belveder,Dorsale,80)
* volo(Cortesano,Belveder,40)
* volo(Cortesano,Dorsale,150)
* volo(Dorsale,Aurora,60)

Un viaggio tra due città è descritto da una lista che elenca, in ordine, le città attraversate (comprese partenza e arrivo), senza mai attraversare due volte la stessa città.

Trovare la lista L del viaggio più economico da Aurora a Dorsale, e il suo costo totale.

##### Soluzione

| L | [Aurora,Belveder,Dorsale] |
| costo | 200 |

##### Commenti alla soluzione

Elenchiamo tutti i cammini semplici (senza città ripetute) che da Aurora arrivano a Dorsale, seguendo il verso delle frecce:

| Cammino | Costo |
|---|---|
| [Aurora,Belveder,Dorsale] | 120+80 = 200 |
| [Aurora,Cortesano,Dorsale] | 90+150 = 240 |
| [Aurora,Cortesano,Belveder,Dorsale] | 90+40+80 = 210 |

Il cammino più economico è [Aurora,Belveder,Dorsale], con costo 200.

## Esercizio proposto

Un piccolo villaggio è descritto dal seguente elenco di archi (grafo non diretto, non pesato):

* arco(v1,v2) arco(v2,v3) arco(v3,v4) arco(v4,v5) arco(v5,v1) arco(v1,v3)

Trovare il numero minimo di lampioni necessario per illuminare tutte le piazze e la lista delle piazze in cui posizionarli (ricorda che un lampione illumina se stesso e tutti i suoi vicini diretti).
