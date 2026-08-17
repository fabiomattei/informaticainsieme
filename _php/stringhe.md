---
title: Le stringhe di testo
date: '2026-08-17T09:20:00+01:00'
author: Fabio Mattei
layout: page
---

# Un susseguirsi di caratteri ascii

Una stringa di testo altro non è che un susseguirsi di caratteri ascii. Il computer manipola soltanto numeri binari ed utilizza la tabella ascii per associare un numero a ciascun simbolo al fine di essere in grado di manipolarlo e lavorarci.

Nello specifico vediamo che la porzione dedicata ai numeri e alle lettere è espressa nella tabella di fianco.

Questo significa che se vogliamo codificare la parola "CIAO" il computer memorizzerà una sequenza di codici ascii in questo modo:

|C  |I  |A   |O   |
| 01000011  | 01001001  | 01000001   | 01001111   |

Per questo motivo si dice che le stringhe di testo sono tipi di dato composti, in effetti sono composte da dati più piccoli: i caratteri.

# Inizializzare una stringa

{% highlight php %}
<?php
$saluto = "ciao";
{% endhighlight %}

Assegnare una stringa di testo corrisponde al memorizzare i caratteri che la compongono all'interno di una area di memoria e ad etichettare quell'area di memoria attraverso un nome. L'istruzione precedente si legge: **assegno alla variabile saluto la stringa di testo ciao** e corrisponde proprio all'atto appena descritto. L'assegnamento si esprime attraverso l'operatore =.

In PHP le stringhe si possono scrivere sia con apici doppi `"..."` sia con apici singoli `'...'`. La differenza importante è che solo gli apici doppi **interpretano** le variabili al loro interno:

{% highlight php %}
<?php
$nome = "Marco";
echo "Ciao $nome";   // scrive: Ciao Marco
echo 'Ciao $nome';   // scrive: Ciao $nome (non interpreta la variabile)
{% endhighlight %}

# Lunghezza di una stringa

Dato che abbiamo visto che una stringa è composta da caratteri può capitare di voler contare i caratteri che la compongono. Facciamo questo attraverso la funzione **strlen**:

{% highlight php %}
<?php
strlen("ciao");
{% endhighlight %}

La funzione strlen restituisce **un numero intero** pari al numero di caratteri che sono contenuti nella stringa.

Potrei assegnare un numero intero ad una variabile

{% highlight php %}
<?php
$numero_caratteri = strlen("ciao");
echo $numero_caratteri;
{% endhighlight %}

In questo esempio la funzione strlen **conta** quanti caratteri sono contenuti nella stringa di testo (4) e assegna questo numero appena calcolato alla variabile **numero_caratteri**.
In seguito la funzione echo scrive sulla console il contenuto della variabile numero_caratteri.

# Accediamo ad un singolo carattere contenuto in una stringa

Qualche volta ci capita di avere necessità di controllare, o scrivere un singolo carattere all'interno della stringa. In quel caso utilizziamo l'operatore \[\].

{% highlight php %}
<?php
$saluto = "ciao";
$singola_lettera = $saluto[1];
echo $singola_lettera;
{% endhighlight %}

PHP numera ciascun carattere contenuto in una stringa con un indice. Il conteggio inizia da 0 e finisce a n-1 dove n sono i caratteri che compongono la stringa. A differenza di Python, PHP non supporta gli **indici negativi** sulle stringhe (`$saluto[-1]` genera un errore prima di PHP 7.1, ed è comunque una funzionalità poco usata rispetto a Python).

|lettera|C  |I  |A   |O   |
|codice ascii| 01000011  | 01001001  | 01000001   | 01001111   |
|indice|0  |1  |2   |3   |

Il codice appena scritto dunque scrive la lettera **i**, cioè il simbolo corrispondente all'indice 1 all'interno della stringa **saluto**.

# Confrontiamo le stringhe

Gli operatori di confronto per le stringhe di testo sono i seguenti:

