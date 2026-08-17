---
title: 'Le liste (array indicizzati)'
date: '2026-08-17T10:00:00+01:00'
author: Fabio Mattei
layout: page
---

PHP fornisce un tipo built-in chiamato **array**. A differenza di Python, dove esistono tipi separati per liste, tuple e dizionari, in PHP **esiste un solo tipo array**, che può comportarsi sia come lista (indici numerici in ordine) sia come dizionario (chiavi testuali), come vedremo nelle prossime lezioni. In questa lezione lo usiamo come lista, cioè come sequenza mutabile di variabili, in genere omogenei.

Come per le variabili a ciascun array viene assegnato un nome. È possibile creare un array vuoto usando le parentesi quadre. In questo modo si viene a creare una struttura dati che non contiene elementi al suo interno ma è pronta a riceverne.

{% highlight php %}
<?php
$vuota = []; // nuovo array vuoto di nome: vuota
{% endhighlight %}

Se ho bisogno di creare un array cui intendo inserire dei valori al momento stesso della sua creazione è sufficiente che io elenchi tra parentesi quadre questi valori separati da virgole.

{% highlight php %}
<?php
$numeri = [0, 1, 2, 3];               // array di 4 elementi interi
$lettere = ['a', 'b', 'c', 'd', 'e']; // array di 5 elementi di tipo stringa (di un carattere)
$parole = ['mattina', 'pomeriggio'];  // array di 2 elementi di tipo stringa
{% endhighlight %}

Gli esempi in alto creano tre array: il primo si chiama numeri e contiene i quattro numeri: 0, 1, 2, 3; il secondo si chiama lettere e contiene i caratteri: 'a', 'b', 'c', 'd' ed 'e'; il terzo si chiama parole e contiene le stringhe di testo: 'mattina' e 'pomeriggio'.

#### Cosa posso fare con un array?

Una volta immagazzinate le informazioni in un array come le posso utilizzare? Le tecniche sono molteplici. Ogni array numera internamente i suoi elementi cominciando a contare dallo 0. Si fa riferimento all'indice usando la notazione: **nomearray\[#numero indice\]**. Gli indici dunque vanno da 0 a n − 1 dove n sono gli elementi dell'array. A differenza di Python, **PHP non supporta gli indici negativi**: `$lettere[-1]` non restituisce l'ultimo elemento, restituisce un errore (o `null`) perché in un array indicizzato -1 non è una chiave valida.

| indice | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| contenuto | 'a' | 'b' | 'c' | 'd' | 'e' |

Dunque sono sintatticamente corrette le seguenti assegnazioni:

{% highlight php %}
<?php
$prima_lettera = $lettere[0];                    // 'a'
$seconda_lettera = $lettere[1];                   // 'b'
$ultima_lettera = $lettere[count($lettere) - 1];  // 'e' (equivalente a lettere[-1] in Python)
{% endhighlight %}

Le cose funzionano anche al contrario, per esempio se eseguo le istruzioni:

{% highlight php %}
<?php
$lettere[0] = 'u';
$lettere[3] = 'k';
{% endhighlight %}

Mi ritroverò con i contenuti dell'array *lettere* variati nel seguente modo.

| indice | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| contenuto | 'u' | 'b' | 'c' | 'k' | 'e' |

#### Operatori e funzioni equivalenti

Python usa gli operatori `+` e `*` per concatenare e ripetere le liste. In PHP questo si ottiene con funzioni dedicate:

- `array_merge($a, $b)` → concatenazione: crea un nuovo array formato dall'unione degli elementi di $a con gli elementi di $b
- `array_merge(...array_fill(0, 3, $a))` → ripetizione: ripete gli elementi dell'array $a per un certo numero di volte

{% highlight php %}
<?php
// concatenazione
$vocali = array_merge(['a', 'e'], ['i', 'o', 'u']);
// risultato ['a', 'e', 'i', 'o', 'u']

// ripetizione: 3 copie di ['a', 'e']
$ripetute = array_merge([], ...array_fill(0, 3, ['a', 'e']));
// risultato ['a', 'e', 'a', 'e', 'a', 'e']
{% endhighlight %}

Cosa fa secondo te il seguente programma PHP?

{% highlight php %}
<?php
$classe = ['Gino', 'Sandra'];
echo "Digita il tuo nome: ";
$nuovo_alunno = trim(fgets(STDIN));
$classe[] = $nuovo_alunno;
{% endhighlight %}

