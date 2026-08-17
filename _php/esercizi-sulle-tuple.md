---
title: 'Le tuple (e la destrutturazione degli array)'
date: '2026-08-17T10:05:00+01:00'
author: Fabio Mattei
layout: page
---

A differenza di Python, **PHP non ha un tipo tupla nativo**. In PHP esiste un solo tipo composto, l'array, che può contenere elementi eterogenei ed è sempre mutabile: non esiste una struttura dati "gemella dell'array ma immutabile" come la tupla di Python.

Quello che in Python si ottiene con una tupla, in PHP si ottiene quasi sempre con un normale array (che useremo con la convenzione di *non modificarlo* dopo la creazione), oppure, quando serve davvero l'immutabilità, con le **costanti** (`define()` o `const`). Ciò che invece PHP condivide pienamente con Python è la possibilità di **destrutturare** un array, cioè spacchettarne il contenuto direttamente in più variabili in un colpo solo, con la sintassi `list()` (o la forma abbreviata con le parentesi quadre).

{% highlight php %}
<?php
// un array "usato come tupla": tre valori legati insieme
$numeri = [0, 1, 2, 3];
$lettere = ['a', 'b', 'c', 'd', 'e'];
$parole = ['mattina', 'pomeriggio'];
{% endhighlight %}

Tutti gli operatori e le funzioni che abbiamo visto per gli array (min, max, count, in_array) sono utilizzabili anche qui. Ovviamente, dato che PHP non impedisce la modifica, sta a noi programmatori **non modificare** l'array quando lo usiamo con il significato di tupla.

Cosa fa secondo te il seguente algoritmo?

{% highlight php %}
<?php
$vowels = ['a', 'e', 'i', 'o', 'u'];
$letters = ['a', 'b', 'c', 'd', 'e'];
foreach ($letters as $x) {
    if (in_array($x, $vowels)) {
        echo $x . " is a vowel\n";
    } else {
        echo $x . " is a consonant\n";
    }
}
{% endhighlight %}

Il seguente frammento di codice stampa il quadrato di ogni numero di seq.

{% highlight php %}
<?php
$seq = [1, 2, 3, 4, 5];
foreach ($seq as $n) {
    echo "Il quadrato di ", $n, " e' ", $n ** 2, "\n";
}
{% endhighlight %}

#### Esercizio 1:
Scrivi un programma PHP che crei un array con 10 numeri interi e li stampi sul video con un ciclo foreach (echo)

#### Esercizio 2:
Scrivi un programma PHP che crei un array con 10 numeri interi e 10 stringhe e stampi il tutto sul video con un ciclo foreach (echo)

#### Esercizio 3:
Scrivi un programma PHP che trasformi l'array del punto 2 in una stringa e la stampi (usa l'operatore `.` di concatenazione stringa oppure `implode()`)

#### Esercizio 4:
Scrivi un programma PHP che trovi l'indice di un elemento all'interno di un array (funzione `array_search()`)

#### Esercizio 5:
Scrivi un programma PHP che inverta l'ordine degli elementi in un array (crea un nuovo array con gli elementi invertiti: ciclo foreach e `array_merge`, oppure usa direttamente `array_reverse()`)

#### Esercizio 6:
Scrivi un programma PHP che trovi gli elementi comuni tra due array (funzione `array_intersect()`)

Ricorda che:

{% highlight php %}
<?php
// posso destrutturare un array in più variabili
$tuplex = [4, 8, 3];
[$n1, $n2, $n3] = $tuplex;
{% endhighlight %}

#### Esercizio 7:
Scrivi un programma PHP che destrutturi un array costituito da 10 interi e li mostri a monitor

Ricorda che:

{% highlight php %}
<?php
// lunghezza di un array
$tuplex = [4, 8, 3];
count($tuplex);  // ritorna 3
{% endhighlight %}

#### Esercizio 8:
Scrivi un programma PHP che prenda da un array il secondo elemento e il penultimo e li scriva a monitor

#### Esercizio 9:
Scrivi un programma PHP che scriva in ordine crescente gli elementi contenuti all'interno di un array senza modificare l'array originale (crea una copia con `$copia = $originale;`, dato che in PHP assegnare un array ne fa automaticamente una copia, a differenza degli oggetti)

#### Esercizio 10:
Scrivi un programma PHP che definisca un array di array a due elementi
 $lista = \[\["Gianni", 10\], \["Michele", 12\], \["Cristina", 18\]\];
 e scandendo l'array ne scriva a monitor tutto il contenuto

#### Esercizio 11:
Scrivi un programma PHP che scriva gli elementi del punto 10 prima in ordine alfabetico e poi in ordine numerico (funzioni `usort()`).

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, mostrando come cambia il valore di ogni variabile ad ogni iterazione del ciclo.

#### Esercizio 12:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$n` e `$acc` ad ogni iterazione):

{% highlight php %}
<?php
$numeri = [2, 4, 6, 8];
$acc = 1;
foreach ($numeri as $n) {
    $acc = $acc * $n;
}
echo $acc;
{% endhighlight %}

#### Esercizio 13:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$tuplex` dopo la sua creazione e i valori di `$n1`, `$n2`, `$n3` dopo l'assegnamento):

{% highlight php %}
<?php
$tuplex = [4, 8, 3];
[$n1, $n2, $n3] = $tuplex;
$n1 = $n1 + $n2;
$n2 = $n3;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 14:

Questo programma genera un errore di sintassi.

{% highlight php %}
<?php
$numeri = [1, 2, 3, 4, 5];
foreach ($numeri as $x) {
    echo $x
}
{% endhighlight %}

#### Esercizio 15:

A differenza di Python, dove modificare una tupla genera un `TypeError`, questo programma **funziona senza errori**: individuate perché, e spiegate qual è la reale differenza tra un array PHP e una tupla Python.

{% highlight php %}
<?php
$numeri = [1, 2, 3, 4, 5];
$numeri[0] = 10;
print_r($numeri);
{% endhighlight %}

#### Esercizio 16:

Questo programma non genera errori ma contiene un **errore logico**: `$n1`, `$n2` e `$n3` non ricevono i valori attesi. Individuate l'errore.

{% highlight php %}
<?php
$tuplex = [4, 8, 3];
[$n1, $n2, $n3] = [$tuplex, $tuplex, $tuplex];
print_r([$n1, $n2, $n3]);
{% endhighlight %}
