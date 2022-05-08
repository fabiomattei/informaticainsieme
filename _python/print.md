---
title: Print
date: '2020-02-07T22:32:45+01:00'
author: fabio
layout: page
---

La funzione print stampa un messaggio sulla console sullo schermo.

{% highlight python %}
print("Hello World")

{% endhighlight %}

La funzione print() stampa un messaggio specifico sullo schermo.

Il messaggio può essere una stringa di testo o un qualsiasi altro tipo di variabile che verrà automaticamente convertito in stringa dal linguaggio prima di essere scritto sullo schermo.

#### Sintassi

{% highlight python %}
print(oggetto(i), sep=separatore, end=fine)

{% endhighlight %}

| Parametro | Descrizione |
|---|---|
| *oggetto(i)* | Qualsiasi oggetto, tanti quanti ne vuoi. Sarà convertito in stringa prima di essere scritto sullo schermo. |
| sep=’*separator*e’ | Opzionale. Specifica come separare gli oggetti. Il valore di default è ‘ ‘ |
| end=’*fine*‘ | Opzionale. Specifica il simbolo da scrivere dopo aver scritto tutti gli oggetti. Il valore di default è ‘\\n’ (line feed) |

#### Esempi

Digita e manda in esecuzione le seguenti istruzioni:

{% highlight python %}
 print("Ciao", "Come stai?") 
 print("Mela", "Ciliegia", "Pesca", sep="---")
 print("Trota", "Salmone", "Tonno", end=" ")
 print("Aquila", "Falco", "Cardellino", sep="#", end="@")
 
{% endhighlight %}

Leggi cosa viene scritto nella console e mettilo in relazione con i comandi che hai digitato.
