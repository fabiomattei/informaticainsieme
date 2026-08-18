---
title: 'Gestione di una pila'
date: '2026-08-18'
author: Fabio Mattei
layout: page
---

![In una pila l'ultimo elemento inserito è il primo ad essere estratto (LIFO)](/images/problemsolving/gestione-pila/gestione-pila.svg){:class="aside-image"}

Una **pila** (o **stack**) è una sequenza di elementi in cui è possibile aggiungere o togliere elementi soltanto da un estremo, detto **testa** della pila. Il meccanismo si chiama **LIFO** (*Last In First Out*: l'ultimo ad entrare è il primo ad uscire).

Esempi di pila nella vita reale: una pila di piatti (l'ultimo appoggiato è il primo che si toglie), un tubetto di pastiglie, una pila di libri.

Le due operazioni fondamentali su una pila sono:

* **push**: inserisce un elemento in cima alla pila;
* **pop**: estrae (e rimuove) l'elemento che si trova in cima alla pila.

## Esempio 1: consegne in ordine inverso

Un corriere deve consegnare un pacco a cinque clienti, nel seguente ordine: **Anna**, **Bruno**, **Carla**, **Dario**, **Elisa**. Il furgone però è organizzato come una pila: l'ultimo pacco caricato è il primo che si può scaricare.

Come deve caricare (push) i pacchi il corriere, in modo da poterli consegnare esattamente nell'ordine richiesto?

#### Soluzione

Poiché la consegna di Anna deve avvenire per prima, il suo pacco deve trovarsi in cima alla pila, e quindi deve essere **l'ultimo** ad essere caricato. Il caricamento deve quindi avvenire nell'ordine **inverso** rispetto alla consegna:

L = [Elisa, Dario, Carla, Bruno, Anna]

Caricando i pacchi in quest'ordine (push di Elisa, poi Dario, poi Carla, poi Bruno, infine Anna), lo scarico (pop) avviene nell'ordine Anna, Bruno, Carla, Dario, Elisa: esattamente quello richiesto.

## Esempio 2: sequenza di colori

Un fuochista deve inserire delle cariche colorate in una scatola pirotecnica, che verrà poi svuotata (in ordine LIFO) durante lo spettacolo. Le cariche sono etichettate con un codice numerico:

* colore(21,rosso)
* colore(22,blu)
* colore(23,verde)
* colore(24,giallo)
* colore(25,viola)

Il cliente vuole vedere in cielo, in quest'ordine: **blu, viola, rosso, verde, giallo**.

Scrivere la lista L di caricamento (push) della scatola, usando i codici numerici.

#### Soluzione

| L | [24,23,21,25,22] |

#### Commenti alla soluzione

Poiché la scatola è una pila, l'ordine di uscita (pop) è **l'inverso** dell'ordine di ingresso (push). Per ottenere in uscita la sequenza blu, viola, rosso, verde, giallo, dobbiamo caricare la scatola nell'ordine **opposto**: giallo, verde, rosso, viola, blu, cioè, in codici: 24, 23, 21, 25, 22.

Verifichiamo: caricando (push) 24, 23, 21, 25, 22 e poi scaricando (pop) via via l'elemento in cima, si estrae prima 22 (blu), poi 25 (viola), poi 21 (rosso), poi 23 (verde), infine 24 (giallo): esattamente la sequenza richiesta.

## Simulare una pila in Python

In Python, una lista si comporta già come una pila se si usano solo i metodi `append` (per il push, aggiunge in fondo alla lista) e `pop` senza argomenti (per il pop, rimuove ed estrae l'ultimo elemento della lista).

{% highlight python %}
pila = []

pila.append("Elisa")   # push
pila.append("Dario")   # push
pila.append("Carla")   # push

while len(pila) > 0:
    estratto = pila.pop()   # pop: rimuove ed estrae l'ULTIMO elemento
    print("Consegno a:", estratto)
{% endhighlight %}

Il programma stampa:

{% highlight python %}
Consegno a: Carla
Consegno a: Dario
Consegno a: Elisa
{% endhighlight %}

Si noti la differenza rispetto alla coda: lì si usava `pop(0)` (si estrae il **primo** elemento), qui si usa `pop()` senza argomenti (si estrae l'**ultimo** elemento).

## Esercizio proposto

Un bibliotecario deve restituire dei libri a quattro persone, che si presenteranno in quest'ordine: **Marco**, **Sofia**, **Luigi**, **Elena**. Il bibliotecario tiene i libri impilati su un carrello (una pila).

Scrivi la lista L dell'ordine in cui il bibliotecario deve impilare (push) i libri sul carrello, in modo da poterli consegnare (pop) esattamente nell'ordine di arrivo delle persone.
