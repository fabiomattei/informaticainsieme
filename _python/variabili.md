---
id: 202
title: Variabili
date: '2020-02-07T21:44:05+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=202'
---

Le variabili sono **contenitori di informazioni**. Possiamo pensare ad una varibile come ad un cassetto, dotato di etichetta, che può contenere una informazione.
Lo pensiamo come un cassetto dotato di etichetta perché ogni variabile ha un nome, il nome ci serve per ricordare cosa contiene e per trovarla facilmente tra
le tante variabili che andremo a creare per ciascun nostro programma.

Al fine di creare o inizializzare una variabile Python si avvale dell’operatore di assegnazione indicato dal simbolo **=** in questo modo:

{% highlight python %}
x = 5
y = "John"
z = 3.564
{% endhighlight %}

Facciamo bene attenzione, questa non è una equazione matematica ma una assegnazione, si legge in questo modo: assegno alla variabile x il numero 5.
Significa che nella memoria del computer verrà conservato uno spazio (il cassetto) che avrà come etichetta la **x**. All'interno di questo spasio sarà 
posto il numero 5.

Quindi ricordiamo:
* il simbolo alla sinistra del simbolo = è il **nome della variabile**
* il simbolo alla destra del simbolo = il **valore da conservare** all’interno della variabile.

In questo esempio vengono create tre variabili:

- una variabile x cui viene assegnato il valore 5
- una variabile y cui viene assegnato il valore “John”
- una variabile z cui viene assegnato il valore 3.564

## Nomi di variabile

Una variabile può avere un nome corto (come x e y) oppure un nome più descrittivo (nome, volume\_totale, area, altezza, cognome).

Nello scegliere il nome di una variabile Python dà molta libertà, ci sono però alcune regole da rispettare:

- Il nome di una variabile deve iniziare con una lettera o con il carattere trattino basso (underscore)
- Il nome di una variabile non può iniziare con un numero
- Il nome di una variabile può contenere soltanto simboli alfabetici, maiuscoli e minuscoli, numeri e trattino basso (A-z, 0-9, e \_ )
- I nomi di variabili sono case-sensitive, questo significo che lettere maiuscoli e minuscole vengono riconosciute come diverse. nome, Nome e NOME sono tre nomi di variabile diversi.

#### Variabili e calcoli

E’ possibile utilizzare le variabili per memorizzare i risultati di un calcolo. In questo caso l’operando alla sinistra del simbolo di = è il **nome della variabile**, l’operando alla destra **l’espressione da calcolare**.

Esempio:  
Patrizio va al mercato e compra 6 panini. Ciascun panino costa 2€. Quanto spenderà Patrizio?

{% highlight python %}
numero_panini = 6
costo_panino = 2
costo_totale = numero_panini * costo_panino
{% endhighlight %}

Osserviamo che:

- Alle variabili sono stati dati nomi significativi che seguono le regole elencate in precedenza
- Nelle variabili si memorizzano valori ma non le unità di misura
- Il simbolo \* in informatica è utilizzato per indicare la moltiplicazione