* == (è uguale)
* \> (viene dopo di)
* \>= (viene dopo di o è uguale a)
* < (viene prima di)
* <= (viene prima di o è uguale a)
* != (è diverso)

{% highlight php %}
<?php
"ciao" == "ciao";       // true
"ciao" > "ciao";        // false
"mattino" > "sera";     // false
"mattino" > "mattina";  // false
{% endhighlight %}

Il confronto tra stringhe di testo avviene considerando un carattere per volta di ciascuna stringa.

È possibile anche confrontare stringhe che sono all'interno di variabili

{% highlight php %}
<?php
$saluto = "ciao";
$momento = "mattino";
$saluto > $momento;      // false
{% endhighlight %}

Ovviamente i confronti fra stringhe possono essere utilizzati nelle espressioni booleane che sono all'interno delle condizioni:

{% highlight php %}
<?php
$saluto = "ciao";
$momento = "mattino";
if ($saluto > $momento) {
    echo $saluto;
} else {
    echo $momento;
}
{% endhighlight %}

#### Tabella di tracciamento

|saluto| momento  | output |
|"ciao"| "mattino"   | mattino   |

Devi comunque fare attenzione al fatto che PHP non gestisce le parole maiuscole e minuscole come facciamo noi in modo intuitivo: in un confronto le lettere maiuscole vengono sempre prima delle minuscole.

# Visitiamo le stringhe

Attraverso l'uso degli indici possiamo visitare una stringa di testo, cioè possiamo considerare un carattere per volta all'interno di un ciclo per farci delle operazioni.

{% highlight php %}
<?php
$saluto = "ciao";
$indice = 0;
while ($indice < strlen($saluto)) {
    $lettera = $saluto[$indice];
    if ($lettera == "a" || $lettera == "e" || $lettera == "i" || $lettera == "o" || $lettera == "u") {
        echo "vocale\n";
    } else {
        echo "consonante\n";
    }
    $indice = $indice + 1;
}
{% endhighlight %}

#### Tabella di tracciamento

|saluto| indice  | lettera  | output |
|"ciao"| 0  | "c"  | consonante   |
| |1  | "i"  | vocale   |
| |2  | "a"  | vocale   |
| |3  | "o"  | vocale   |

A differenza di Python, in PHP non si può scrivere direttamente `foreach ($saluto as $lettera)` perché una stringa non è un array. Se vogliamo comunque usare un ciclo `for`, possiamo scandire l'indice, oppure trasformare la stringa in un array di caratteri con **str_split**:

{% highlight php %}
<?php
$saluto = "ciao";
foreach (str_split($saluto) as $lettera) {
    if ($lettera == "a" || $lettera == "e" || $lettera == "i" || $lettera == "o" || $lettera == "u") {
        echo "vocale\n";
    } else {
        echo "consonante\n";
    }
}
{% endhighlight %}


## Esercizi

#### Esercizio 1:
Scrivi un programma PHP che utilizzando `echo` e il carattere asterisco legga 3 numeri e generi il relativo istogramma
Es: input 3, 5, 6
***
*****
******

{% highlight php %}
<?php
// Ricorda che:
$nome = trim(fgets(STDIN)); // legge una riga da tastiera, restituisce sempre una stringa
strlen($nome);               // ritorna la lunghezza della stringa
echo str_repeat("#", 3);     // scrive 3 volte il carattere #
{% endhighlight %}

#### Esercizio 2:
Scrivi un programma PHP che lette due stringhe le scriva in ordine di lunghezza (prima la più corta)

#### Esercizio 4:
Scrivi un programma PHP che letta una stringa ne calcoli la lunghezza e la riscriva tante volte quanto è la sua lunghezza

#### Esercizio 5:
Scrivi un programma PHP che lette due stringhe le scriva in ordine alfabetico [con le stringhe si possono utilizzare gli operatori < e >]

