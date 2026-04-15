---
title: Laboratorio sulla complessità
date: '2026-02-09T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

Proviamo a cercare una evidenza sperimentale di quanto studiato sulla complessità.

Faremo questo andando a popolare la seguente tabella che riserva una riga ad ogni algoritmo che sappiamo appartenere
ad una certa **classe di complessità** ed una colonna per ogni N che rappresenta una diversa **dimensione del problema**. 

| Algoritmo | N=10<sup>3</sup> | N=10<sup>4</sup>  | N=10<sup>6</sup> | N=10<sup>9</sup> |
| --------- | ---------------- | ----------------- | ---------------- | ---------------- |
| .....     | .....            | .....             | ......           | ......           |

Al fine di determinare il tempo speso dalla macchina per risolvere un algoritmo utilizzeremo la libreria **timeit** nativa
di Python. Faremo questo memorizzando nella varibile **start** il tempo immediatamente precedente all'inizio della soluzione dell'algoritmo e nella variabile **stop** il tempo immediatamente successivo.

{% highlight python %}
import timeit

start = timeit.default_timer()

# Programma

stop = timeit.default_timer()
print('Time: ', stop - start)
{% endhighlight %}

## Formula di Gauss

Sappiamo dalla teoria che questo algoritmo ha complessità costante, andiamo a verificare la cosa sperimentalmente. 

{% highlight python %}
import timeit

start = timeit.default_timer()

N=10**3
somma = N*(N+1)/2

stop = timeit.default_timer()
print('Time: ', stop - start)
{% endhighlight %}

Andando a sostituire ad N i valori definiti in tabella otteniamo i seguenti risultati:

| Algoritmo    | N=10<sup>3</sup>  | N=10<sup>4</sup>  | N=10<sup>6</sup>  | N=10<sup>9</sup>  |
| ------------ | ----------------- | ----------------- | ----------------- | ----------------- |
| Gauss        | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 4*10<sup>-6</sup> |

Possiamo vedere che il tempo non cambia in modo significativo al variare della dimensione del problema dunque la teoria 
è provata. 

## Somma di N numeri con ciclo

Questo algoritmo dovrebbe essere un O(N), andiamo a verificarlo.

{% highlight python %}
import timeit

start = timeit.default_timer()

N=10**3
acc = 0
for cont in range(1,N):
    acc = acc + cont

stop = timeit.default_timer()
print('Time: ', stop - start)
{% endhighlight %}

Otteniamo i seguenti valori sperimentali

| Algoritmo    | N=10<sup>3</sup>  | N=10<sup>4</sup>  | N=10<sup>6</sup>  | N=10<sup>9</sup>  |
| ------------ | ----------------- | ----------------- | ----------------- | ----------------- |
| Gauss        | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 4*10<sup>-6</sup> |
| Ciclo        | 3*10<sup>-4</sup> | 9*10<sup>-4</sup> | 0,1               | 92                |

## Generazione Lista

Per contnuare nelle nostre prove sperimentali abbiamo bisogno di una lista contenente numeri. 
Proviamo a vedere quanto tempo viene impiegato per la generazione di una lista.

{% highlight python %}
import random
import timeit

start = timeit.default_timer()

N = 10**3
lista = []

for cont in range(1,N):
    lista.append(random.randint(0, 1000000))

stop = timeit.default_timer()
print('Time: ', stop - start)
{% endhighlight %}

Anche la generazione di una lista ha le caratteristiche di un algoritmo O(N)

| Algoritmo    | N=10<sup>3</sup>  | N=10<sup>4</sup>  | N=10<sup>6</sup>  | N=10<sup>9</sup>  |
| ------------ | ----------------- | ----------------- | ----------------- | ----------------- |
| Gauss        | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 4*10<sup>-6</sup> |
| Ciclo        | 3*10<sup>-4</sup> | 9*10<sup>-4</sup> | 0,1               | 92                |
| Gen. Lista   | 1*10<sup>-2</sup> | 1*10<sup>-1</sup> | 1                 | 1000              |

## Ricerca Lineare

La ricerca lineare consiste nel cercare le occorrenze di un numero all'interno di una lista.

{% highlight python %}
import random
import timeit

N = 10**3
lista = []

for cont in range(1,N):
    lista.append(random.randint(0, 1000000))

start = timeit.default_timer()

numero = 477
for k in lista:
    if k==477:
	    print(k)

start = timeit.default_timer()


stop = timeit.default_timer()
print('Time: ', stop - start)
{% endhighlight %}

