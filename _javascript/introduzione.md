---
title: Introduzione
date: '2026-08-18T09:05:00+01:00'
author: Fabio Mattei
layout: page
---

Per programmare useremo un semplice **editor di testo** (ad esempio Visual Studio Code) e **Node.js da riga di comando**, che abbiamo installato nella pagina precedente.

# Scrivi il tuo primo programma

Apriamo l'editor e creiamo un nuovo file chiamato `primoesempio.js` nella nostra cartella di lavoro personale.

A differenza di PHP, un file JavaScript **non ha bisogno di alcun tag di apertura**: ogni riga del file è già codice JavaScript. Scriviamo all'interno del file le seguenti istruzioni:

{% highlight javascript %}
console.log("Ciao mondo!");
console.log("Questo è il mio primo programma");
console.log("Da questo momento sono uno sviluppatore software");
{% endhighlight %}

`console.log()` è la funzione che stampa un valore sulla console: è l'equivalente JavaScript di `echo` in PHP e di `print` in Python. A differenza di entrambi, `console.log()` **va a capo automaticamente** dopo ogni chiamata: non serve scrivere `\n` alla fine della stringa.

Nota anche il **punto e virgola** `;` alla fine di ogni istruzione: in JavaScript è **facoltativo** (l'interprete lo aggiunge automaticamente dove serve, un meccanismo chiamato *Automatic Semicolon Insertion*), ma è buona pratica scriverlo sempre esplicitamente, per evitare i rari casi in cui questo meccanismo automatico non fa quello che ci aspettiamo.

# Fai eseguire il programma al computer

Apriamo il terminale (o il prompt dei comandi), ci posizioniamo nella cartella che contiene il file e digitiamo:

{% highlight shell %}
node primoesempio.js
{% endhighlight %}

Vedrai apparire all'interno della console:

{% highlight shell %}
Ciao mondo!
Questo è il mio primo programma
Da questo momento sono uno sviluppatore software
{% endhighlight %}

È tradizione che il primo programma quando si comincia a studiare un nuovo linguaggio di programmazione sia il "Ciao Mondo". Il programma avrà lo scopo di far apparire la scritta *Ciao Mondo* sullo schermo. Dal punto di vista logico è un compito semplice tuttavia il fatto che questo venga eseguito correttamente ci dà indicazione del fatto che l'ambiente di programmazione funziona in modo corretto e che il programmatore è riuscito ad operare con l'ambiente in maniera adeguata.

Da questo momento siete sviluppatori software!

## I commenti

Come negli altri linguaggi visti, possiamo aggiungere commenti al codice: righe di testo ignorate dall'interprete, che servono solo a noi umani per capire il codice.

{% highlight javascript %}
// Questo è un commento su una sola riga

/*
   Questo è un commento
   che si estende su più righe
*/
console.log("Ciao"); // commento alla fine di una riga
{% endhighlight %}

## Esercizi

#### Esercizio 1:
Scrivi ed esegui un programma che stampi sullo schermo il tuo nome, la tua età e la scuola che frequenti, usando tre istruzioni `console.log` separate.

#### Esercizio 2:
Scrivi ed esegui un programma che stampi un piccolo disegno fatto di caratteri, ad esempio un triangolo:
{% highlight javascript %}
console.log("*");
console.log("**");
console.log("***");
{% endhighlight %}

#### Esercizio 3:
Scrivi ed esegui un programma che stampi la tabellina del 2, da 2x1 a 2x10, usando 10 istruzioni `console.log` (una per riga).

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete scrivere esattamente cosa apparirà sulla console quando il programma verrà eseguito, riga per riga.

#### Esercizio 4:

Scrivi cosa viene stampato sulla console dal seguente programma:

{% highlight javascript %}
console.log("Ciao mondo!");
console.log("Questo è il mio primo programma");
console.log("Da questo momento sono uno sviluppatore software");
{% endhighlight %}

#### Esercizio 5:

Scrivi, riga per riga, cosa viene stampato sulla console dal seguente programma:

{% highlight javascript %}
console.log("Uno");
console.log("Due");
console.log("Tre");
console.log("Quattro");
{% endhighlight %}

Cosa cambia se scambi tra loro la seconda e la terza riga di codice?

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 6:

{% highlight javascript %}
console.log("Ciao mondo!;
{% endhighlight %}

#### Esercizio 7:

{% highlight javascript %}
console.log("Ciao mondo!"
{% endhighlight %}

#### Esercizio 8:

{% highlight javascript %}
consol.log("Ciao mondo!");
{% endhighlight %}
