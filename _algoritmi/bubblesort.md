---
title: Bubble Sort
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

## Descrizione dell'algoritmo

L'algoritmo esegue ripetute visite dell'insieme da ordinare.
Ad ogni visita dell'insieme l'algoritmo confronta gli elementi due alla volta, nel caso in cui il primo elemento sia più grande del secondo l'algoritmo scambia gli elementi.
Alla fine della visita sono __sicuro__ che di avere l'elemento più grande in fondo alla lista.
Ripeto il procedimento considerando la lista cui viene sottratto l'elemento ormai nella giusta posizione.

![Esempio](/images/algoritmi/bubblesort/bubblesort.png){:class="aside-image"}

{% highlight python %}
def bubbleSort(arr):
    n = len(arr)
    # Si scandisce la lista elemento per elemento
    for i in range(n):
        # Gli ultimi i elementi sono gia' in rodne
        for j in range(0, n-i-1):
            # Se gli elementi da 0 ad i-1 non sono i ordine si invertono
            if arr[j] > arr[j+1] :
                arr[j], arr[j+1] = arr[j+1], arr[j]
 
## Esempio di chiamata
arr = [63, 38, 22, 12, 24, 12, 99]
 
bubbleSort(arr)
 
print ("Lista ordinata:")
for i in range(len(arr)):
    print ("%d" %arr[i]),
{% endhighlight %}

## Complessità

L'algoritmo si basa su di un ciclo che visita la lista completamente dall'inizio alla fine e da un sotto-ciclo che visita la parte della lista che è situata a destra dell'elemento chiave. La sotto-visita diventa più corta ad ogni passo della visita principale.
L'Algoritmo ha complessità O(n^2) nel caso peggiore di una lista ordinata inzialmente in senso opposto a quello desiderato. 
Ha una complessità media O(n log(n)).