#### Esercizio 6:
Scrivi un programma PHP che lette tre stringhe le scriva in ordine alfabetico

#### Esercizio 7:
Scrivi un programma PHP che letto un numero intero n e una stringa s scriva la stringa s solo se la sua lunghezza è maggiore di n

#### Esercizio 8:
Tabella di tracciamento
{% highlight php %}
<?php
$S = "mandolino";
$C = 1;
$K = "";
while ($C <= strlen($S)) {
    If ($C > 10) {
        $K = "X";
    else:
        $K = "P";
    }
    $C = $C + 1;
}
echo $K;
{% endhighlight %}

#### Esercizio 9:
Scrivi un programma PHP che visiti una stringa di testo e scriva sul display "vocale" ogni volta che incontra una vocale e "consonante" ogni volta che incontra una consonante.

#### Esercizio 10:
Scrivi un programma PHP che legga due stringhe di testo e componga una nuova stringa di testo alternando i caratteri delle stringhe iniziali.
Esempio
Str1: casa
Str2: rosa
Output: craossaa

#### Esercizio 11:
Scrivi un programma PHP che letta una stringa di testo conti quante vocali ci sono al suo interno.

#### Esercizio 12:
Scrivi un programma PHP che letta una stringa di testo crei una nuova stringa che sostituisca tutte le S della stringa letta (maiuscole e minuscole) con il carattere $ e tutte le E (maiuscole e minuscole) con il carattere €.

#### Esercizio 13:
Scrivi un programma PHP che letto un numero calcoli la somma delle cifre che lo compongono.
Es input = 124 output = 7

#### Esercizio 14:
Scrivi un programma PHP che crei una stringa di 10 vocali estratte casualmente e conti il numero di occorrenze di A al suo interno

#### Esercizio 15:
Generatore di password: Crea un programma che generi una password a lunghezza variabile casuale compresa tra 8 e 25

#### Esercizio 16:
Generatore di 10 password: Crea un programma che generi 10 password a lunghezza variabile casuale compresa tra 8 e 25

#### Esercizio 17:
Scrivi un programma che letta una stringa di testo messaggio ed un numero intero k (compreso tra 1 e 25) applichi alla stringa di testo messaggio l'algoritmo del cifrario di Cesare con chiave k.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, cioè una tabella che mostra riga per riga come cambia il valore di ogni variabile ad ogni iterazione del ciclo.

#### Esercizio 19:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$parola`, `$indice` e `$acc` ad ogni iterazione):

{% highlight php %}
<?php
$parola = "php";
$indice = 0;
$acc = "";
while ($indice < strlen($parola)) {
    $acc = $parola[$indice] . $acc;
    $indice = $indice + 1;
}
echo $acc;
{% endhighlight %}

Cosa calcola questo programma?

#### Esercizio 20:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$frase`, `$lettera` e `$conta` ad ogni iterazione):

{% highlight php %}
<?php
$frase = "casa mia";
$conta = 0;
foreach (str_split($frase) as $lettera) {
    if ($lettera == "a") {
        $conta = $conta + 1;
    }
}
echo $conta;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 21:

{% highlight php %}
<?php
$saluto = "ciao";
echo $saluto[0]
{% endhighlight %}

#### Esercizio 22:

{% highlight php %}
<?php
$saluto = 'ciao";
echo $saluto;
{% endhighlight %}

#### Esercizio 23:

{% highlight php %}
<?php
$saluto = "ciao";
$indice = 0;
while ($indice < strlen($saluto))
    echo $saluto[$indice];
    $indice = $indice + 1;
{% endhighlight %}

#### Esercizio 24:

Questo programma non contiene errori di sintassi (viene eseguito senza generare errori), ma contiene un **errore logico**: individuatelo e spiegate perché il ciclo non termina mai.

{% highlight php %}
<?php
$saluto = "ciao";
$indice = 0;
while ($indice < strlen($saluto)) {
    echo $saluto[$indice];
}
{% endhighlight %}
