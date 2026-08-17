---
title: 'La ricorsione'
date: '2026-08-17T10:20:00+01:00'
author: Fabio Mattei
layout: page
---

Problema: devo lavare una pila di 15 piatti.
Soluzione:

- lavo un piatto;
- devo lavare 14 piatti.

Sembra un ragionamento banale ma il suo scopo è quello di sottolineare come un problema possa essere scomposto utilizzando la sua stessa definizione. Il problema devo lavare 15 piatti viene scomposto nel problema lavo un piatto e devo lavare 14 piatti. Generalizzando possiamo scrivere che devo lavare n piatti viene scomposto nel problema lavo un piatto e devo lavare n – 1 piatti.

La ricorsione (in inglese recursion) è una tecnica di programmazione molto potente che sfrutta l'idea di lavorare sulla definizione stessa del problema che stiamo risolvendo al fine di risolverlo attraverso l'algoritmo più semplice che si possa immaginare. Il concetto è identico in PHP e in Python: cambia solo la sintassi.

## Ragioniamo con i numeri

Supponiamo di voler calcolare la somma dei primi n numeri interi. Quello che facciamo consiste nel lavorare sulla definizione stessa del problema. Ragioniamo sui numeri, calcoliamo la somma dei primi 4 numeri interi: *1 + 2 + 3 + 4 = 10*

Se immaginiamo di aver definito una funzione *sommainteri(n)* dove n rappresenta il numero di interi da sommare, possiamo scrivere:

*sommainteri(4) = 1 + 2 + 3 + 4 = 10*

Ma questo equivale a scrivere:

*(1 + 2 + 3) + 4 = sommainteri(3) + 4*

Come abbiamo ragionato? Abbiamo lavorato sulla definizione stessa della funzione decomponendola in due parti:

*sommainteri(n) = sommainteri(n − 1) + n*

Questa relazione è valida fintantoché n > 0. Parafrasando in PHP:

{% highlight php %}
<?php
function sommainteri($n) {
    if ($n > 0) {
        return $n + sommainteri($n - 1);
    } else {
        return 0;
    }
}
{% endhighlight %}

La soluzione appena trovata presenta un aspetto interessante: per risolvere il problema ci siamo basati sul poter risolvere lo stesso problema per un numero più piccolo. Questo approccio viene definito ricorsivo.

Un algoritmo ricorsivo per la risoluzione di un dato problema deve essere definito nel modo seguente:

- passo base: si definisce come risolvere il problema quando questo ha dimensione minima e può essere risolto in maniera estremamente semplice;
- passo ricorsivo: si definisce come ottenere la soluzione del problema come composizione di un problema analogo ma di dimensione inferiore e una operazione semplice.

## Ricorsione Diretta

Una funzione si dice ricorsiva quando all'interno della propria definizione compare una chiamata diretta a se stessa. Questa forma di ricorsione si chiama ricorsione diretta.

Un esempio di ricorsione diretta è la funzione per calcolare il fattoriale di un numero n:

Osserviamo che:

*n! = n * (n − 1) * … * 2 * 1 = n * (n − 1)!*

Quindi la definizione induttiva del fattoriale è:

- (passo base) 0! = 1
- (passo ricorsivo) n! = n * (n−1)! (se n > 0).

{% highlight php %}
<?php
function fattoriale($n) {
    if ($n == 0) {
        return 1;
    } else {
        return $n * fattoriale($n - 1);
    }
}
{% endhighlight %}

Condizioni come `$n == 0` si chiamano clausole di chiusura perché garantiscono che la ricorsione termini.

Esistono due requisiti che sono basilari per essere sicuri che la ricorsione funzioni:

- ogni invocazione ricorsiva deve semplificare in qualche modo l'elaborazione;
- devono esistere casi speciali che gestiscano in modo diretto le elaborazioni più semplici.

Occorre però fare molta attenzione: cosa succede se si calcola il fattoriale di -1? La clausola di chiusura non sarebbe mai verificata e il sistema andrebbe in un loop infinito (in PHP, un errore fatale `Allowed memory size exhausted` o `Maximum function nesting level reached`, a seconda della configurazione). Per evitare che ciò accada facciamo una piccola modifica.

{% highlight php %}
<?php
function fattoriale($n) {
    if ($n == 0) {
        return 1;
    } elseif ($n < 0) {
        echo "Errore nell'input";
    } else {
        return $n * fattoriale($n - 1);
    }
}
{% endhighlight %}

In questo modo la funzione fattoriale riesce sempre a concludere la propria elaborazione.

## Ricorsione indiretta

Si parla di ricorsione indiretta quando nella definizione di una funzione compare la chiamata ad un'altra funzione la quale direttamente o indirettamente chiama la funzione iniziale.

