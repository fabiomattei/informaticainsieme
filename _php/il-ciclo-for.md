---
title: 'Il ciclo for'
date: '2026-08-17T09:55:00+01:00'
author: Fabio Mattei
layout: page
---

![Diagramma di flusso del ciclo for con inizializzazione, condizione e incremento](/images/php/il-ciclo-for/il-ciclo-for.svg){:class="aside-image"}

A differenza di Python, dove esiste un solo tipo di ciclo `for` (che scandisce sempre gli elementi di una sequenza), PHP mette a disposizione **due costrutti distinti**:

- `for`: un ciclo **C-style**, in cui inizializzazione, condizione e incremento sono scritti esplicitamente sulla stessa riga;
- `foreach`: un ciclo pensato apposta per scandire gli elementi di un array, molto simile al `for ... in` di Python.

## Il ciclo for (C-style)

{% highlight php %}
for (<inizializzazione>; <condizione>; <incremento>) {
    // corpo del ciclo
}
{% endhighlight %}

{% highlight php %}
<?php
// Esempio 1: stampa i numeri da 0 a 4
for ($i = 0; $i < 5; $i++) {
    echo $i, "\n";
}
{% endhighlight %}

`$i++` è una scorciatoia per `$i = $i + 1;`. Esiste anche `$i--` per decrementare.

## Il ciclo foreach

Il ciclo `foreach` ci permette di iterare su tutti gli elementi di un array ed eseguire un determinato blocco di codice, in modo molto simile al `for` di Python.

{% highlight php %}
foreach ($array as $variabile) {
    // elabora su $variabile
}
{% endhighlight %}

{% highlight php %}
<?php
// Esempio 2:
foreach ([5, 10, 15, 20, 25] as $a) {
    echo $a, "\n";
}
{% endhighlight %}

{% highlight php %}
<?php
// Esempio 3:
$seq = [1, 2, 3, 4, 5];
foreach ($seq as $n) {
    echo "Il numero ", $n, " e' ";
    if ($n % 2 == 0) {
        echo "pari\n";
    } else {
        echo "dispari\n";
    }
}
{% endhighlight %}

## range

Anche PHP fornisce una funzione chiamata **range** che permette di generare una sequenza di numeri, ma con una differenza importante rispetto a Python: `range($inizio, $fine, $passo)` **include sempre l'estremo finale**, mentre in Python `range(inizio, fine, passo)` lo esclude.

{% highlight php %}
<?php
// Esempio di uso di range con inizio, fine e passo
foreach (range(1, 6, 1) as $x) {
    echo $x, "\n";
}
// scrive la sequenza di numeri: 1, 2, 3, 4, 5, 6
// notare che, a differenza di Python, il valore 6 viene incluso!
{% endhighlight %}

Posso utilizzare range con due soli argomenti (il passo viene implicitamente posto a 1):

{% highlight php %}
<?php
foreach (range(1, 4) as $x) {
    echo "Quadrato di ", $x, ": ", $x ** 2, "\n";
}
// scrive:
// Quadrato di 1: 1
// Quadrato di 2: 4
// Quadrato di 3: 9
// Quadrato di 4: 16
{% endhighlight %}

Per riprodurre esattamente il comportamento di `range()` in Python (estremo finale escluso), possiamo usare il ciclo `for` C-style, che è comunque il modo più comune di scrivere un ciclo numerico in PHP:

{% highlight php %}
<?php
// equivalente a range(7) in Python: da 0 a 6
for ($x = 0; $x < 7; $x++) {
    echo $x, "\n";
}
{% endhighlight %}

## Variabili accumulatori e ciclo for

Utilizzare il ciclo for con una variabile accumulatore è molto semplice. Nel seguente esempio si vede come si usa l'accumulatore per sommare 6 numeri.

{% highlight php %}
<?php
// Esempio di uso di accumulatore per la somma
$acc = 0;
for ($x = 1; $x <= 6; $x++) {
    $acc = $acc + $x;
}
echo $acc; // nota che questa istruzione e' fuori dal ciclo
{% endhighlight %}

Nel seguente esempio si vede come si usa l'accumulatore per moltiplicare 6 numeri.

{% highlight php %}
<?php
// Esempio di uso di accumulatore per la moltiplicazione
$acc = 1;
for ($x = 1; $x <= 6; $x++) {
    $acc = $acc * $x;
}
echo $acc; // nota che questa istruzione e' fuori dal ciclo
{% endhighlight %}

## Istruzioni di lettura ripetute

