---
title: Introduzione
date: '2020-01-22T09:18:35+01:00'
author: Fabio Mattei
layout: page
---

Python è un linguaggio di programmazione di alto livello il cui sviluppo è iniziato nel Dicembre 1989 da **Guido van Rossum** al CWI (Centrum Wiskunde &amp; Informatica – National Research Institute for Mathematics and Computer Science) in Olanda.


{::options parse_block_html="true" /}
<div>

![Apertura editor](/images/python/introduzione/console.jpg){:class="aside-image"}

Per programmare useremo **IDLE** (Ambiente di sviluppo integrato di Python).
	
# Mandare in esecuzione il programma IDLE. 

Si aprirà una shell

Cliccare su file → new e creare un nuovo file. Si aprirà l’editor. Salvare il file con nome primoesempio.py (save as) nella cartella di lavoro personale.

## Scrivi il tuo primo programma

Scrivi all’interno dell’editor le seguenti istruzioni:

{% highlight python %}
print("Ciao mondo!")
print("Questo è il mio primo programma")
print("Da questo momento sono uno sviluppatore software")
{% endhighlight %}
</div>


<div>
##  Fai eseguire il programma al computer 

Nel menù dell'editor dovresti trovare la voce **Run**, clicca su questa voce e poi su **Run module**

Run -&gt; Run module

Vedrai apparire all'interno della console:

![Il mio primo programma](/images/python/introduzione/console2.png){:class="aside-image"}

{% highlight python %}
Ciao mondo!
Questo è il mio primo programma
Da questo momento sono uno sviluppatore software
{% endhighlight %}

<div>


E’ tradizione che il primo programma quando si comincia a studiare un nuovo linguaggio di programmazione sia il “Ciao Mondo”. Il programma avrà lo scopo di far apparire la scritta *Ciao Mondo* sullo schermo. Dal punto di vista logico è un compito semplice tuttavia il fatto che questo venga eseguito correttamente ci dà indicazione del fatto che l’ambiente di programmazione funziona in modo corretto e che il programmatore è riuscito ad operare con l’ambiente in maniera adeguata.

Da questo momento siete sviluppatori software!

## Esercizi

#### Esercizio 1:
Scrivi ed esegui un programma che stampi sullo schermo il tuo nome, la tua età e la scuola che frequenti, usando tre istruzioni print separate.

#### Esercizio 2:
Scrivi ed esegui un programma che stampi un piccolo disegno fatto di caratteri, ad esempio un triangolo:
{% highlight python %}
print("*")
print("**")
print("***")
{% endhighlight %}

#### Esercizio 3:
Scrivi ed esegui un programma che stampi la tabellina del 2, da 2x1 a 2x10, usando 10 istruzioni print (una per riga).

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete scrivere esattamente cosa apparirà sulla console quando il programma verrà eseguito, riga per riga.

#### Esercizio 4:

Scrivi cosa viene stampato sulla console dal seguente programma:

{% highlight python %}
print("Ciao mondo!")
print("Questo è il mio primo programma")
print("Da questo momento sono uno sviluppatore software")
{% endhighlight %}

#### Esercizio 5:

Scrivi, riga per riga, cosa viene stampato sulla console dal seguente programma:

{% highlight python %}
print("Uno")
print("Due")
print("Tre")
print("Quattro")
{% endhighlight %}

Cosa cambia se scambi tra loro la seconda e la terza riga di codice?

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Python ed infine correggetelo.

#### Esercizio 6:

{% highlight python %}
print("Ciao mondo!)
{% endhighlight %}

#### Esercizio 7:

{% highlight python %}
print("Ciao mondo!"
{% endhighlight %}

#### Esercizio 8:

{% highlight python %}
Print("Ciao mondo!")
{% endhighlight %}