Un esempio di ricorsione indiretta

{% highlight php %}
<?php
function numero_pari($n) {
    if ($n == 0) {
        return true;
    } else {
        return numero_dispari($n - 1);
    }
}

function numero_dispari($n) {
    return !numero_pari($n);
}
{% endhighlight %}

Possiamo notare come la funzione numero_pari abbia al suo interno la clausola di chiusura e la chiamata alla funzione numero_dispari. Quest'ultima ha al suo interno la chiamata alla funzione numero_pari.

## Ricorsione Multipla

Una funzione implementa una ricorsione multipla quando al suo interno compare, almeno due volte, la chiamata a se stessa.

Un classico esempio di ricorsione multipla è l'implementazione dei numeri di Fibonacci, la cui definizione è riportata sotto:

- fib(0)=0
- fib(1)=1
- fib(n)=fib(n−1)+fib(n−2) (se n>1)

{% highlight php %}
<?php
function fib($n) {
    if ($n < 0) {
        echo "Errore nell'input";
    } elseif ($n == 0) {
        return 0;
    } elseif ($n == 1) {
        return 1;
    } else {
        return fib($n - 1) + fib($n - 2);
    }
}
{% endhighlight %}

Notare che, come per il fattoriale, la funzione è definita solo su interi non negativi.

## La ricorsione e gli array

È possibile utilizzare algoritmi ricorsivi per operare sugli array. Se per esempio volessimo sommare tutti i numeri contenuti in un array potremmo operare nel seguente modo:

{% highlight php %}
<?php
$numeri = [6, 1, 7, 3];
function s_lista($lista) {
    if (count($lista) == 0) {
        return 0;
    } else {
        $primo = $lista[0];
        $resto = array_slice($lista, 1);
        return $primo + s_lista($resto);
    }
}
{% endhighlight %}

- passo base: se l'array è vuoto la somma dei numeri al suo interno è 0;
- passo ricorsivo: se l'array non è vuoto la somma dei numeri al suo interno è pari al primo numero in array cui va sommata il risultato della somma del sotto-array ottenuto togliendo all'array il primo numero (`array_slice($lista, 1)`).

Dato che una stringa può essere trasformata in un array di caratteri con `str_split()`, è possibile operare su questa in modo simile.

{% highlight php %}
<?php
function reverse($stringa) {
    if (strlen($stringa) == 0) {
        return $stringa;
    } else {
        return reverse(substr($stringa, 1)) . $stringa[0];
    }
}
{% endhighlight %}

`substr($stringa, 1)` restituisce la stringa privata del primo carattere, l'equivalente PHP dello slicing `string[1:]` di Python.

## Esercizi

#### Esercizio 1:
Scrivere una funzione ricorsiva che controlli se una stringa è palindroma (ovvero se "rigirandola" non cambia, es. "ossesso" è palindroma).

Esempi di frasi palindrome:

I verbi brevi ("iverbibrevi") Aceto nell'enoteca ("acetonellenoteca") I topi non avevano nipoti ("itopinonavevanonipoti")

Definizione ricorsiva di palindromicità:

- Una stringa nulla è palindroma. Esempio: "".
- Una stringa di un carattere è palindroma. Esempio: "a".
- Una stringa avente il primo e l'ultimo carattere uguali e la sottostringa nel mezzo è palindroma, è palindroma. Esempio: "ossesso".

#### Esercizio 2:
Scrivere una funzione ricorsiva che analizzando una stringa in modo ricorsivo ne estragga, scrivendole in output (echo), le sole lettere vocali.

{% highlight php %}
<?php
echo "Digita la stringa da analizzare: ";
$analizzanda = trim(fgets(STDIN));
$vocali = ['a', 'e', 'i', 'o', 'u'];

function estrai_vocali($analizzanda, $vocali) {
    // scrivi tu questa funzione
}
{% endhighlight %}

#### Esercizio 3:
Scrivere una funzione ricorsiva che calcoli la somma delle cifre contenute in un numero. Es. f(325) = 10

Soluzione:
 (Caso Base) Se le cifre sono finite allora la somma delle sue cifre è zero
 (Passo generico) Se il numero è composto da tante cifre allora la somma delle sue cifre è data dalla somma della prima cifra più la somma delle cifre seguenti.

#### Esercizio 4:
Creare una funzione ricorsiva per calcolare una funzione definita così:
 per m>0 allora f(n,m) = 1+f(n,m-1)
 per m=0 allora f(n,m) = n
 Una volta implementata, provarla e dire cosa calcola la funzione.

#### Esercizio 5:
Scrivere il codice di una funzione ricorsiva f(n) che restituisce 0 nel caso n sia dispari, 1+f(n/2) altrimenti.

