---
title: Quick Sort
date: '2022-05-20T05:56:13+02:00'
author: Fabio Mattei
layout: page
---

Quick Sort è un algoritmo basato sull'approccio dividi e conquista in cui:

1. una lista è suddivisa in sottoliste seleezionando un elemento pivot (elemento selzionato dalla lista). Durante le successive partizioni l'elemento pivot sarà posizionato in modo che gli elementi più piccoli si trovino alla sua sinistra mentre gli elementi più grandi alla sua destra.
2. Le sottoliste di sinistra e destra sono divise a loro volta usando lo stesso approccio, fino ad arrivare a liste composte da un solo elemento. A questo punto gli elementi sono in ordine. 
3. In fine le sottoliste sono ricombinate per formare la lista ordinata.

## Vediamo come funziona

![Si seleziona l'elemento pivot](/images/algoritmi/quicksort/quicksort01.png){:class="aside-image"}

### 1. Si seleziona l'elemento pivot

Ci sono molte varianti del quick sort in cui l'elemento pivot è selezionato da posizioni diverse. Qui utilizzeremo come pivot l'elemento più a destra della lista o sotto-lista.

{::options parse_block_html="true" /}
<br style="clear:both" />

![Si seleziona l'elemento pivot](/images/algoritmi/quicksort/quicksort02.png){:class="aside-image"}

### 2. Arrangiare la lista

Ora tutti gli elementi della lista sono riarrangati in modo che gli elementi più piccoli del pivot siano sulla sinistra e quelli più grandi sulla destra.

<br style="clear:both" />

Ecco come si riarrangia la lista:

![Si seleziona l'elemento pivot](/images/algoritmi/quicksort/quicksort03.png){:class="aside-image"}

#### Un puntatore viene direzionato verso l'elemento pivot, questo viene confrontato con gli elementi a partire dal prmo indice della partizione presa in esame.

<br style="clear:both" />

![Puntatore direzionato verso pivot](/images/algoritmi/quicksort/quicksort04.png){:class="aside-image"}

#### Si confronta il pivot con il primo elemento della partizione, se questo è più grande si direziona a questo un secondo puntatore.

<br style="clear:both" />

![Secondo puntatore](/images/algoritmi/quicksort/quicksort05.png){:class="aside-image"}

#### Se l'elemento è più grande del pivot gli si assegna un secondo puntatore.

<br style="clear:both" />

![Si seleziona l'elemento pivot](/images/algoritmi/quicksort/quicksort06.png){:class="aside-image"}

#### II pivot viene confrontato con gli elementi che seguono.

<br style="clear:both" />

![II pivot viene confrontato con gli elementi che seguono](/images/algoritmi/quicksort/quicksort07.png){:class="aside-image"}

#### Se l'elemento è più piccolo del pivot, questo viene scambiato con l'elemento puntato dal secondo puntatore.

<br style="clear:both" />

![Si seleziona l'elemento pivot](/images/algoritmi/quicksort/quicksort08.png){:class="aside-image"}

#### Il secondo puntatore viene spostato in avanti di una posizione e il processo si ripete. Si punta il secondo puntatore ad un elemento più grande del pivot e lo si scambia quando si incontra un elemento più piccolo del pivot.

<br style="clear:both" />

![Si seleziona l'elemento pivot](/images/algoritmi/quicksort/quicksort09.png){:class="aside-image"}

#### Lo 0 è più piccolo del pivot.

<br style="clear:both" />

![Si seleziona l'elemento pivot](/images/algoritmi/quicksort/quicksort10.png){:class="aside-image"}

#### Quindi lo 0 deve essere scambiato con l'elemento puntato dal secondo puntatore.

<br style="clear:both" />

![Si seleziona l'elemento pivot](/images/algoritmi/quicksort/quicksort11.png){:class="aside-image"}

#### Il secondo puntatore viene di nuovo spostato in avanti

<br style="clear:both" />

![Si seleziona l'elemento pivot](/images/algoritmi/quicksort/quicksort12.png){:class="aside-image"}

#### Si continua fino ad arrivare all'ellemento che precede il pivot.

<br style="clear:both" />

![Si seleziona l'elemento pivot](/images/algoritmi/quicksort/quicksort13.png){:class="aside-image"}

#### Alla fine l'elemento pivot viene scambiato con l'elemento indicato dal secondo puntatore.

<br style="clear:both" />

![Divisione in sottoliste](/images/algoritmi/quicksort/quicksort14.png){:class="aside-image"} 

### 3. Divisione in sottoliste

A questo punto siamo sicuri che l'elemento pivot è al posto giusto, tutti gli elementi più piccoli sono alla sua sinstra
e quelli più grandi alla sua destra. Quindi chiamiamo ricorsivamente il quick sort per le due sotto liste.
Si continua in questo modo fino ad arrivare a liste formate da un solo elemento che sono per loro natura ordinate.

## Implementazione in python


{% highlight python %}
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


data = [8, 7, 2, 1, 0, 9, 6]
print("Lista non ordinata")
print(data)

size = len(data)

quickSort(data, 0, size - 1)

print('Lista ordnata:')
print(data)
{% endhighlight %}
