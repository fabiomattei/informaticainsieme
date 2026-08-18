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

## Esercizi dalle gare OPS

I problemi seguenti sono tratti (e adattati) dalle gare a squadre delle Olimpiadi di Problem Solving (OPS) 2026, categoria Secondaria di secondo grado. Prova a risolverli da solo prima di aprire la soluzione.

### Gara 1: il centro postale

In un centro postale ogni cliente riceve un codice che indica il tipo di richiesta: **P** per il pagamento di fatture (**2** unità di tempo) oppure **C** per la consegna di pacchi (**1** unità di tempo). Ha maggiore priorità chi arriva prima; a parità di istante di arrivo, il codice **P** ha sempre la precedenza sul codice **C**.

I clienti arrivati sono descritti dal termine `cliente(<istante di arrivo>,<id cliente>,<codice>)`:

`cliente(01,008,C)`, `cliente(09,006,P)`, `cliente(09,003,P)`, `cliente(04,004,C)`, `cliente(05,002,P)`, `cliente(08,009,P)`, `cliente(02,005,P)`, `cliente(02,007,C)`, `cliente(11,010,C)`, `cliente(03,001,C)`, `cliente(06,011,P)`, `cliente(10,012,C)`

Scrivi la lista L degli id dei clienti nell'ordine in cui vengono serviti allo sportello.

<details markdown="1">
<summary>Soluzione</summary>

Riordinando i clienti per istante di arrivo, e applicando la priorità P > C a parità di istante, si ottiene:

**L = [008,005,007,002,011,009,006,001,004,003,012,010]**

Il cliente 008 arriva per primo (istante 01) e passa subito. I clienti 005 e 007 arrivano insieme (istante 02): passa prima 005 (codice P). Nel frattempo si accumulano in coda 001 e 004; arriva poi 002 che, avendo codice P, ha priorità su di loro; a seguire 011, 009, 006 (priorità P su chi già aspettava); infine passano, in ordine di arrivo, 001, 004, 003, 012, 010.

</details>

### Gara 2: l'autolavaggio

Alla stazione di lavaggio arrivano otto auto, nel seguente ordine:

| auto | ora di arrivo | tipologia lavaggio |
|---|---|---|
| A | 9:05 | completo |
| B | 9:45 | interni |
| C | 10:10 | esterni |
| D | 9:15 | completo |
| E | 9:50 | esterni |
| F | 10:15 | cerchioni |
| G | 9:20 | completo |
| H | 9:55 | esterni |

La stazione apre alle 9:00, non c'è nessuna auto in coda all'apertura e viene lavata un'auto per volta. La durata del lavaggio dipende dal tipo: **30 minuti** per il completo, **20 minuti** per gli interni, **10 minuti** per i soli esterni, **5 minuti** per la pulizia dei cerchioni.

Determina:

1. la lista L che rappresenta la coda delle otto auto (in ordine di arrivo);
2. a che ora termina il lavaggio dell'auto A;
3. a che ora inizia il lavaggio della penultima auto;
4. a che ora tutte le otto auto sono state lavate.

<details markdown="1">
<summary>Soluzione</summary>

Ordinando per ora di arrivo si ottiene L = [A,D,G,B,E,H,C,F].

| Inizio | Fine | auto |
|---|---|---|
| 9:05 | 9:35 | A |
| 9:35 | 10:05 | D |
| 10:05 | 10:35 | G |
| 10:35 | 10:55 | B |
| 10:55 | 11:05 | E |
| 11:05 | 11:15 | H |
| 11:15 | 11:25 | C |
| 11:25 | 11:30 | F |

1. **L = [A,D,G,B,E,H,C,F]**
2. Il lavaggio di A termina alle **9:35**.
3. La penultima auto (C) inizia il lavaggio alle **11:15**.
4. Tutte le auto sono lavate per le **11:30**.

</details>

### Gara 3: la palestra

