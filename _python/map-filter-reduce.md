---
title: 'Map, Filter e Reduce'
date: '2020-02-04T15:07:35+01:00'
author: Fabio Mattei
layout: page
---

### La funzione map

La funzione map() prende come parametri una funzione e una o più strutture dati di tipo iterable (list, tuple etc.) 
e restituisce un oggetto map (che è un iterator) contenente i risultati che si ottengono applicando la funzione passata a ciascuno degli oggetti contenuti nella struttura dati.

Facciamo un esempio:

{% highlight python %}
def addition(n):
    """Questa funzione prende un numero come parametro, lo raddoppia e restituisce il risultato"""
    return n + n

numbers = (1, 2, 3, 4)
result = map(addition, numbers)
print(list(result))
{% endhighlight %}

##### Output:
[2, 4, 6, 8]

Possiamo ottenere lo stesso effetto utilizzando le lambda functions:

{% highlight python %}
numbers = (1, 2, 3, 4)
result = map(lambda x: x + x, numbers)
print(list(result))
{% endhighlight %}

##### Output:
[2, 4, 6, 8]


E' possibile utilizzare la funzione map su due liste contemporaneamente, si presume che le due liste contengano lo stesso numero di elementi

{% highlight python %}
numbers1 = [1, 2, 3]
numbers2 = [4, 5, 6]
  
result = map(lambda x, y: x + y, numbers1, numbers2)
print(list(result))
{% endhighlight %}

##### Output:
[5, 7, 9]


### La funzione filter

La funzione filter() prende come parametri una funzione che restituisce un booleano e una struttura dati di tipo iterable (list, tuple etc.) e restituisce un iterator contenente i valori della struttura dati per cui la funzione passata restitusce True.

{% highlight python %}
def isvocal(variable):
    """Questa funziona restituisce True quando riceve una vocale come parametro e falso altrimenti"""
    letters = ['a', 'e', 'i', 'o', 'u']
    if (variable in letters):
        return True
    else:
        return False
  
sequence = ['g', 'e', 'e', 'j', 'k', 's', 'p', 'r']
  
filtered = filter(isvocal, sequence)

print('The filtered letters are:')
for s in filtered:
    print(s)
{% endhighlight %}

##### Output:

The filtered letters are:
e
e


E' possibile anche in questo caso utilizzare una lambda function:

{% highlight python %}
"""a list contains both even and odd numbers. """
seq = [0, 1, 2, 3, 5, 8, 13]
  
"""result contains odd numbers of the list"""
result = filter(lambda x: x % 2 != 0, seq)
print(list(result))
  
"""result contains even numbers of the list"""
result = filter(lambda x: x % 2 == 0, seq)
print(list(result))
{% endhighlight %}

##### Output:

[1, 3, 5, 13]
[0, 2, 8]


### La funzione reduce

The reduce(fun,seq) function is used to apply a particular function passed in its argument to all of the list elements mentioned in the sequence passed along. This function is defined in “functools” module.

Working :  

    At first step, first two elements of sequence are picked and the result is obtained.
    Next step is to apply the same function to the previously attained result and the number just succeeding the second element and the result is again stored.
    This process continues till no more elements are left in the container.
    The final returned result is returned and printed on console.

Facciamo un esempio

{% highlight python %}
"""abbiamo bisogno di importare i functools per poter utilizzare reduce()"""
import functools
 
"""inizializziamo la lista"""
numeri = [1, 3, 5, 6, 2, ]
 
"""usiamo reduce per calcolare la somma degli elementi di una lista"""
print("The sum of the list elements is : ", end="")
print(functools.reduce(lambda a, b: a+b, numeri))
 
"""usiamo reduce per calcolare l'elemento più grande in una lista"""
print("The maximum element of the list is : ", end="")
print(functools.reduce(lambda a, b: a if a > b else b, numeri))
{% endhighlight %}


##### Output: 
The sum of the list elements is : 17
The maximum element of the list is : 6
 
# python code to demonstrate working of reduce()
# using operator functions


{% highlight python %}
"""importing functools for reduce()"""
import functools

"""importing operator for operator functions"""
import operator

"""initializing list"""
lis = [1, 3, 5, 6, 2, ]

"""using reduce to compute sum of list"""
"""using operator functions"""
print("The sum of the list elements is : ", end="")
print(functools.reduce(operator.add, lis))

"""using reduce to compute product"""
"""using operator functions"""
print("The product of list elements is : ", end="")
print(functools.reduce(operator.mul, lis))

"""using reduce to concatenate string"""
print("The concatenated product is : ", end="")
print(functools.reduce(operator.add, ["geeks", "for", "geeks"]))
{% endhighlight %}

##### Output:
The sum of the list elements is : 17
The product of list elements is : 180
The concatenated product is : geeksforgeeks


{% highlight python %}
"""Python program to  illustrate sum of two numbers."""
def reduce(function, iterable, initializer=None):
    it = iter(iterable)
    if initializer is None:
        value = next(it)
    else:
        value = initializer
    for element in it:
        value = function(value, element)
    return value
 
"""Note that the initializer, when not None, is used as the first value instead of the first value from iterable , and after the whole iterable."""
tup = (2,1,0,2,2,0,0,2)
print(reduce(lambda x, y: x+y, tup,6))
{% endhighlight %}

##### Output:
15
