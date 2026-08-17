---
title: 'I dizionari (array associativi)'
date: '2026-08-17T10:10:00+01:00'
author: Fabio Mattei
layout: page
---

Se le variabili sono cassetti e gli array indicizzati sono cassettiere, i dizionari sono cassettiere con etichette. In PHP i dizionari si chiamano **array associativi**: sono lo stesso tipo `array` usato per le liste, ma con chiavi testuali (o comunque non necessariamente numeriche in ordine) al posto degli indici numerici.

Un array associativo è una struttura dati che può accogliere molti dati. Ciascun dato è associato ad una etichetta, che d'ora in avanti chiameremo **chiave**, che ci permette di identificarlo in maniera univoca all'interno della struttura.

Gli array associativi sono **mutabili**. A differenza dei dizionari di Python, che dalla versione 3.7 mantengono l'ordine di inserimento come garanzia del linguaggio, anche gli array associativi PHP mantengono l'ordine di inserimento delle chiavi.

#### Definire gli array associativi

Gli array associativi vengono definiti elencando tra **parentesi quadre** una serie di elementi separati da **virgole**, dove ciascun elemento è formato da una chiave e un valore separati dal simbolo **freccia** `=>`. È possibile creare un array associativo vuoto usando le parentesi quadre senza alcun elemento al suo interno (è la stessa sintassi dell'array vuoto: la differenza sta in come lo si usa).

{% highlight php %}
<?php
$d1 = [];                        // array associativo vuoto
$d2 = ['a' => 1];                // contenente un elemento
$d3 = ['a' => 1, 'b' => 2, 'c' => 3]; // contenente 3 elementi

$ilmiodiz = [
    "lati" => 6,
    "parola" => "Saluti",
    "3x3" => 9,
    "area" => 4.3,
    13 => "Panino",
];
{% endhighlight %}

In questo esempio possiamo vedere che $d3 è un array associativo che contiene 3 elementi formati da una chiave e un valore. 'a', 'b' e 'c' sono le chiavi, mentre 1, 2 e 3 sono i valori. Le chiavi di un array associativo sono solitamente stringhe o interi. I valori possono essere di qualsiasi tipo.

{% highlight php %}
<?php
// chiave intera, valore array
$d = [20 => ['Jack', 'Jane'], 28 => ['John', 'Mary']];
{% endhighlight %}

#### Accedere agli elementi conservati in un array associativo

Una volta creato un array associativo, è possibile ottenere il valore associato a una chiave usando la sintassi **nome_array\[chiave\]**:

{% highlight php %}
<?php
// definisco un array associativo di esempio
$d = [
    'a' => 1,
    'b' => 2,
    'c' => 3,
    'campana' => 'rumorosa',
    10 => 'automobili'
];

$d['a'];       // ritorna il valore associato alla chiave 'a': 1
$d['c'];       // ritorna il valore associato alla chiave 'c': 3
$d['campana']; // ritorna 'rumorosa'
$d[10];        // ritorna 'automobili'
{% endhighlight %}

Se viene specificata una chiave inesistente, PHP genera un **warning** e restituisce `null` (a differenza di Python, che genera un'eccezione `KeyError` bloccante). È comunque possibile usare la funzione `array_key_exists()` (o `isset()`) per verificare se una chiave è presente:

{% highlight php %}
<?php
$d = ['a' => 1, 'b' => 2, 'c' => 3];
$d['x'];
// PHP: Warning: Undefined array key "x" e restituisce null
// Python, con lo stesso codice, lancerebbe: KeyError
{% endhighlight %}

#### array_key_exists() e isset()

Le funzioni **array_key_exists()** e **isset()** mi permettono di controllare se una specifica chiave è stata definita all'interno di un array associativo, esattamente come fanno gli operatori `in` / `not in` di Python sui dizionari.

*Attenzione: queste funzioni monitorano la presenza di una chiave ma non monitorano in alcun modo i valori associati alle chiavi stesse. Inoltre `isset()` restituisce `false` anche se la chiave esiste ma il suo valore è `null`, mentre `array_key_exists()` no: sono leggermente diverse.*

{% highlight php %}
<?php
if (array_key_exists('x', $d)) { // la chiave 'x' è presente in $d
    echo "Chiave x definita\n";
}

if (!array_key_exists('x', $d)) { // la chiave 'x' non è presente in $d
    echo "Chiave x non definita\n";
}

if (array_key_exists('b', $d)) { // la chiave 'b' è presente
    echo $d['b'], "\n"; // il valore associato alla chiave 'b' è 2
}
{% endhighlight %}

#### Aggiungere e modificare gli elementi di un array associativo

È possibile aggiungere o modificare elementi in un array associativo usando la sintassi **array\[chiave\] = valore**.

{% highlight php %}
<?php
// definisco un array associativo che contiene 3 elementi
$d = ['a' => 1, 'b' => 2, 'c' => 3];

// aggiungo un elemento
$d['k'] = 2020;
print_r($d);  // ['a' => 1, 'b' => 2, 'c' => 3, 'k' => 2020]

// modifico un elemento
$d['a'] = 123;
print_r($d);  // ['a' => 123, 'b' => 2, 'c' => 3, 'k' => 2020]
{% endhighlight %}

#### Rimuovere un elemento da un array associativo

È possibile rimuovere un elemento dall'array associativo usando la funzione **unset()**, con la sintassi: **unset(nome_array\[chiave\])**:

{% highlight php %}
<?php
// definisco un array associativo
$d = ['a' => 1, 'b' => 2, 'c' => 3];

// rimuove l'elemento (chiave e valore) con chiave 'a'
unset($d['a']);
print_r($d);  // ['b' => 2, 'c' => 3]
{% endhighlight %}

#### Visita di un array associativo

Visitare un array associativo significa utilizzare un ciclo per scandire tutti gli elementi che sono al suo interno al fine di fare con questi delle operazioni.

Di solito per visitare un array associativo si utilizza un ciclo `foreach`. Se ci interessano **solo le chiavi** usiamo la stessa sintassi vista per gli array indicizzati:

{% highlight php %}
<?php
$stati_e_capitali = [
    'Gujarat' => 'Gandhinagar',
    'Maharashtra' => 'Mumbai',
    'Rajasthan' => 'Jaipur',
    'Bihar' => 'Patna'
];

echo "Lista degli stati:\n";

// Visito l'array associativo prendendo le sole chiavi
foreach (array_keys($stati_e_capitali) as $stato) {
    echo $stato, "\n"; // scrive Gujarat, Maharashtra, Rajasthan, Bihar
}
{% endhighlight %}

Se volessi utilizzare, all'interno del ciclo, sia le chiavi sia i valori a queste associati, uso la forma speciale `foreach ($array as $chiave => $valore)`, che è più diretta di quella vista in Python:

{% highlight php %}
<?php
echo "Lista degli stati e delle relative capitali:\n";

// Visito l'array prendendo chiavi e valori insieme
foreach ($stati_e_capitali as $stato => $capitale) {
    echo $stato, ": ", $capitale, "\n";
}
{% endhighlight %}

## Esercizi

#### Esercizio 1:
- definisci un array associativo "persona1" che al suo interno abbia due elementi, il primo con chiave "nome" e valore "Mario", il secondo con chiave "cognome" e valore "Serenelli".
- definisci un array associativo "persona2" che al suo interno abbia due elementi, il primo con chiave "nome" e valore "Maria", il secondo con chiave "cognome" e valore "Giacobini".
- aggiungi a persona1 l'elemento a chiave "indirizzo" con valore "Via Giuseppe Verdi"
- scrivi un ciclo che permetta di scrivere tutto il contenuto di persona1
- scrivi un ciclo che permetta di scrivere tutto il contenuto di persona2
- definisci un array che contenga persona1 e persona2 appena definiti.

#### Esercizio 2:
Scrivi un algoritmo che concateni due array associativi creandone uno nuovo che contiene tutti gli elementi del primo e tutti gli elementi del secondo (funzione `array_merge()`).
dic1 = [1 => 10, 2 => 20]
dic2 = [3 => 30, 4 => 40]
Expected Result : [1 => 10, 2 => 20, 3 => 30, 4 => 40]

#### Esercizio 3:
Scrivi un programma PHP che letto un numero n generi un array associativo i cui elementi abbiano la forma x => x*x:
Esempio ( n = 5) :
Output : [1 => 1, 2 => 4, 3 => 9, 4 => 16, 5 => 25]

#### Esercizio 4:
Scrivi uno script PHP che controlli se due array associativi hanno le stesse chiavi (tutte le chiavi del primo sono definite nel secondo e viceversa)

#### Esercizio 5:
Scrivi uno script PHP che ricevuto un array associativo i cui valori sono tutti numeri interi, trovi il massimo valore e il minimo valore (funzioni `max()` e `min()` accettano anche gli array).

#### Esercizio 6:
Scrivi un programma PHP che controlli se un array associativo è vuoto oppure no (funzione `empty()`).

#### Esercizio 7:
Crea un array di array associativi che descriva il seguente problema di Knapsack:
m1: peso 23 valore 54
m2: peso 27 valore 59
m3: peso 19 valore 40
m4: peso 26 valore 57

#### Esercizio 8:

dato l'array associativo:
$voti = [
  'Fisica' => 8,
  'Matematica' => 6,
  'Storia' => 7
];

Scrivere il nome della disciplina con il voto più basso.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, mostrando come cambia il contenuto dell'array associativo (o delle variabili) ad ogni iterazione o istruzione.

#### Esercizio 9:

Costruite la tabella di tracciamento del seguente programma (mostrate il contenuto di `$d` dopo ogni riga):

{% highlight php %}
<?php
$d = ['a' => 1, 'b' => 2];
$d['c'] = 3;
$d['a'] = 10;
unset($d['b']);
{% endhighlight %}

#### Esercizio 10:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$chiave` e `$acc` ad ogni iterazione):

{% highlight php %}
<?php
$voti = ['Fisica' => 8, 'Matematica' => 6, 'Storia' => 7];
$acc = 0;
foreach ($voti as $chiave => $valore) {
    $acc = $acc + $valore;
}
echo $acc;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 11:

Questo programma genera un errore di sintassi.

{% highlight php %}
<?php
$d = ['a' => 1, 'b' => 2, 'c' => 3];
if (!array_key_exists('x', $d) {
    echo 'Chiave x non definita';
}
{% endhighlight %}

#### Esercizio 12:

Questo programma genera solo un **warning** e restituisce `null` (in Python genererebbe un `KeyError` bloccante): individuate quale istruzione lo causa e spiegate perché.

{% highlight php %}
<?php
$d = ['a' => 1, 'b' => 2, 'c' => 3];
echo $d['x'];
{% endhighlight %}

#### Esercizio 13:

Questo programma non genera errori ma contiene un **errore logico**: dovrebbe contare quante chiavi ha l'array associativo, ma il valore stampato è sempre sbagliato. Individuate l'errore.

{% highlight php %}
<?php
$d = ['a' => 1, 'b' => 2, 'c' => 3];
$conta = 0;
foreach ($d as $chiave => $valore) {
    $conta = 1;
}
echo $conta;
{% endhighlight %}