Allo sportello prenotazioni di una palestra ogni utente riceve un codice di servizio: **P** (partita ufficiale, **3** unità di tempo, priorità massima), **A** (allenamento programmato, **2** unità di tempo, priorità media), **R** (riscaldamento libero, **1** unità di tempo, priorità minima). Ha maggiore priorità chi arriva prima; a parità di istante vale l'ordine P > A > R.

Gli utenti sono descritti dal termine `utente(<istante arrivo>,<id utente>,<codice servizio>)`:

`utente(01,009,R)`, `utente(03,005,P)`, `utente(03,002,A)`, `utente(05,004,P)`, `utente(05,006,A)`, `utente(07,008,R)`, `utente(06,003,A)`, `utente(06,007,P)`, `utente(08,011,A)`, `utente(09,001,R)`, `utente(10,010,P)`

Rispondi alle seguenti domande:

1. Scrivi la lista degli id degli utenti in ordine di accesso alla palestra.
2. A che unità di tempo termina l'attività dell'utente 004?
3. Quante prenotazioni di tipo A sono presenti nella lista?
4. A che unità di tempo tutti gli utenti hanno completato la propria attività?

<details markdown="1">
<summary>Soluzione</summary>

1. **[009,005,004,007,010,002,006,003,011,008,001]**
2. L'utente 004 termina alla **9ª** unità di tempo.
3. Ci sono **4** prenotazioni di tipo A.
4. Tutti gli utenti terminano l'attività alla **25ª** unità di tempo.

L'utente 009 è il primo ad arrivare e passa subito. All'istante 03 arrivano insieme 005 (P) e 002 (A): passa prima 005, priorità maggiore. Agli istanti successivi si applica sempre la stessa regola (prima chi è arrivato prima; a parità di istante, prima P, poi A, poi R), fino a esaurire la coda.

</details>

### Gara 4 (Finale): il pronto soccorso

Al pronto soccorso i pazienti ricevono un codice triage a 4 livelli: **R** (rosso, critico, **4** unità di tempo), **B** (blu, molto urgente, **3** unità), **G** (giallo, urgente, **2** unità), **V** (verde, non urgente, **1** unità). Ha priorità chi arriva prima; a parità di istante vale l'ordine R > B > G > V; a parità di istante e codice, passa prima il paziente con id numerico minore.

I pazienti sono descritti dal termine `paziente(<istante arrivo>,<id>,<codice>)`:

`paziente(02,007,V)`, `paziente(01,003,G)`, `paziente(03,001,B)`, `paziente(02,008,R)`, `paziente(05,005,G)`, `paziente(05,004,R)`, `paziente(06,009,B)`, `paziente(04,006,V)`, `paziente(07,002,G)`, `paziente(08,010,B)`, `paziente(03,011,R)`

Rispondi alle seguenti domande (per le domande 2 e 4 indica l'unità di tempo *successiva* all'ultima in cui il paziente è rimasto in pronto soccorso):

1. Scrivi la lista dei pazienti in ordine di accesso al pronto soccorso.
2. A che unità di tempo termina il servizio del paziente 001?
3. Quanti pazienti con codice G sono presenti nella lista?
4. A che unità di tempo tutti i pazienti sono stati visitati?

<details markdown="1">
<summary>Soluzione</summary>

1. **[003,008,011,004,001,009,010,005,002,007,006]**
2. Il servizio del paziente 001 termina all'unità di tempo **18**.
3. Ci sono **3** pazienti con codice G.
4. Tutti i pazienti sono visitati entro l'unità di tempo **30**.

Riordinando per istante di arrivo: 003 (istante 01) parte subito. All'istante 02 arrivano 007 (V) e 008 (R): passa prima 008, priorità massima. All'istante 03 arrivano 001 (B) e 011 (R): passa prima 011. Si prosegue applicando sempre la stessa regola (arrivo, poi R>B>G>V, poi id minore) fino a smaltire tutti i pazienti.

</details>
