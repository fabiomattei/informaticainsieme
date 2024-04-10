---
title: Algoritmi di ricerca
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

## Ricerca lineare o sequenziale

L'algoritmo controlla in sequenza gli elementi dell'insieme arrestandosi quando ne trova uno che soddisfa il criterio di ricerca. Non potendosi avvalere di alcun ordinamento tra gli elementi l'algoritmo può concludere con certezza che l'insieme non contiene alcun elemento corrispondente solo dopo averli verificati tutti, richiedendo pertanto un numero di controlli, nel caso peggiore, pari alla cardinalità dell'intero insieme. 
La ricerca sequenziale ha complessità O(n) nel caso peggiore a O(n/2) nel caso medio e O(1) se l'elemento si trova in prima posizione. 

{% highlight python %}
def ricercaSequenziale(L, ricercato):
    for i in range(len(L))
        if L[i] == ricercato:
            return i                            # trovato, restituisco l'indice      
    return -1                                   # non trovato
{% endhighlight %}

## Ricerca binaria o dicotomica

L'algoritmo cerca un elemento all'interno di una lista. La lista deve necessariamente essere ordinata in ordine crescente.
L'algoritmo effettua mediamente meno confronti rispetto ad una ricerca sequenziale perché, sfruttando l'ordinamento, dimezza l'intervallo di ricerca ad ogni passaggio.

La ricerca binaria non usa mai più di log(N) in base 2 (logaritmo base 2 di N approssimato per eccesso) confronti per una search hit o una search miss. Tuttavia, a meno che non si controllino ad ogni iterazione i valori dei due indici estremi, la ricerca binaria impiega effettivamente sempre lo stesso tempo su uno stessa lista per cercare elementi anche in posizioni diverse.

{% highlight python %}
def ricercaBinaria(L, ricercato):
    start = 0
	end = len(L)
	centro = 0
    while start <= end:
        centro = (start + end) // 2
        if ricercato == L[centro]:
            return centro                       # trovato, restituisco l'indice
        if ricercato < L[centro]:
            end = centro - 1
        if elemento > L[centro]:
            start = centro + 1
    return -1                                   # non trovato
{% endhighlight %}