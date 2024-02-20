---
title: Naive Sort
date: '2024-02-19T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

## Descrizione dell'algoritmo

L'algoritmo esegue ripetute visite dell'insieme da ordinare.
Ad ogni visita dell'insieme l'algoritmo seleziona l'elemento più piccolo e lo scambia con l'elemento al primo posto.
Fatto questo, dato che sono sicuro di avere l'elemento più piccolo al primo posto, si ripete il processo con in sottoinsieme cui viene sottratto il primo elemento ormai ordinato.


{% highlight python %}

def naiveSort(array):
	n = len(arr)
	for j in range(n):
		posmin = j
		min = vet[j]
		for i in range(j+1, N)
			if vet[i] < min: # /* istr. dom. */
				min=vet[i]
				posmin=i
		if posmin != j: # scambia 
			vet[posmin] = vet[j]
			vet[j] = min

data = [9, 5, 1, 4, 3]
naiveSort(data)
print('Sorted Array in Ascending Order:')
print(data)
{% endhighlight %}
  
## Esempio di chiamata

{% highlight python %}
arr = [63, 38, 22, 12, 24, 12, 99]
 
naiveSort(arr)
 
print ("Lista ordinata:")
for i in range(len(arr)):
    print ("%d" %arr[i]),
{% endhighlight %}

## Complessità

L'algoritmo si basa su di un ciclo che visita la lista completamente dall'inizio alla fine e da un sotto-ciclo che visita la parte della lista che è situata a destra dell'elemento chiave. La sotto-visita diventa più corta ad ogni passo della visita principale.
L'Algoritmo ha complessità O(n^2)
