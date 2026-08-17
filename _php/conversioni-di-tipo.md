---
title: 'Conversioni di tipo'
date: '2026-08-17T09:25:00+01:00'
author: Fabio Mattei
layout: page
---

## Perché convertire i tipi

In PHP ogni valore ha un tipo: `int`, `float`, `string`, `bool` e così via.
A volte è necessario trasformare un valore da un tipo a un altro. Il caso
più comune è la lettura da tastiera: quando leggiamo un valore da `STDIN`
otteniamo **sempre una stringa**, anche se l'utente ha digitato un numero. Per poter
fare calcoli è necessario convertirla esplicitamente.

{% highlight php %}
<?php
$testo = trim(fgets(STDIN));
echo gettype($testo);   // string
$numero = (int) $testo;
echo gettype($numero);  // integer
{% endhighlight %}

La funzione `gettype()` restituisce il tipo di una variabile ed è utile per
fare debug.

---

## (int) — da stringa o float a intero

`(int) $x` converte `$x` in un numero intero. Se `$x` è un `float`, la parte
decimale viene **troncata** (non arrotondata). Se `$x` è una stringa, PHP legge
i primi caratteri numerici validi e si ferma al primo carattere non numerico.

#### Esercizio 1
Copia il seguente codice nell'editor ed eseguilo.

{% highlight php %}
<?php
echo (int) 3.9, "\n";     // 3  (troncato, non arrotondato)
echo (int) 3.1, "\n";     // 3
echo (int) "42", "\n";    // 42
echo (int) "-7", "\n";    // -7
{% endhighlight %}

Per convertire stringhe che rappresentano numeri in basi diverse da 10, PHP
mette a disposizione la funzione `intval()` con un secondo parametro:

{% highlight php %}
<?php
echo intval("1010", 2), "\n";   // 10  (binario)
echo intval("FF", 16), "\n";    // 255 (esadecimale)
echo intval("17", 8), "\n";     // 15  (ottale)
{% endhighlight %}

Attenzione: a differenza di Python, `(int) "3.14"` **non genera un errore**,
restituisce semplicemente `3`, perché PHP legge i caratteri numerici finché può
e poi si ferma.

{% highlight php %}
<?php
echo (int) "3.14";  // 3
{% endhighlight %}

---

## (float) — da stringa o intero a numero decimale

`(float) $x` converte `$x` in un numero a virgola mobile.

#### Esercizio 2
Copia il seguente codice nell'editor ed eseguilo.

{% highlight php %}
<?php
echo (float) 5, "\n";        // 5
echo (float) "3.14", "\n";   // 3.14
echo (float) "2e3", "\n";    // 2000  (notazione scientifica)
{% endhighlight %}

La conversione da `int` a `float` è utile per evitare la divisione intera:

{% highlight php %}
<?php
$a = 7;
$b = 2;
echo $a / $b, "\n";           // 3.5  (PHP divide sempre con la precisione necessaria)
echo intdiv($a, $b), "\n";    // 3    (quoziente intero)
echo (float) $a / $b, "\n";   // 3.5
{% endhighlight %}

---

## (string) — da numero a stringa

`(string) $x` converte `$x` nella sua rappresentazione testuale.
In PHP, a differenza di Python, la funzione `strval()` è equivalente al cast `(string)`,
e la concatenazione con `.` converte automaticamente i numeri in stringa senza bisogno del cast.

#### Esercizio 3
Copia il seguente codice nell'editor ed eseguilo.

{% highlight php %}
<?php
$eta = 25;
echo "Ho " . $eta . " anni\n";       // Ho 25 anni (conversione automatica!)
echo "Ho " . strval($eta) . " anni\n"; // stesso risultato, cast esplicito
{% endhighlight %}

A differenza di Python, dove `"Ho " + 25 + " anni"` genera un `TypeError`, in
PHP l'operatore `.` converte automaticamente i numeri in stringa. Questo rende
il cast a `string` meno indispensabile che in Python, ma è comunque buona
pratica renderlo esplicito quando il tipo non è ovvio a chi legge il codice.

---

## (bool) — da qualsiasi valore a booleano

`(bool) $x` converte `$x` in `true` o `false` secondo le regole di **truthiness**
di PHP: quasi tutto è `true`, tranne i valori considerati "vuoti" o "nulli".

#### Esercizio 4
Copia il seguente codice nell'editor ed eseguilo.

{% highlight php %}
<?php
var_dump((bool) 0);       // false
var_dump((bool) 0.0);     // false
var_dump((bool) "");      // false  (stringa vuota)
var_dump((bool) "0");     // false  (la stringa "0" è un caso speciale!)
var_dump((bool) []);      // false  (array vuoto)
var_dump((bool) null);    // false

var_dump((bool) 1);       // true
var_dump((bool) -5);      // true
var_dump((bool) "ciao");  // true
var_dump((bool) [0]);     // true  (array con un elemento)
{% endhighlight %}

Attenzione ad un caso che non esiste in Python: la stringa `"0"` è considerata
`false` in PHP, mentre in Python `bool("0")` vale `True` perché la stringa
non è vuota. È una delle trappole più comuni per chi passa da un linguaggio
all'altro.

---

## chr() e ord() — caratteri e codici ASCII

