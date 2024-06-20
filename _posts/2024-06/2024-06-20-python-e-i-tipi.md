---
layout: post
title:  "Python e i tipi"
date:   2024-06-20 00:00:00
categories: python
excerpt: 
author: Fabio
tags: python funzioni
---

Python è sia un linguaggio fortemente tipato sia un linguaggio a tipizzazione dinamica. Cosa significa?

* fortemente tipato: ad ogni variabile viene assegnato un tipo di dato (intero, booleano, stringa di testo) e questo tipo è predefinito e parte del linguaggio
* l'interprete assegna i tipi durante l'esecuzione del programma basandosi sui valori che le variabili assumono

{% highlight python %}
def magic(a, b):
  return a + b
{% endhighlight %}

^ we can write functions like that because Python is dynamically typed — meaning that variable data types are determined at runtime.

{% highlight python %}
def magic(a:int, b:int) -> int:
  return a + b
{% endhighlight %}

^ writing the exact same function as above, but with type hints

    a should be an integer
    b should be an integer
    the function’s return value should also be an integer

When our code base gets huge, type hinting becomes increasingly important in making our code as human-readable as possible — imagine having 10,000 functions but you need to infer the data types they take in, and their return types. Not too fun

{% highlight python %}
def test1(ls: list[int], x:float) -> list[float]:
  # stuff
{% endhighlight %}

^ ls should be a list of floats, x should be a float, and the function should return a list of floats

{% highlight python %}
def test2(a:str, b:bool, c:dict[str,str]) -> dict[str,int]:
  # stuff
{% endhighlight %}

^ a should be a string, b should be a boolean value, and c should be a dictionary where keys are strings, and values are strings. The return value should be a dict where keys are strings, but values are integers.

Note — type hints hint, not enforce. If we do not follow a type hint’s type, Python will still allow it.


