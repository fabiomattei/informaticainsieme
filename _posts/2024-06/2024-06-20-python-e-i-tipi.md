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

Normalmente noi definiamo una funzione senza specificare il tipo dei parametri, come nell'esempio che segue:

{% highlight python %}
def somma(a, b):
  return a + b
{% endhighlight %}

Lo possiamo fare perché python è un linguaggio a tipizzazione dinamica e questo significa che il tipo dei parametri viene determinato durante l'esecuzione.
La funzione qui in alto riesce a lavorare bene sia che le si passino due numeri interi sia che le si passino due stringhe di testo.

Es:

{% highlight python %}
print(somma(2, 3))
# stampa il numero 5
print(somma("ciao", "marco"))
# stampa la stringha di testo "ciaomarco"
{% endhighlight %}

La nuova sintassi di python permette di specificare il tipo dei parametri:

{% highlight python %}
def somma(a:int, b:int) -> int:
  return a + b
{% endhighlight %}

La funzione è analoga alla precedente ma ora come si vede specifichiamo i tipi:

* a parametro di tipo integer
* b parametro di tipo integer
* parametro restituito di tipo integer

Quando il nostro codice è costituito da centinaia o migliaia di righe specificare il tipo dei parametri ci aiuta ad evitare un sacco di errori e ci permette di fare **refactoring** del codice in modo molto più semplice.

Possiamo fare di più, possiamo specificare che un parametro è una lista e specificare il tipo di dato all'interno della lista:

{% highlight python %}
def miafunzione(ls: list[int], x:float) -> list[float]:
  # corpo della funzione
{% endhighlight %}

* ls è una lista di numeri interi
* x è un numero float 
* la funzione dovrebbe restituire una lista di float

Facciamo un esempio con i dizionari:

{% highlight python %}
def miasecondafunzione(a:str, b:bool, c:dict[str,str]) -> dict[str,int]:
  # corpo della funzione
{% endhighlight %}

* a è una stringa
* b è un booleano
* c è un dizionario le cui chiavi sono stringhe di testo e i valori sono stringhe di testo
* il dato restituito è un dizionario le cui chiavi sono stringhe e i cui valori sono int