Lo scopo fondamentale per cui esistono i cicli è quello di ripetere un certo numero di operazioni. Se devo per esempio leggere 10 numeri e sommarli tra loro, potrei fare così:

{% highlight php %}
<?php
$acc = 0;
echo "Numero: "; $acc = $acc + (int) trim(fgets(STDIN));
echo "Numero: "; $acc = $acc + (int) trim(fgets(STDIN));
echo "Numero: "; $acc = $acc + (int) trim(fgets(STDIN));
echo "Numero: "; $acc = $acc + (int) trim(fgets(STDIN));
echo "Numero: "; $acc = $acc + (int) trim(fgets(STDIN));
echo "Numero: "; $acc = $acc + (int) trim(fgets(STDIN));
echo "Numero: "; $acc = $acc + (int) trim(fgets(STDIN));
echo "Numero: "; $acc = $acc + (int) trim(fgets(STDIN));
echo "Numero: "; $acc = $acc + (int) trim(fgets(STDIN));
echo "Numero: "; $acc = $acc + (int) trim(fgets(STDIN));

echo $acc;
{% endhighlight %}

Oppure potrei fare così:

{% highlight php %}
<?php
$acc = 0;
for ($x = 0; $x < 10; $x++) {
    echo "Numero: ";
    $acc = $acc + (int) trim(fgets(STDIN));
}
echo $acc;
{% endhighlight %}

Notate come il ciclo for mi ha permesso di ridurre molto le istruzioni che ho scritto. Cosa sarebbe successo se avessi dovuto leggere 100 numeri?

## Il campione

Può capitare di dover leggere una sequenza di numeri e trovare il più grande. In questo caso utilizzo una variabile **campione** che ciclo dopo ciclo conterrà il valore migliore trovato. Potrò utilizzare una condizione per confrontare ad ogni iterazione il valore letto con il campione ottenuto fino a quel momento.

{% highlight php %}
<?php
$massimo = 0;
for ($x = 0; $x < 10; $x++) {
    echo "Numero: ";
    $num = (int) trim(fgets(STDIN));
    if ($num > $massimo) {
        $massimo = $num;
    }
}
echo $massimo;
{% endhighlight %}

## Esercizi

#### Esercizio 1:
 Scrivere un programma che scriva tutti i numeri da 1 a 100

#### Esercizio 2:
 Scrivere un programma che scriva tutti i numeri pari da 1000 a 5000.

#### Esercizio 3:
 Scrivere un ciclo che sommi tutti i numeri dispari minori di 100. Scrivere la somma ottenuta.

#### Esercizio 4:
 Scrivere un ciclo che letto un numero N scriva i dieci numeri pari successivi ad N

#### Esercizio 5:
 Scrivere un ciclo che letti 10 numeri ne scriva il massimo.

#### Esercizio 6:
 Scrivere un ciclo che letti 10 numeri scriva la somma dei numeri il cui valore è compreso fra 10 e 20.

#### Esercizio 7:
 Scrivere un ciclo che letti due numeri N e M con N < M, scriva tutti i numeri compresi tra N e M.

#### Esercizio 8:
 Scrivere un ciclo che letti due numeri N e M con N < M, sommi tutti i numeri compresi tra N e M. Scrivere la somma ottenuta.

#### Esercizio 9:
 Scrivere un ciclo che letto un numero N scriva tutti i suoi divisori

#### Esercizio 10:
 Scrivere un ciclo che letto un numero N ne calcoli la radice quadrata intera (ovvero il massimo intero x tale che x²≤N)

#### Esercizio 11:
 Scrivere un ciclo che letto un numero N conti i suoi divisori e ne scriva il numero

#### Esercizio 12:
 Scrivere un ciclo che calcoli il fattoriale di un numero intero fornito dall'utente.

#### Esercizio 13:
 Scrivere un ciclo che letto un numero N permetta poi all'utente di inserire N numeri e provveda a calcolare la loro somma e la loro media e le scriva.

#### Esercizio 14:
 Scrivere un programma che legga due numeri, quindi chiede di inserire la somma. Fino a quando l'utente non inserisce la somma corretta, il programma scrive la frase "Errato: riprova"

#### Esercizio 15:
 Scrivere un programma che letto un numero positivo N determini il massimo intero K tale che la somma dei primi K interi sia minore o uguale a N.

Ad esempio, se N=20 allora K risulta 5, infatti

1 + 2 + 3 + 4 + 5 = 15 mentre

1 + 2 + 3 + 4 + 5 + 6 = 21

