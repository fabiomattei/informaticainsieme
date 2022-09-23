---
title: 'Map, Filter e Reduce'
date: '2020-02-04T15:07:35+01:00'
author: Fabio Mattei
layout: page
---

## La funzione map

La funzione map() prende come parametri una funzione e una o più strutture dati di tipo iterable (list, tuple etc.) 
e restituisce un oggetto map (che è un iterator) contenente i risultati che si ottengono applicando la funzione passata a ciascuno degli oggetti contenuti nella struttura dati.

Facciamo un esempio:

{% highlight python %}
def raddoppia(n):
    """Questa funzione prende un numero come parametro, lo raddoppia e restituisce il risultato"""
    return n + n

numbers = (1, 2, 3, 4)
result = map(raddoppia, numbers)
print(list(result))
{% endhighlight %}

##### Output:
[2, 4, 6, 8]

Possiamo ottenere lo stesso effetto utilizzando le lambda functions.
La lambda function non è altro che una funzione molto breve, anonima, che sta su una sola riga, in pratica la funzione:

{% highlight python %}
def raddoppia(x):
    return x + x
{% endhighlight %}

Si trasforma in:

{% highlight python %}
lambda x: x + x
{% endhighlight %}

La lambda function si riconosce per la parola chiave *lambda* seguida dal o dai parametri seguito dal simbolo *:*.
Dopo di questo si scrive l'espressione per il calcolo del valore restituito. Non c'è bisogno della parola _return_.

A questo punto lo scrip map può diventare:

{% highlight python %}
numbers = (1, 2, 3, 4)
result = map(lambda x: x + x, numbers)
print(list(result))
{% endhighlight %}

##### Output:
[2, 4, 6, 8]


E' possibile utilizzare la funzione map su due liste contemporaneamente, si presume che le due liste contengano lo stesso numero di elementi.
In questo caso è necessario che la funzione che va ad operare riceva due parametri e che operi su questi.

{% highlight python %}
numbers1 = [1, 2, 3]
numbers2 = [4, 5, 6]
  
result = map(lambda x, y: x + y, numbers1, numbers2)
print(list(result))
{% endhighlight %}

##### Output:
[5, 7, 9]

Questo script somma il primo elemento della lista numbers1 con il primo della lista numbers2, il secondo elemento della lista numbers1 con il secondo della lista numbers2 e il terzo elemento della lista numbers1 con il terzo della lista numbers2.

In questo script la funzione *lambda x, y: x + y* equivale a

{% highlight python %}
def somma(x, y):
    return x + y
{% endhighlight %}

## La funzione filter

La funzione filter() prende come parametri una funzione, che restituisce un booleano, e una struttura dati di tipo iterable (list, tuple etc.) e restituisce un iterator contenente i valori della struttura dati per cui la funzione passata restitusce True.

{% highlight python %}
def isvocal(variable):
    """Questa funziona restituisce True quando riceve una vocale come parametro e falso altrimenti"""
    letters = ['a', 'e', 'i', 'o', 'u']
    if (variable in letters):
        return True
    else:
        return False
  
listaDiLettere = ['g', 'e', 'e', 'j', 'k', 's', 'p', 'r']
  
lettereFiltrate = filter(isvocal, listaDiLettere)

print('Le lettere filtrate sono:')
for s in lettereFiltrate:
    print(s)
{% endhighlight %}

##### Output:

The filtered letters are:
e
e


E' possibile anche in questo caso utilizzare una lambda function:

{% highlight python %}
"""Una lista contenete numeri pari e dispari. """
seq = [0, 1, 2, 3, 5, 8, 13]
  
"""result contains odd numbers of the list"""
numeriDispari = filter(lambda x: x % 2 != 0, seq)
print(list(numeriDispari))
  
"""result contains even numbers of the list"""
numeriPari = filter(lambda x: x % 2 == 0, seq)
print(list(numeriPari))
{% endhighlight %}

##### Output:
[1, 3, 5, 13]
[0, 2, 8]


## La funzione reduce

La funzione *reduce* riduce gli elementi di una lista ad uno solo. E' utile per sommare tutti gli elementi di una lista o per concatenare, è utile ogni qual volta pensiamo al concetto di *accumulatore* visto quando abbiamo studiato i cicli.

La funzione *reduce* prende come parametri una funzione, che riceve due parametri, e una struttura dati di tipo iterable (list, tuple etc.) e fa questi passi:

* Al primo passo prende i primi due elementi della struttura dati e li passa alla funzione calcolando il risultato
* Al secondo passo prende il risultato calcolato al primo passo e lo passa come primo parametro della funzione per la seconda iterazione, come secondo parametro passa il terzo elemento della struttura.
* Il processo continua fino all'esaurimento della struttura dati passata
* Il risultato finale viene restituito al chiamante della funzione *reduce*

La funzione *reduce* è definita nel modulo *functools* e va inclusa prima di poter essere usata.

Facciamo un esempio

{% highlight python %}
"""abbiamo bisogno di importare i functools per poter utilizzare reduce()"""
import functools
 
"""inizializziamo la lista"""
numeri = [1, 3, 5, 6, 2, ]
 
"""usiamo reduce per calcolare la somma degli elementi di una lista"""
print("Somma degli elementi nella lista: ", end="")
somma = functools.reduce(lambda a, b: a+b, numeri)
print(somma)
 
"""usiamo reduce per calcolare l'elemento più grande in una lista"""
print("Massimo elemento della lista: ", end="")
massimo = functools.reduce(lambda a, b: a if a > b else b, numeri)
print(massimo)
{% endhighlight %}


##### Output: 
Somma degli elementi nella lista: 17
Massimo elemento della lista: 6
 

#### Reduce con funzioni operator


{% highlight python %}
import functools

"""importo le funzioni operator"""
import operator

"""initializing list"""
lista = [1, 3, 5, 6, 2, ]

"""using reduce to compute sum of list"""
"""using operator functions"""
print("Somma degli elementi nella lista: ", end="")
print(functools.reduce(operator.add, lista))

"""using reduce to compute product"""
"""using operator functions"""
print("Massimo elemento della lista: ", end="")
print(functools.reduce(operator.mul, lista))

"""using reduce to concatenate string"""
print("Le stringhe concatenate sono: ", end="")
print(functools.reduce(operator.add, ["latte", "miele", "biscotti"]))
{% endhighlight %}

##### Output:
Somma degli elementi nella lista: 17
Massimo elemento della lista: 180
Le stringhe concatenate sono: lattemielebiscotti

