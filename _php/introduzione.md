---
title: Introduzione
date: '2026-08-17T09:05:00+01:00'
author: Fabio Mattei
layout: page
---

PHP (acronimo ricorsivo di **PHP: Hypertext Preprocessor**) è un linguaggio di programmazione il cui sviluppo è iniziato nel 1994 da **Rasmus Lerdorf**. Nato come un insieme di script per tracciare le visite al proprio sito personale, è diventato negli anni il linguaggio server-side più diffuso al mondo: siti come Wikipedia, Facebook (nelle sue origini) e la maggior parte dei siti realizzati con WordPress sono scritti in PHP.

Per programmare useremo un semplice **editor di testo** (ad esempio Visual Studio Code) e l'**interprete PHP da riga di comando**, che abbiamo installato nella pagina precedente.

# Scrivi il tuo primo programma

Apriamo l'editor e creiamo un nuovo file chiamato `primoesempio.php` nella nostra cartella di lavoro personale.

Ogni file PHP che contiene solo codice PHP inizia con il tag di apertura `<?php`. Scriviamo all'interno del file le seguenti istruzioni:

{% highlight php %}
<?php
echo "Ciao mondo!\n";
echo "Questo è il mio primo programma\n";
echo "Da questo momento sono uno sviluppatore software\n";
{% endhighlight %}

Notate il simbolo `\n` alla fine di ogni riga: serve a mandare a capo la scrittura sulla console, esattamente come farebbe automaticamente la funzione `print` di Python.

# Fai eseguire il programma al computer

Apriamo il terminale (o il prompt dei comandi), ci posizioniamo nella cartella che contiene il file e digitiamo:

{% highlight shell %}
php primoesempio.php
{% endhighlight %}

Vedrai apparire all'interno della console:

{% highlight shell %}
Ciao mondo!
Questo è il mio primo programma
Da questo momento sono uno sviluppatore software
{% endhighlight %}

È tradizione che il primo programma quando si comincia a studiare un nuovo linguaggio di programmazione sia il "Ciao Mondo". Il programma avrà lo scopo di far apparire la scritta *Ciao Mondo* sullo schermo. Dal punto di vista logico è un compito semplice tuttavia il fatto che questo venga eseguito correttamente ci dà indicazione del fatto che l'ambiente di programmazione funziona in modo corretto e che il programmatore è riuscito ad operare con l'ambiente in maniera adeguata.

Da questo momento siete sviluppatori software!

## Esercizi

#### Esercizio 1:
Scrivi ed esegui un programma che stampi sullo schermo il tuo nome, la tua età e la scuola che frequenti, usando tre istruzioni `echo` separate.

#### Esercizio 2:
Scrivi ed esegui un programma che stampi un piccolo disegno fatto di caratteri, ad esempio un triangolo:
{% highlight php %}
<?php
echo "*\n";
echo "**\n";
echo "***\n";
{% endhighlight %}

#### Esercizio 3:
Scrivi ed esegui un programma che stampi la tabellina del 2, da 2x1 a 2x10, usando 10 istruzioni `echo` (una per riga).

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete scrivere esattamente cosa apparirà sulla console quando il programma verrà eseguito, riga per riga.

#### Esercizio 4:

Scrivi cosa viene stampato sulla console dal seguente programma:

{% highlight php %}
<?php
echo "Ciao mondo!\n";
echo "Questo è il mio primo programma\n";
echo "Da questo momento sono uno sviluppatore software\n";
{% endhighlight %}

#### Esercizio 5:

Scrivi, riga per riga, cosa viene stampato sulla console dal seguente programma:

{% highlight php %}
<?php
echo "Uno\n";
echo "Due\n";
echo "Tre\n";
echo "Quattro\n";
{% endhighlight %}

Cosa cambia se scambi tra loro la seconda e la terza riga di codice?

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 6:

{% highlight php %}
<?php
echo "Ciao mondo!;
{% endhighlight %}

#### Esercizio 7:

{% highlight php %}
<?php
echo "Ciao mondo!"
{% endhighlight %}

#### Esercizio 8:

{% highlight php %}
<?php
print "Ciao", "mondo!";
{% endhighlight %}