Ogni carattere è associato a un numero intero nella tabella **ASCII**. `chr($n)`
restituisce il carattere corrispondente al codice `$n`; `ord($c)` fa il
contrario. Queste due funzioni si chiamano esattamente come in Python.

#### Esercizio 5
Copia il seguente codice nell'editor ed eseguilo.

{% highlight php %}
<?php
echo chr(65), "\n";   // A
echo chr(97), "\n";   // a
echo chr(48), "\n";   // 0

echo ord('A'), "\n";  // 65
echo ord('a'), "\n";  // 97
echo ord('0'), "\n";  // 48
{% endhighlight %}

Le lettere maiuscole vanno da 65 (`A`) a 90 (`Z`), le minuscole da 97 (`a`)
a 122 (`z`). La differenza tra maiuscola e minuscola è sempre 32.

{% highlight php %}
<?php
echo ord('a') - ord('A'), "\n";   // 32
echo chr(ord('A') + 32), "\n";    // a  (da maiuscola a minuscola)
{% endhighlight %}

---

## Tabella riepilogativa

| Cast / Funzione     | Converte a | Note                                      |
|----------------------|------------|-------------------------------------------|
| `(int) $x`           | `int`      | Tronca i decimali, legge il prefisso numerico di una stringa |
| `intval($x, $base)`  | `int`      | Converte stringa in base 2, 8, 16…        |
| `(float) $x`         | `float`    | Accetta notazione scientifica             |
| `(string) $x`        | `string`   | Spesso implicita con l'operatore `.`      |
| `(bool) $x`          | `bool`     | false per 0, "", "0", [], null; true altrimenti |
| `chr($n)`             | `string`   | Codice ASCII/Unicode → carattere          |
| `ord($c)`             | `int`      | Carattere → codice ASCII/Unicode          |
| `gettype($x)`         | `string`   | Restituisce il nome del tipo di `$x`      |

---

## Esercizi

#### Esercizio 6
Scrivi un programma che legga due numeri interi dall'utente e stampi la loro
somma, differenza, prodotto e quoziente (con decimali).

#### Esercizio 7
Scrivi un programma che legga un numero decimale dall'utente e stampi
separatamente la parte intera e la parte decimale.
(Suggerimento: la parte decimale si ottiene sottraendo la parte intera.)

#### Esercizio 8
Scrivi un programma che legga dall'utente un numero in binario (come stringa)
e stampi il suo valore in base 10.

#### Esercizio 9
Scrivi un programma che legga un carattere dall'utente e stampi il suo codice
ASCII. Se il carattere è una lettera minuscola, stampa anche la corrispondente
lettera maiuscola senza usare `strtoupper()` (usa `chr` e `ord`).

#### Esercizio 10
Scrivi un programma che stampi la tabella ASCII dei caratteri stampabili,
ovvero i caratteri con codice da 32 a 126, nel formato:
```
32  (spazio)
33  !
34  "
...
```

#### Esercizio 11
Scrivi un programma che legga il nome dell'utente e la sua età come stringa,
poi costruisca e stampi la frase:
`"Tra 10 anni, Nome avrà X anni."`
dove X è l'età aumentata di 10.

#### Esercizio 12
Scrivi un programma che legga tre voti (numeri decimali) e stampi la media
arrotondata a due cifre decimali (funzione `round()`) e il valore intero della
media (troncato).

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, indicando per ogni variabile sia il **valore** sia il **tipo** dopo ogni riga di codice.

#### Esercizio 13:

Costruite la tabella di tracciamento del seguente programma (mostrate valore e tipo di `$x` dopo ogni riga):

{% highlight php %}
<?php
$x = "10";
$x = (int) $x;
$x = $x + 5;
$x = (string) $x;
$x = $x . " anni";
{% endhighlight %}

#### Esercizio 14:

Costruite la tabella di tracciamento del seguente programma (mostrate valore e tipo di `$n` dopo ogni riga):

{% highlight php %}
<?php
$n = 7.9;
$n = (int) $n;
$n = (float) $n;
$n = $n / 2;
{% endhighlight %}

#### Esercizio 15:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$c` e `$codice` ad ogni iterazione):

{% highlight php %}
<?php
$c = 'a';
$codice = ord($c);
$cont = 0;
while ($cont < 3) {
    $codice = $codice + 1;
    $c = chr($codice);
    $cont = $cont + 1;
}
echo $c;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 16:

Questo programma genera un errore di sintassi.

{% highlight php %}
<?php
$testo = trim(fgets(STDIN));
$numero = (int) $testo
echo $numero;
{% endhighlight %}

#### Esercizio 17:

Questo programma non genera l'errore che ci si aspetterebbe (a differenza di Python, che qui lancerebbe un `ValueError`): individuate perché il risultato stampato non è quello desiderato.

{% highlight php %}
<?php
$testo = "3.14";
$numero = (int) $testo;
echo $numero;
{% endhighlight %}

#### Esercizio 18:

Questo programma non genera errori ma contiene un **errore logico**: il programmatore, abituato a Python, pensava che servisse un cast esplicito. Il risultato stampato è comunque quello atteso: spiegate perché, a differenza di Python, questo codice funziona.

{% highlight php %}
<?php
$eta = 25;
echo "Ho " . $eta . " anni";
{% endhighlight %}