#### Esercizio 6:
Scrivere il codice di una funzione ricorsiva f(n) che restituisce quante coppie di cifre uguali in posizioni adiacenti ci sono nel numero n, nel caso n sia negativo restituisce 0.
*Ad es: f(551122) restituisce 3, f(5122) restituisce 1, f(9) restituisce 0.*

#### Esercizio 7:
Scrivere una funzione ricorsiva POT(n) per il calcolo dei numeri F(n) definiti dalle seguenti relazioni:

F(1) = 2
F(n)=2F(n−1) n≥2

#### Esercizio 7:
Scrivere una funzione ricorsiva che, avendo in input un array di n interi, dia in output il numero degli elementi positivi dell'array.

#### Esercizio 8:
Calcolo della potenza di un numero; scrivere una funzione ricorsiva potenza($num, $esponente)
 che calcoli la potenza di un numero.

#### Esercizio 9:
Stringa inversa: scrivere una funzione ricorsiva che presa come parametro una stringa di testo calcoli la stringa inversa.

#### Esercizio 10:

Scrivere una funzione ricorsiva che analizzando una stringa in modo ricorsivo ne estragga, scrivendole in output (echo), le sole lettere vocali.

#### Esercizio 11:

Creare una funzione ricorsiva per calcolare una funzione definita così:
per m>0 allora f(n,m) = 1+f(n,m-1)
per m=0 allora f(n,m) = n
Una volta implementata, provarla e dire cosa calcola la funzione.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete eseguire il codice: dovete costruire la **tabella di tracciamento** del programma, elencando in ordine tutte le chiamate ricorsive effettuate (con il rispettivo parametro) e il valore che ciascuna di esse restituisce, dalla più interna alla più esterna.

#### Esercizio 12:

Costruite la tabella di tracciamento delle chiamate ricorsive generate da `fattoriale(4)`, indicando per ciascuna chiamata il valore di `$n` e il valore restituito:

{% highlight php %}
<?php
function fattoriale($n) {
    if ($n == 0) {
        return 1;
    } else {
        return $n * fattoriale($n - 1);
    }
}

echo fattoriale(4);
{% endhighlight %}

#### Esercizio 13:

Costruite la tabella di tracciamento delle chiamate ricorsive generate da `sommainteri(4)`, indicando per ciascuna chiamata il valore di `$n` e il valore restituito:

{% highlight php %}
<?php
function sommainteri($n) {
    if ($n > 0) {
        return $n + sommainteri($n - 1);
    } else {
        return 0;
    }
}

echo sommainteri(4);
{% endhighlight %}

#### Esercizio 14:

Costruite la tabella di tracciamento delle chiamate ricorsive generate da `fib(4)` (attenzione: essendo una ricorsione multipla, da ogni chiamata ne partono altre due), indicando per ciascuna chiamata il valore di `$n` e il valore restituito:

{% highlight php %}
<?php
function fib($n) {
    if ($n == 0) {
        return 0;
    } elseif ($n == 1) {
        return 1;
    } else {
        return fib($n - 1) + fib($n - 2);
    }
}

echo fib(4);
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 15:

Questo programma genera un errore di sintassi.

{% highlight php %}
<?php
function fattoriale($n) {
    if ($n == 0)
        return 1;
    else {
        return $n * fattoriale($n - 1);
    }
}
{% endhighlight %}

#### Esercizio 16:

Questo programma non genera errori di sintassi, ma manca del **passo base**: eseguendolo va in errore per superamento del limite di memoria consentito. Individuate cosa manca e correggetelo.

{% highlight php %}
<?php
function sommainteri($n) {
    return $n + sommainteri($n - 1);
}

echo sommainteri(4);
{% endhighlight %}

#### Esercizio 17:

Questo programma non genera errori ma contiene un **errore logico**: la chiamata ricorsiva non semplifica il problema, quindi il ciclo di chiamate non termina mai correttamente. Individuate l'errore.

{% highlight php %}
<?php
function fattoriale($n) {
    if ($n == 0) {
        return 1;
    } else {
        return $n * fattoriale($n);
    }
}
{% endhighlight %}

#### Esercizio 18:

Questo programma non genera errori ma contiene un **errore logico**: `s_lista` dovrebbe restituire la somma di tutti i numeri dell'array, ma restituisce sempre 0. Individuate l'errore.

{% highlight php %}
<?php
function s_lista($lista) {
    if (count($lista) == 0) {
        return 0;
    } else {
        return s_lista(array_slice($lista, 1));
    }
}

$numeri = [6, 1, 7, 3];
echo s_lista($numeri);
{% endhighlight %}