#### Esercizio 16:
un programma che letti N numeri in ingresso calcoli il numero massimo e il
numero minimo inseriti

#### Esercizio 17:
un programma che letti N numeri in ingresso calcoli la somma di tutti i numeri
mostrando le somme parziali ogni 3 numeri inseriti

#### Esercizio 18:
 Scrivere un ciclo che continui a **leggere** valori interi digitati dall'utente e a sommarli fino all'immissione del quinto numero pari. Scrivere la somma ottenuta.

#### Esercizio 19:
 Trovare il minor numero di banconote da 100€, 50€, 10€, 5€, necessarie per pagare una assegnata cifra C multipla di 5.

#### Esercizio 20:
scrivi un programma PHP che utilizzando due cicli for scriva la tabellina della addizione

#### Esercizio 21:
scrivi un programma PHP che utilizzando due cicli for scriva la tabellina della moltiplicazione

#### Esercizio 22:
scrivi un programma PHP che trovi i numeri primi compresi tra 1 e 100000.

#### Esercizio 23:
Scrivere un ciclo che continui a leggere valori interi digitati dall'utente e a sommarli fino all'immissione del quinto numero pari. Scrivere la somma ottenuta.

#### Esercizio 24:
Scrivere un programma che letto un numero intero N calcoli la somma di tutte le cifre
dispari che lo compongono

#### Esercizio 25:
Scrivere un programma che letti due numeri interi calcoli il massimo comune divisore e scriva il risultato

#### Esercizio 26:
Scrivi un algoritmo che letto un numero scriva "numero primo" se questo è un numero primo, "numero non primo" in caso opposto.

#### Esercizio 27:
Scrivi un programma PHP che generi tre numeri interi compresi tra 100 e 999 che siano divisibili per 5.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, cioè una tabella che mostra riga per riga come cambia il valore di ogni variabile ad ogni iterazione del ciclo `for`.

#### Esercizio 28:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$x` e `$acc` ad ogni iterazione):

{% highlight php %}
<?php
$acc = 0;
for ($x = 1; $x < 6; $x++) {
    $acc = $acc + $x;
}
echo $acc;
{% endhighlight %}

#### Esercizio 29:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$x` e `$acc` ad ogni iterazione). Attenzione al valore di partenza e al passo:

{% highlight php %}
<?php
$acc = 1;
for ($x = 10; $x > 0; $x -= 2) {
    $acc = $acc * 1;
    $acc = $acc + $x;
}
echo $acc;
{% endhighlight %}

#### Esercizio 30:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$x`, `$cont` e `$somma` ad ogni iterazione):

{% highlight php %}
<?php
$somma = 0;
$cont = 0;
for ($x = 1; $x < 11; $x++) {
    if ($x % 3 == 0) {
        $somma = $somma + $x;
        $cont = $cont + 1;
    }
}
echo $cont, " ", $somma;
{% endhighlight %}

#### Esercizio 31:

Costruite la tabella di tracciamento del seguente programma. Fate attenzione al valore di `$massimo` prima ancora che il ciclo cominci:

{% highlight php %}
<?php
$valori = [3, 7, 2, 9, 4];
$massimo = 0;
foreach ($valori as $x) {
    if ($x > $massimo) {
        $massimo = $x;
    }
}
echo $massimo;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 32:

{% highlight php %}
<?php
for ($x = 1; $x < 10; x++) {
    echo $x;
}
{% endhighlight %}

#### Esercizio 33:

{% highlight php %}
<?php
for ($x = 1, $x < 10, $x++) {
    echo $x;
}
{% endhighlight %}

#### Esercizio 34:

{% highlight php %}
<?php
$acc = 0;
for ($x = 1; $x < 11; $x++) {
    $acc = $acc + $x;
    print "Il valore parziale e'", $acc;
}
{% endhighlight %}

#### Esercizio 35:

{% highlight php %}
<?php
for ($x = 1; $x < 10; $x++) {
    echo $x
}
{% endhighlight %}

#### Esercizio 36:

{% highlight php %}
<?php
for ($x = 1; $x < 6; $x++) {
    if ($x % 2 == 0)
        echo $x, " e' pari"
}
{% endhighlight %}

#### Esercizio 37:

Questo programma non contiene errori di sintassi (viene eseguito senza generare errori), ma contiene un **errore logico**: individuatelo e spiegate perché il programma non stampa il risultato che ci si aspetterebbe.

{% highlight php %}
<?php
$acc = 0;
for ($x = 1; $x < 11; $x++) {
    $acc = $x + $x;
}
echo $acc;
{% endhighlight %}