| Algoritmo    | N=10<sup>3</sup>  | N=10<sup>4</sup>  | N=10<sup>6</sup>  | N=10<sup>9</sup>  |
| ------------ | ----------------- | ----------------- | ----------------- | ----------------- |
| Gauss        | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 4*10<sup>-6</sup> |
| Ciclo        | 3*10<sup>-4</sup> | 9*10<sup>-4</sup> | 0,1               | 92                |
| Gen. Lista   | 1*10<sup>-2</sup> | 1*10<sup>-1</sup> | 1                 | 1000              |
| Ric. Lineare | 5*10<sup>-5</sup> | 5*10<sup>-4</sup> | 5*10<sup>-2</sup> | 500               |

## Ricerca Dicotomica (o ricerca binaria)

{% highlight python %}
import random
import timeit

N = 10**3
lista = []

for cont in range(1,N):
    lista.append(random.randint(0, 1000000))
	
lista.sort()

def ricercaBinaria(arr, targetVal):
  left = 0
  right = len(arr) - 1

  while left <= right:
    mid = (left + right) // 2

    if arr[mid] == targetVal:
      return mid

    if arr[mid] < targetVal:
      left = mid + 1
    else:
      right = mid - 1

  return -1

start = timeit.default_timer()

x = 477
trovato = ricercaBinaria(lista, x)

stop = timeit.default_timer()
print('Time: ', stop - start)
{% endhighlight %}

| Algoritmo       | N=10<sup>3</sup>  | N=10<sup>4</sup>  | N=10<sup>6</sup>  | N=10<sup>9</sup>  |
| --------------- | ----------------- | ----------------- | ----------------- | ----------------- |
| Gauss           | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 4*10<sup>-6</sup> |
| Ciclo           | 3*10<sup>-4</sup> | 9*10<sup>-4</sup> | 0,1               | 92                |
| Gen. Lista      | 1*10<sup>-2</sup> | 1*10<sup>-1</sup> | 1                 | 1000              |
| Ric. Lineare    | 5*10<sup>-5</sup> | 5*10<sup>-4</sup> | 5*10<sup>-2</sup> | 500               |
| Ric. Dicotomica | 7,4*10<sup>-6</sup> | 6,8*10<sup>-6</sup> | 1,22*10<sup>-5</sup> | 5*10<sup>-5</sup> |


## Ordinamento: naive sort

L'algoritmo Naive Sort è un algoritmo di ordinamento di complessità O(N<sup>2</sup>).
Se guardiamo la procedura in basso ci rendiamo conto della presenza di due cicli annidati. 
Proviamo a verificare sperimentalmente la sua complessità. 

{% highlight python %}
import random
import timeit

N = 10**3
lista = []

for cont in range(1,N):
    lista.append(random.randint(0, 1000000))

def naiveSort(array):
    n = len(arr)
    for j in range(n):              # ciclo principale di visita della lista
        poskey = j
        key = vet[j]
        for i in range(j+1, N):     # questo ciclo visita la parte destra della lista in cerca di
            if vet[i] < key:        # elementi più piccoli di quello selezionato
                key = vet[i]
                poskey = i
        if poskey != j:             # se viene trovato un elemento più piccolo di key
            vet[poskey] = vet[j]    # i due vengono scambiati
            vet[j] = key

start = timeit.default_timer()

lista_ordinata = naiveSort(lista)

stop = timeit.default_timer()
print('Time: ', stop - start)
{% endhighlight %}

| Algoritmo       | N=10<sup>3</sup>  | N=10<sup>4</sup>  | N=10<sup>6</sup>  | N=10<sup>9</sup>  |
| --------------- | ----------------- | ----------------- | ----------------- | ----------------- |
| Gauss           | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 4*10<sup>-6</sup> |
| Ciclo           | 3*10<sup>-4</sup> | 9*10<sup>-4</sup> | 0,1               | 92                |
| Gen. Lista      | 1*10<sup>-2</sup> | 1*10<sup>-1</sup> | 1                 | 1000              |
| Ric. Lineare    | 5*10<sup>-5</sup> | 5*10<sup>-4</sup> | 5*10<sup>-2</sup> | 500               |
| Ric. Dicotomica | .... | .... | .... | ....               |
| Naive Sort      | .... | .... | .... | ....               |

## Ordinamento bubble sort

L'algoritmo Naive Sort è un algoritmo di ordinamento che ha complessità O(N<sup>2</sup>) nel caso peggiore in cui
la lista da ordinare sia ordinata nel senso opposto a quello desiderato.

Proviamo a verificare sperimentalmente la sua complessità. 

{% highlight python %}
import random
import timeit

