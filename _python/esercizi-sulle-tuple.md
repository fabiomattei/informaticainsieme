---
id: 121
title: 'Le tuple'
date: '2020-02-04T15:18:20+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=121'
---

Le tuple sono strutture dati analoghe alle liste. La differenza tra tuple e liste risiede nel fatto che una lista è mutabile, possono dunque modificarla aggiungendo e rimuovendo un elemento da questa, una tupla al contrario, una volta definita non può essere modificata non è possibile aggiungere o rimuovere elementi ad una tupla in un secondo momento. Tutti gli operatori che abbiamo visto per le liste (min, max, len, in, not in) sono utilizzabili anche con le tuple. Ovviamente non potremo utilizzare l’operatore del.

Viene spontaneo chiedersi che senso abbiamo per un linguaggio definire due strutture dati analoghe ma una con meno potenzialità dell’altra. La risposta risiede nel bisogno di prestazioni, una struttura che ha meno po- tenzialità risulta molto più snella per un elaboratore dunque occupa meno memoria ed offre prestazioni migliori in termini di computabilità.

{% highlight python %}
numeri = (0, 1, 2, 3) # tupla di 4 elementi di tipo int 
lettere = ('a', 'b', 'c', 'd', 'e') # tupla di 5 elementi di tipo char 
parole = ('mattina', 'pomeriggio') # tupla di 2 elementi di tipo str
{% endhighlight %}

</div>Le tuple vengono definite attraverso le parentesi tonde. Cosa fa secondo te il seguente algoritmo?

{% highlight python %}
vowels = ('a', 'e', 'i', 'o', 'u') 
letters = ('a', 'b', 'c', 'd', 'e') 
for x in letters:
    if x in vowels:
        print( x + " is a vowel ")
    else:
        print( x + " is a consonant ")
{% endhighlight %}

</div>Il seguente frammento di codice stampa il quadrato di ogni numero di seq.

{% highlight python %}
seq = (1, 2, 3, 4, 5)
for n in seq:
    print('Il quadrato di ', n, ' e\' ', n**2)
{% endhighlight %}

</div>**Esercizio 1:** Scrivi un programma Python che crei una tupla con 10 numeri interi e li stampi sul video con un ciclo for (print)

**Esercizio 2:** Scrivi un programma Python che crei una tupla con 10 numeri interi e 10 stringhe e stampi il tutto sul video con un ciclo for (print)

**Esercizio 3:** Scrivi un programma Python che trasformi la tupla del punto 2 in una stringa e la stampi (usa l’operatore + di concatenazione stringa)

**Esercizio 4:** Scrivi un programma Python che trovi l’indice di un elemento all’interno di una tupla

**Esercizio 5:** Scrivi un programma Python che inverta l’ordine degli elementi in una tupla (crea una nuova tupla con gli elementi invertiti: ciclo for e operatore di merge + fra tuple)

**Esercizio 6:** Scrivi un programma Python che trovi gli elementi comuni tra due tuple

Ricorda che:

{% highlight python %}
# posso suddividere una tupla
tuplex = 4, 8, 3 
n1, n2, n3 = tuplex
{% endhighlight %}

</div>**Esercizio 7:** Scrivi un programma Python che suddivida una tupla costituita da 10 interi e li mostri a video

Ricorda che:

{% highlight python %}
# lunghezza di una tupla
tuplex = 4, 8, 3 
len(tuplex)  # ritorna 3
{% endhighlight %}

</div>**Esercizio 8:** Scrivi un programma Python che prenda da una tupla il secondo elemento e il penultimo e li scriva a monitor

**Esercizio 9:** Scrivi un programma Python che scriva in ordine crescente gli elementi contenuti all’interno di una tupla senza modificare la stessa.

**Esercizio 10:** Scrivi un programma Python che definisca una lista di tuple di due elementi  
 lista = \[(“Gianni”, 10), (“Michele”, 12), (“Cristina”, 18)\]  
 e scandendo la lista ne scriva a monitor tutto il contenuto

**Esercizio 11:** Scrivi un programma Python che scriva gli elementi del punto 10 prima in ordine alfabetico e poi in ordine numerico.