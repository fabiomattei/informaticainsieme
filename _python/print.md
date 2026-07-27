---
title: Print
date: '2020-02-07T22:32:45+01:00'
author: Fabio Mattei
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

## Esercizi

#### Esercizio 1:
Scrivi un’istruzione print che stampi le parole "Lunedì", "Martedì" e "Mercoledì" separate da una virgola e uno spazio.

#### Esercizio 2:
Scrivi un’istruzione print che stampi i numeri 1, 2 e 3 separati dal carattere `-` e che termini con il simbolo `!` invece che con l’a capo.

#### Esercizio 3:
Scrivi due istruzioni print che, eseguite una di seguito all’altra, scrivano sulla stessa riga della console le parole "Ciao" e "mondo" (suggerimento: usa il parametro `end`).

### Esercizi di tracciamento

Per i seguenti esercizi non dovete eseguire il codice: scrivete esattamente, carattere per carattere, cosa apparirà sulla console.

#### Esercizio 4:

Cosa scrive sulla console il seguente programma?

{% highlight python %}
print("Mela", "Pera", "Banana", sep=", ")
print("Fine")
{% endhighlight %}

#### Esercizio 5:

Cosa scrive sulla console il seguente programma? Fate attenzione a dove va a capo la scrittura.

{% highlight python %}
print("Uno", end=" - ")
print("Due", end=" - ")
print("Tre")
{% endhighlight %}

#### Esercizio 6:

Cosa scrive sulla console il seguente programma?

{% highlight python %}
print("A", "B", "C", sep="")
print("A", "B", "C")
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Python ed infine correggetelo.

#### Esercizio 7:

{% highlight python %}
print("Ciao", "mondo"
{% endhighlight %}

#### Esercizio 8:

{% highlight python %}
print("Mela", "Pera" sep=", ")
{% endhighlight %}

#### Esercizio 9:

{% highlight python %}
print "Ciao mondo"
{% endhighlight %}