Il programma nella prima istruzione crea un array di nome classe che contiene gli elementi 'Gino' e 'Sandra'. La seconda istruzione consiste nella lettura del nome di un nuovo alunno. La terza istruzione usa `$classe[] = $nuovo_alunno;`, la sintassi PHP per **aggiungere un elemento in coda** ad un array (l'equivalente del metodo `.append()` di Python).

## Slicing, facciamo gli array "a fette"

Attraverso la funzione **array_slice** si può prendere una fetta di un array, esattamente come lo slicing di Python.

{% highlight php %}
<?php
$lettere = ['a', 'b', 'c', 'd', 'e'];
$lettere_al_centro = array_slice($lettere, 1, 3); // ['b', 'c', 'd']
{% endhighlight %}

`array_slice($array, $inizio, $lunghezza)` estrae dall'array una sua sottosezione che parte dall'indice $inizio e comprende $lunghezza elementi. Attenzione: il secondo parametro di array_slice è una **lunghezza**, non un indice finale come nello slicing Python.

## in_array e !in_array

Ci sono funzioni che mi permettono di controllare se un elemento è contenuto in un array oppure no: `in_array()`. Chiariamoci le idee con un esempio:

{% highlight php %}
<?php
$lettere = ['a', 'b', 'c', 'd', 'e'];
if (in_array('c', $lettere)) {
    echo "Il carattere c e' contenuto nell'array chiamato lettere\n";
}
if (!in_array('k', $lettere)) {
    echo "Il carattere k NON e' contenuto nell'array chiamato lettere\n";
}
{% endhighlight %}

Gli array in PHP supportano anche altre funzioni:

| Funzione | Cosa fa | Esempio | Risultato |
|---|---|---|---|
| **count** | Calcola la lunghezza dell'array | count($lettere) | 5 |
| **min** | Trova l'elemento più piccolo | min($lettere) | 'a' |
| **max** | Trova l'elemento più grande | max($lettere) | 'e' |

## Visitiamo il nostro array

Visitare o percorrere l'array significa **prendere ad uno ad uno ciascuno degli elementi che lo compongono e applicare dei comandi a ciascuno di questi**. Volendo ad esempio stampare gli elementi che compongono un array posso scrivere il seguente codice:

{% highlight php %}
<?php
$lettere = ['a', 'b', 'c', 'd', 'e'];
foreach ($lettere as $x) {
    echo $x, "\n";
}
{% endhighlight %}

Ad ogni iterazione $x assumerà il valore di uno dei caratteri contenuto nell'array lettere e lo scriverà sullo schermo.

{% highlight php %}
<?php
$numeri = [1, 2, 3, 4, 5, 6];
foreach ($numeri as $x) {
    if ($x % 2 == 0) {
        echo $x, "\n";
    }
}
{% endhighlight %}

In questo secondo esempio **percorro** l'array numeri prendendo un numero per volta ed assegnandolo ad $x, quindi applica l'operatore modulo (resto della divisione intera) per vedere se il numero selezionato é pari e se ciò risulta vero lo scrivo.

{% highlight php %}
<?php
// stampa il quadrato di ogni numero di seq
$seq = [1, 2, 3, 4, 5];
foreach ($seq as $n) {
    echo "Il quadrato di ", $n, " e' ", $n ** 2, "\n";
}
{% endhighlight %}

È possibile visitare l'array anche utilizzando un ciclo **while**

{% highlight php %}
<?php
$numeri = [1, 2, 3, 4, 5, 6];
$indice = 0;
while ($indice < count($numeri)) {
    echo $numeri[$indice], "\n";
    $indice = $indice + 1;
}
{% endhighlight %}

## Esercizi sugli array

#### Esercizio 1:
Scrivi un programma PHP che inizializzi un array "nomi" contenente sei nomi a tua scelta. In seguito il programma scrive sullo schermo il nome inserito nell'array all'indice 3 e sostituisce il nome ad indice 5 con uno letto in input dall'utente.

#### Esercizio 1:
Scrivi un programma PHP in cui venga inizializzato un array di nomi contenente sei nomi a piacere e che scriva i nomi contenuti nell'array iniziale utilizzando un ciclo foreach.

#### Esercizio 2:
Scrivi un programma PHP in cui venga inizializzato un array di nomi contenente sei nomi a piacere e che utilizzando una stringa da utilizzarsi come accumulatore crei una stringa contenente tutti i nomi contenuti nell'array iniziale concatenati.

#### Esercizio 3:
Scrivi un programma PHP in cui venga inizializzato un array di nomi contenente sei nomi a piacere e che utilizzando una stringa da utilizzarsi come accumulatore crei una stringa contenente tutti i nomi contenuti nell'array iniziale concatenati in questo modo: il primo nome compare una volta, il secondo nome due volte, il terzo nome tre volte ecc…

#### Esercizio 4:
Scrivi un programma PHP in cui venga inizializzato un array di nomi contenente sei nomi a piacere e che crei un nuovo array contenente tutti i nomi contenuti nell'array iniziale in questo modo: il primo nome compare una volta, il secondo nome due volte, il terzo nome tre volte ecc…

#### Esercizio 5:
Scrivi un programma PHP in cui venga inizializzato un array di nomi contenente sei nomi a piacere e che crei un nuovo array contenente tutti i nomi contenuti nell'array iniziale in questo modo: i nomi in posizione ad indice pari compaiono una volta, quelli ad indice dispari compaiono due volte.

#### Esercizio 6:
Scrivi un programma PHP in cui venga inizializzato un array di nomi contenente sei nomi a piacere e che crei un nuovo array contenente tutti i nomi contenuti nell'array iniziale che iniziano per vocale.

#### Esercizio 7:
Inizializza un array comprendente numeri interi a caso a tua scelta, quindi scrivi un programma PHP che scandisca gli elementi dell'array creato e scriva a video i soli numeri dispari

#### Esercizio 8:
Inizializza due array di numeri interi a caso a tua scelta quindi scrivi una piccola funzione che confronti i due array e ritorni vero se hanno almeno un elemento in comune

#### Esercizio 9:
Scrivi un programma PHP che trovi il penultimo elemento più piccolo in un array

#### Esercizio 10:
Scrivi un programma PHP che calcoli la frequenza di elementi di un array (conta il ripetersi). Suggerimento: la funzione `array_count_values()` fa già questo lavoro.

#### Esercizio 11:
Scrivi un programma PHP che crei un array concatenando un array con un intervallo di numeri es:
 Esempio:
 Array iniziale : \['p', 'q'\]
 n =5
 Array in Output : \['p1', 'q1', 'p2', 'q2', 'p3', 'q3', 'p4', 'q4', 'p5', 'q5'\]

#### Esercizio 12:
 Scrivi un programma PHP che inizializzati due array con 10 elementi a piacere trovi gli elementi comuni tra i due array e per ciascuno di questi scriva: l'elemento x è comune.

#### Esercizio 13:
Scrivi un programma PHP che converta un array di multipli interi in un singolo intero
 Esempio:
 Array iniziale: \[11, 33, 50\]
 Intero in Output: 113350

#### Esercizio 14:
Scrivi un programma PHP che inizializzato un array di numeri interi calcoli il prodotto di tutti i numeri contenuti nell'array

#### Esercizio 15:
Scrivi un programma PHP che legga 10 numeri (utilizzare un ciclo), li memorizzi in un array e li scriva in ordine inverso (funzione `array_reverse()`).

#### Esercizio 16:
Scrivi un programma PHP che legga 10 numeri e li memorizzi in due array separati che si chiamano: numeri_grandi e numeri_piccoli. Nel primo array vanno i numeri più piccoli di 100, nel secondo quelli più grandi. I numeri negativi non vanno memorizzati.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, mostrando come cambia il contenuto dell'array (o delle variabili) ad ogni iterazione del ciclo.

#### Esercizio 17:

Costruite la tabella di tracciamento del seguente programma (mostrate il contenuto di `$numeri` e il valore di `$x` ad ogni iterazione):

{% highlight php %}
<?php
$numeri = [3, 8, 1];
for ($x = 1; $x < 4; $x++) {
    $numeri[] = $x * 2;
}
print_r($numeri);
{% endhighlight %}

#### Esercizio 18:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$lettere`, `$indice` e `$acc` ad ogni iterazione):

{% highlight php %}
<?php
$lettere = ['a', 'b', 'c', 'd'];
$indice = 0;
$acc = "";
while ($indice < count($lettere)) {
    $acc = $acc . $lettere[$indice];
    $indice = $indice + 1;
}
echo $acc;
{% endhighlight %}

#### Esercizio 19:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$numeri`, `$x` e `$massimo` ad ogni iterazione):

{% highlight php %}
<?php
$numeri = [4, 9, 2, 15, 6];
$massimo = $numeri[0];
foreach ($numeri as $x) {
    if ($x > $massimo) {
        $massimo = $x;
    }
}
echo $massimo;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 20:

Questo programma genera un errore di sintassi.

{% highlight php %}
<?php
$numeri = [1, 2, 3, 4, 5];
foreach ($numeri as $x) {
    echo $x;
{% endhighlight %}

#### Esercizio 21:

Questo programma genera un **warning** ed il valore `null` (in Python genererebbe un `IndexError`): individuate quale istruzione lo causa e spiegate perché.

{% highlight php %}
<?php
$numeri = [1, 2, 3, 4, 5];
echo $numeri[5];
{% endhighlight %}

#### Esercizio 22:

Questo programma non genera errori ma contiene un **errore logico**: dovrebbe stampare la somma di tutti gli elementi dell'array, ma stampa un valore sbagliato. Individuate l'errore.

{% highlight php %}
<?php
$numeri = [1, 2, 3, 4, 5];
$somma = 0;
foreach ($numeri as $x) {
    $somma = $x;
}
echo $somma;
{% endhighlight %}
