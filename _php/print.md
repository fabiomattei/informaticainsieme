---
title: Print
date: '2026-08-17T09:30:00+01:00'
author: Fabio Mattei
layout: page
---

In PHP la funzione (in realtà un **costrutto del linguaggio**) che stampa un messaggio sulla console è `echo`.

{% highlight php %}
<?php
echo "Hello World";
{% endhighlight %}

`echo` scrive un messaggio specifico sullo schermo. Il messaggio può essere una stringa di testo o un qualsiasi altro tipo di variabile che verrà automaticamente convertito in stringa dal linguaggio prima di essere scritto sullo schermo.

#### Sintassi

{% highlight php %}
echo oggetto1, oggetto2, ...;
{% endhighlight %}

A differenza della funzione `print()` di Python, `echo` in PHP:

- **non è una vera funzione** (è un costrutto del linguaggio, per questo non si usano le parentesi tonde, anche se `echo(...)` è comunque accettato)
- **non accetta i parametri `sep` e `end`**: se passiamo più valori separati da virgola, questi vengono semplicemente scritti uno dopo l'altro, **senza alcun separatore**
- **non va mai a capo automaticamente**: bisogna scrivere esplicitamente il carattere di nuova riga `\n` (dentro stringhe con apici doppi) o la costante `PHP_EOL`

#### Esempi

Digita e manda in esecuzione le seguenti istruzioni:

{% highlight php %}
<?php
echo "Ciao", " ", "Come stai?", "\n";
echo "Mela" . "---" . "Ciliegia" . "---" . "Pesca" . "\n";
echo "Trota", " ", "Salmone", " ", "Tonno";
echo "\n";
echo "Aquila" . "#" . "Falco" . "#" . "Cardellino" . "@";
{% endhighlight %}

Leggi cosa viene scritto nella console e mettilo in relazione con i comandi che hai digitato. Nota come, per ottenere l'effetto del parametro `sep` di Python, in PHP dobbiamo costruire noi stessi la stringa con l'operatore di concatenazione `.` oppure inserire manualmente i separatori tra gli argomenti di `echo`.

## print

Esiste anche l'istruzione `print`, molto simile ad `echo` ma con due differenze importanti: accetta **un solo argomento** (non si può scrivere `print "a", "b";`) ed è una vera espressione che restituisce sempre il valore `1`.

{% highlight php %}
<?php
print "Ciao mondo!\n";
{% endhighlight %}

Nella pratica quotidiana si usa quasi sempre `echo`, che è leggermente più veloce e più flessibile perché accetta più argomenti.

## Esercizi

#### Esercizio 1:
Scrivi un'istruzione `echo` che stampi le parole "Lunedì", "Martedì" e "Mercoledì" separate da una virgola e uno spazio.

#### Esercizio 2:
Scrivi un'istruzione `echo` che stampi i numeri 1, 2 e 3 separati dal carattere `-` e che termini con il simbolo `!` invece che con l'a capo.

#### Esercizio 3:
Scrivi due istruzioni `echo`, eseguite una di seguito all'altra, che scrivano sulla stessa riga della console le parole "Ciao" e "mondo" (suggerimento: in PHP `echo` non va mai a capo da solo, quindi basta non scrivere `\n`).

### Esercizi di tracciamento

Per i seguenti esercizi non dovete eseguire il codice: scrivete esattamente, carattere per carattere, cosa apparirà sulla console.

#### Esercizio 4:

Cosa scrive sulla console il seguente programma?

{% highlight php %}
<?php
echo "Mela", ", ", "Pera", ", ", "Banana", "\n";
echo "Fine";
{% endhighlight %}

#### Esercizio 5:

Cosa scrive sulla console il seguente programma? Fate attenzione a dove va a capo la scrittura.

{% highlight php %}
<?php
echo "Uno", " - ";
echo "Due", " - ";
echo "Tre";
{% endhighlight %}

#### Esercizio 6:

Cosa scrive sulla console il seguente programma?

{% highlight php %}
<?php
echo "A", "B", "C";
echo "\n";
echo "A", " ", "B", " ", "C";
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 7:

{% highlight php %}
<?php
echo "Ciao", "mondo"
{% endhighlight %}

#### Esercizio 8:

{% highlight php %}
<?php
print "Mela", "Pera";
{% endhighlight %}

#### Esercizio 9:

{% highlight php %}
<?php
echo "Ciao mondo"
{% endhighlight %}
