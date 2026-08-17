---
title: 'Map, Filter e Reduce'
date: '2026-08-17T10:25:00+01:00'
author: Fabio Mattei
layout: page
---

## La funzione array_map

La funzione array_map() prende come parametri una funzione (o `null`) e uno o più array,
e restituisce un nuovo array contenente i risultati che si ottengono applicando la funzione passata a ciascuno degli elementi contenuti nell'array. A differenza di `map()` in Python, `array_map()` restituisce **subito un array vero e proprio**, non un iteratore da convertire con `list()`.

Facciamo un esempio:

{% highlight php %}
<?php
function raddoppia($n) {
    // Questa funzione prende un numero come parametro, lo raddoppia e restituisce il risultato
    return $n + $n;
}

$numbers = [1, 2, 3, 4];
$result = array_map('raddoppia', $numbers);
print_r($result);
{% endhighlight %}

{% highlight shell %}
# Output:
[2, 4, 6, 8]
{% endhighlight %}

Notate che il primo argomento è il **nome della funzione come stringa** (oppure, come vedremo, una funzione anonima). Possiamo ottenere lo stesso effetto utilizzando le funzioni anonime (closure) o, ancora più compatte, le **arrow function** introdotte in PHP 7.4.
La arrow function non è altro che una funzione molto breve, che sta su una sola riga, in pratica la funzione:

{% highlight php %}
<?php
function raddoppia($x) {
    return $x + $x;
}
{% endhighlight %}

Si trasforma in:

{% highlight php %}
<?php
fn($x) => $x + $x;
{% endhighlight %}

La arrow function si riconosce per la parola chiave *fn* seguita dal o dai parametri tra parentesi, seguiti dal simbolo `=>`.
Dopo di questo si scrive l'espressione per il calcolo del valore restituito. Non c'è bisogno della parola _return_.

A questo punto lo script con array_map può diventare:

{% highlight php %}
<?php
$numbers = [1, 2, 3, 4];
$result = array_map(fn($x) => $x + $x, $numbers);
print_r($result);
{% endhighlight %}

{% highlight shell %}
# Output:
[2, 4, 6, 8]
{% endhighlight %}

È possibile utilizzare array_map su due array contemporaneamente, si presume che i due array contengano lo stesso numero di elementi.
In questo caso è necessario che la funzione che va ad operare riceva due parametri e che operi su questi.

{% highlight php %}
<?php
$numbers1 = [1, 2, 3];
$numbers2 = [4, 5, 6];

$result = array_map(fn($x, $y) => $x + $y, $numbers1, $numbers2);
print_r($result);
{% endhighlight %}

{% highlight shell %}
# Output:
[5, 7, 9]
{% endhighlight %}

Questo script somma il primo elemento dell'array numbers1 con il primo dell'array numbers2, il secondo elemento dell'array numbers1 con il secondo dell'array numbers2 e il terzo elemento dell'array numbers1 con il terzo dell'array numbers2.

## La funzione array_filter

La funzione array_filter() prende come parametri un array e una funzione che restituisce un booleano, e restituisce un nuovo array contenente i soli valori dell'array per cui la funzione passata restituisce true.

{% highlight php %}
<?php
function isvocal($variabile) {
    // Questa funzione restituisce true quando riceve una vocale come parametro e false altrimenti
    $letters = ['a', 'e', 'i', 'o', 'u'];
    return in_array($variabile, $letters);
}

$listaDiLettere = ['g', 'e', 'e', 'j', 'k', 's', 'p', 'r'];

$lettereFiltrate = array_filter($listaDiLettere, 'isvocal');

echo "Le lettere filtrate sono:\n";
foreach ($lettereFiltrate as $s) {
    echo $s, "\n";
}
{% endhighlight %}

{% highlight shell %}
##### Output:
Le lettere filtrate sono:
e
e
{% endhighlight %}

