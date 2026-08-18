---
title: 'Gestione di una coda'
date: '2026-08-18'
author: Fabio Mattei
layout: page
---

![In una coda il primo utente ad arrivare è il primo ad essere servito (FIFO)](/images/problemsolving/gestione-coda/gestione-coda.svg){:class="aside-image"}

Nel mondo reale capita spesso di "fare la coda": allo sportello delle poste, alla cassa di un negozio, all'autolavaggio. Una **coda** è una struttura in cui:

* gli **utenti** arrivano e si mettono in attesa di un **servizio**;
* esiste una **struttura di servizio** che elabora un utente alla volta, impiegando una certa **unità di tempo**;
* il **primo** utente ad arrivare è il **primo** ad essere servito: questo meccanismo si chiama **FIFO** (*First In First Out*, primo ad entrare, primo ad uscire).

## Esempio 1: coda con ordine di arrivo fissato

Alla stazione di lavaggio ci sono quattro auto, **A**, **B**, **C**, **D**, già in coda in ordine alfabetico. L'unità di tempo per lavare un'auto è di 15 minuti, e l'auto A entra alle ore 8:30.

Determinare:

1. a che ora inizia il lavaggio dell'auto C;
2. a che ora terminano tutti i lavaggi.

#### Soluzione

Basta costruire la tabella di gestione dei lavaggi, un'auto alla volta, nell'ordine dato:

| Inizio lavaggio | Fine lavaggio | auto |
|---|---|---|
| 8:30 | 8:45 | A |
| 8:45 | 9:00 | B |
| 9:00 | 9:15 | C |
| 9:15 | 9:30 | D |

1. Il lavaggio di C inizia alle **9:00**.
2. Tutti i lavaggi terminano alle **9:30**.

## Esempio 2: coda costruita dall'ordine di arrivo

Allo stesso sportello, sapendo che apre alle ore 9:00 e che l'unità di tempo di servizio è di 10 minuti, arrivano quattro persone ai seguenti orari:

| persona | ora di arrivo |
|---|---|
| A | 9:05 |
| B | 9:00 |
| C | 9:20 |
| D | 9:12 |

Determinare:

1. la lista L che rappresenta la coda (l'ordine in cui le persone vengono servite);
2. a che ora termina il servizio di A;
3. a che ora si esaurisce la coda (tutti serviti).

#### Soluzione

Per prima cosa ordiniamo le persone per orario di arrivo, dalla più antica alla più recente:

| persona | ora di arrivo |
|---|---|
| B | 9:00 |
| A | 9:05 |
| D | 9:12 |
| C | 9:20 |

Quindi **L = [B,A,D,C]**.

Costruiamo ora la tabella dei servizi, tenendo presente che ogni persona può iniziare il servizio solo quando **sia** libero lo sportello (cioè finito il servizio precedente) **sia** arrivata effettivamente (non prima del suo orario di arrivo):

| Inizio servizio | Fine servizio | persona |
|---|---|---|
| 9:00 | 9:10 | B |
| 9:10 | 9:20 | A |
| 9:20 | 9:30 | D |
| 9:30 | 9:40 | C |

1. L = [B,A,D,C]
2. Il servizio di A termina alle **9:20**.
3. La coda si esaurisce alle **9:40**.

## Esempio 3: coda con priorità

In un ufficio postale c'è un solo sportello. Le persone svolgono una delle due azioni: pagamento di una **b**olletta (che richiede **1** unità di tempo) oppure spedizione di un **p**acco (che richiede **2** unità di tempo). L'unità di tempo è di 5 minuti e lo sportello apre alle 8:00. A parità di orario di arrivo, ha la precedenza chi deve pagare una bolletta.

| persona | azione | ora di arrivo |
|---|---|---|
| Marco | pacco | 8:00 |
| Giulia | bolletta | 8:00 |
| Paolo | bolletta | 8:20 |
| Rita | pacco | 8:20 |
| Sara | pacco | 9:00 |

Determinare la lista L dell'ordine di servizio e a che ora termina il servizio di Rita.

#### Soluzione

Alle 8:00 arrivano insieme Marco e Giulia: ha la precedenza Giulia (bolletta). Alle 8:20 arrivano insieme Paolo e Rita: ha la precedenza Paolo (bolletta).

| persona | azione | inizio | fine |
|---|---|---|---|
| Giulia | bolletta | 8:00 | 8:05 |
| Marco | pacco | 8:05 | 8:15 |
| Paolo | bolletta | 8:20 | 8:25 |
| Rita | pacco | 8:25 | 8:35 |
| Sara | pacco | 9:00 | 9:10 |

L = [Giulia,Marco,Paolo,Rita,Sara]. Il servizio di Rita termina alle **8:35**.

## Simulare una coda in Python

Una coda si può rappresentare con una lista: i nuovi utenti si aggiungono in fondo (metodo `append`), mentre l'utente da servire è sempre quello **in testa** (posizione 0), che viene rimosso con `pop(0)`.

{% highlight python %}
coda = []

coda.append("A")   # arriva A, si mette in fondo alla coda
coda.append("B")   # arriva B, si mette in fondo alla coda
coda.append("C")   # arriva C, si mette in fondo alla coda

while len(coda) > 0:
    servito = coda.pop(0)   # estrae ed elimina il primo della coda
    print("Servo:", servito)
{% endhighlight %}

Il programma stampa, nell'ordine di arrivo:

{% highlight python %}
Servo: A
Servo: B
Servo: C
{% endhighlight %}

## Esercizio proposto

Al banco di una gelateria arrivano cinque clienti. Il banco apre alle 15:00 e l'unità di tempo per servire un cliente è di 3 minuti.

| cliente | ora di arrivo |
|---|---|
| Luca | 15:02 |
| Marta | 15:00 |
| Nadia | 15:07 |
| Omar | 15:04 |
| Pia | 15:10 |

Trova la lista L della coda e l'ora in cui termina il servizio dell'ultimo cliente.