N = 10**3
lista = []

for cont in range(1,N):
    lista.append(random.randint(0, 1000000))

def bubbleSort(arr):
    n = len(arr)
    # Si scandisce la lista elemento per elemento
    for i in range(n):
        # Gli ultimi i elementi sono gia' in ordine
        for j in range(0, n-i-1):
            # Se gli elementi da 0 ad i-1 non sono i ordine si invertono
            if arr[j] > arr[j+1] :
                arr[j], arr[j+1] = arr[j+1], arr[j]
 

start = timeit.default_timer()

lista_ordinata = bubbleSort(lista)

stop = timeit.default_timer()
print('Time: ', stop - start)
{% endhighlight %}

| Algoritmo       | N=10<sup>3</sup>  | N=10<sup>4</sup>  | N=10<sup>6</sup>  | N=10<sup>9</sup>  |
| --------------- | ----------------- | ----------------- | ----------------- | ----------------- |
| Gauss           | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 4*10<sup>-6</sup> |
| Ciclo           | 3*10<sup>-4</sup> | 9*10<sup>-4</sup> | 0,1               | 92                |
| Gen. Lista      | 1*10<sup>-2</sup> | 1*10<sup>-1</sup> | 1                 | 1000              |
| Ric. Lineare    | 5*10<sup>-5</sup> | 5*10<sup>-4</sup> | 5*10<sup>-2</sup> | 500               |
| Ric. Dicotomica | .... | .... | .... | ....               |
| Naive Sort      | .... | .... | .... | ....               |
| Bubble Sort     | .... | .... | .... | ....               |


## Ordinamento quick sort

L'algoritmo quick sort rientra tra gli algoritmi di ordinamento dalle performances migliori, questo risulta essere
O(log(N)). Il quick sort, a differenza dei precedenti algoritmi, si avvale però della ricorsione per risolvere
il problema dell'ordinamento e questo implica un maggiore uso di memoria. 

{% highlight python %}
import random
import timeit

N = 10**3
lista = []

for cont in range(1,N):
    lista.append(random.randint(0, 1000000))

def partition(array, low, high):
  """
  Questa funzione trova la posizone giusta per fare la partizione
  """
  # prendiamo come pivot l'elemento più a destra della lista
  pivot = array[high]

  # puntatore al primo elemento della lista
  i = low - 1

  # visita di tutti gli elementi confrontando con l'elemento pivot
  for j in range(low, high):
    if array[j] <= pivot:
      # se viene trovato un elemento più piccolo del pivot
      # si incrementa il secondo puntatore
      i = i + 1

      # si scambiano gli elementi
      (array[i], array[j]) = (array[j], array[i])

  # si scambia l'elemento pivot con quello puntato dal secondo puntatore
  (array[i + 1], array[high]) = (array[high], array[i + 1])

  # si restituisce la posizione dell'elemento pivot per procedere alla partizione successiva
  return i + 1

def quickSort(array, low, high):
  """
  funzione che implementa il quick sort
  """
  if low < high:

    # cerca un elemento in modo che
    # gli elementi più piccoli del pivot sono a sinistra
    # gli elementi più grandi del pivot sono a destra
    pi = partition(array, low, high)

    # ricorsione sulla sotto lista a sinistra del pivot
    quickSort(array, low, pi - 1)

    # ricorsione sulla sotto lista a destra del pivot
    quickSort(array, pi + 1, high)
 

start = timeit.default_timer()

size = len(lista)

quickSort(lista, 0, size - 1)

stop = timeit.default_timer()
print('Time: ', stop - start)
{% endhighlight %}

| Algoritmo       | N=10<sup>3</sup>  | N=10<sup>4</sup>  | N=10<sup>6</sup>  | N=10<sup>9</sup>  |
| --------------- | ----------------- | ----------------- | ----------------- | ----------------- |
| Gauss           | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 1*10<sup>-6</sup> | 4*10<sup>-6</sup> |
| Ciclo           | 3*10<sup>-4</sup> | 9*10<sup>-4</sup> | 0,1               | 92                |
| Gen. Lista      | 1*10<sup>-2</sup> | 1*10<sup>-1</sup> | 1                 | 1000              |
| Ric. Lineare    | 5*10<sup>-5</sup> | 5*10<sup>-4</sup> | 5*10<sup>-2</sup> | 500               |
| Ric. Dicotomica | .... | .... | .... | ....               |
| Naive Sort      | .... | .... | .... | ....               |
| Bubble Sort     | .... | .... | .... | ....               |
| Quick Sort      | .... | .... | .... | ....               |