**Attenzione, una trappola tipica di PHP**: `array_filter()`, a differenza del `filter()` di Python, **mantiene gli indici originali** invece di rinumerarli da 0. Se vuoi un array con indici consecutivi, devi passare il risultato ad `array_values()`.

{% highlight php %}
<?php
// Una lista contenente numeri pari e dispari.
$seq = [0, 1, 2, 3, 5, 8, 13];

// $numeriDispari contiene i numeri dispari della lista
$numeriDispari = array_values(array_filter($seq, fn($x) => $x % 2 != 0));
print_r($numeriDispari);

// $numeriPari contiene i numeri pari della lista
$numeriPari = array_values(array_filter($seq, fn($x) => $x % 2 == 0));
print_r($numeriPari);
{% endhighlight %}

{% highlight shell %}
# Output:
[1, 3, 5, 13]
[0, 2, 8]
{% endhighlight %}

## La funzione array_reduce

La funzione *array_reduce* riduce gli elementi di un array ad uno solo. È utile per sommare tutti gli elementi di un array o per concatenare, è utile ogni qual volta pensiamo al concetto di *accumulatore* visto quando abbiamo studiato i cicli.

La funzione *array_reduce* prende come parametri un array, una funzione che riceve due parametri (il **carry**, cioè il risultato accumulato finora, e l'elemento corrente), e un **valore iniziale**. A differenza di `reduce()` in Python, in PHP il valore iniziale **non è opzionale nella pratica**: se lo si omette, `array_reduce()` lo considera implicitamente `null`, il che può creare risultati inattesi con array vuoti o con operazioni come la moltiplicazione.

* Al primo passo prende il valore iniziale e il primo elemento dell'array e li passa alla funzione calcolando il risultato
* Ad ogni passo successivo prende il risultato calcolato al passo precedente (il carry) e lo passa come primo parametro della funzione, come secondo parametro passa l'elemento successivo dell'array
* Il processo continua fino all'esaurimento dell'array passato
* Il risultato finale viene restituito al chiamante della funzione *array_reduce*

Facciamo un esempio

{% highlight php %}
<?php
// inizializziamo l'array
$numeri = [1, 3, 5, 6, 2];

// usiamo array_reduce per calcolare la somma degli elementi di un array
echo "Somma degli elementi nella lista: ";
$somma = array_reduce($numeri, fn($carry, $item) => $carry + $item, 0);
echo $somma, "\n";

// usiamo array_reduce per calcolare l'elemento più grande in un array
echo "Massimo elemento della lista: ";
$massimo = array_reduce($numeri, fn($carry, $item) => $item > $carry ? $item : $carry, PHP_INT_MIN);
echo $massimo, "\n";
{% endhighlight %}

{% highlight shell %}
# Output:
Somma degli elementi nella lista: 17
Massimo elemento della lista: 6
{% endhighlight %}

#### Reduce con funzioni predefinite

PHP non ha un modulo `operator` equivalente a quello di Python, ma per le operazioni più comuni possiamo comunque scrivere arrow function molto brevi:

{% highlight php %}
<?php
$lista = [1, 3, 5, 6, 2];

// utilizziamo array_reduce per calcolare la somma degli elementi di un array
echo "Somma degli elementi nella lista: ";
echo array_reduce($lista, fn($carry, $item) => $carry + $item, 0), "\n";

// utilizziamo array_reduce per calcolare il prodotto degli elementi di un array
echo "Prodotto degli elementi della lista: ";
echo array_reduce($lista, fn($carry, $item) => $carry * $item, 1), "\n";

// utilizziamo array_reduce per concatenare gli elementi di un array
echo "Le stringhe concatenate sono: ";
echo array_reduce(["latte", "miele", "biscotti"], fn($carry, $item) => $carry . $item, ""), "\n";
{% endhighlight %}

{% highlight shell %}
# Output:
Somma degli elementi nella lista: 17
Prodotto degli elementi della lista: 180
Le stringhe concatenate sono: lattemielebiscotti
{% endhighlight %}
