---
title: Input
date: '2026-08-17T09:35:00+01:00'
author: Fabio Mattei
layout: page
---

Quando eseguiamo uno script PHP da riga di comando, la funzione **fgets(STDIN)** viene usata per consentire all'utente di immettere dati da tastiera, che verranno poi utilizzati dal programma. **Tali dati vanno memorizzati in una variabile** altrimenti vengono *dimenticati* immediatamente dal computer.

`STDIN` è una costante predefinita che rappresenta il flusso di input standard (la tastiera). `fgets(STDIN)` legge una riga intera digitata dall'utente, **incluso il carattere di a-capo finale**: per questo la racchiudiamo quasi sempre dentro `trim()`, una funzione che rimuove gli spazi bianchi (e l'a-capo) all'inizio e alla fine di una stringa.

{% highlight php %}
<?php
echo "Inserisci il tuo nome: ";
$nome = trim(fgets(STDIN));
// Il computer scrive sul video "Inserisci il tuo nome: "
// quindi rimane in attesa che l'utente scriva qualcosa.
// Supponiamo che l'utente scriva la stringa "Ezio"
echo $nome; // stampa 'Ezio'
{% endhighlight %}

Notate che, a differenza della funzione `input()` di Python, `fgets(STDIN)` **non accetta un messaggio come parametro**: dobbiamo scrivere noi il prompt con un `echo` separato, prima di leggere il dato.

#### Esercizio 1:
copia il seguente codice nell'editor. Una volta finito eseguilo con `php nomefile.php`.

{% highlight php %}
<?php
echo "scrivi il tuo nome: ";
$nome = trim(fgets(STDIN));
echo "Ciao ", $nome;
{% endhighlight %}

#### Esercizio 2:
copia il seguente codice nell'editor ed eseguilo.

{% highlight php %}
<?php
echo "scrivi il tuo nome: ";
$nome = trim(fgets(STDIN));
echo "scrivi il tuo cognome: ";
$cognome = trim(fgets(STDIN));
echo "Nome: ", $nome, " cognome: ", $cognome;
{% endhighlight %}

Abbiamo detto che per conservare le informazioni che l'utente digita il computer ha bisogno di [variabili]({{ site.baseurl }}{% link _php/variabili.md %}.html). Nell'esercizio precedente abbiamo richiesto all'utente due informazioni, la prima l'abbiamo conservata all'interno della variabile *$nome*, la seconda all'interno della variabile *$cognome*.

#### Esercizio 3:
copia il seguente codice nell'editor ed eseguilo.

{% highlight php %}
<?php
echo "scrivi il tuo nome: ";
$nome = trim(fgets(STDIN));
$lunghezza = strlen($nome);
echo "Il tuo nome e' lungo ", $lunghezza, " caratteri";
{% endhighlight %}

La funzione fgets(STDIN), abbiamo detto, restituisce il valore come una stringa di testo.

Nel caso in cui vogliamo chiedere all'utente un valore numerico abbiamo bisogno di cambiare il tipo attraverso il cast **(int)**.

#### Esercizio 4:
copia il seguente codice nell'editor ed eseguilo.

{% highlight php %}
<?php
echo "scrivi la lunghezza del lato: ";
$valorestringa = trim(fgets(STDIN));
$lato = (int) $valorestringa;
$area = $lato * $lato;
echo "L'area del quadrato di lato ", $lato, " vale ", $area;
{% endhighlight %}

#### Esercizio 5:
scrivi un programma che letta una variabile intera calcoli l'area del cerchio che ha per raggio il valore appena richiesto.

#### Esercizio 6:
Scrivi un programma PHP che legga un numero intero ad una cifra, calcoli il valore di n+nn+nnn e scriva il risultato.

#### Esercizio 7:
scrivi un programma che letta una parola calcoli l'area del cerchio che ha per raggio la lunghezza della parola appena richiesta

#### Esercizio 8:
scrivi un programma che letta una variabile intera indicante un tempo in ore, calcoli di quanti minuti è composto quel tempo

#### Esercizio 9:
scrivi un programma che letta una variabile intera indicante un tempo in ore, calcoli di quanti secondi è composto quel tempo

Qualche volta bisogna richiedere in input dei valori in virgola mobile: in quel caso si usa il cast **(float)**

#### Esercizio 10:
copia il seguente codice nell'editor ed eseguilo.

{% highlight php %}
<?php
echo "scrivi la lunghezza del lato: ";
$valorestringa = trim(fgets(STDIN));
$lato = (float) $valorestringa;
$area = $lato * $lato;
echo "L'area del quadrato di lato ", $lato, " vale ", $area;
{% endhighlight %}

    Operatori matematici:
    +, -, / (divisione), * (moltiplicazione), ** (potenza), intdiv() (divisione intera), % (resto della divisione intera).

#### Esercizio 11:
scrivi un programma che letti due valori interi calcoli le operazioni che si ottengono utilizzando tutti gli operatori matematici elencati e scriva il risultato

#### Esercizio 12:
scrivi un programma che letti due valori a virgola mobile calcoli le operazioni che si ottengono utilizzando tutti gli operatori matematici elencati e scriva il risultato

#### Esercizio 13:
copia il seguente codice nell'editor ed eseguilo.

{% highlight php %}
<?php
echo "Capitale iniziale: ";
$capitaleiniziale = (int) trim(fgets(STDIN));
echo "Tasso: ";
$tasso = (int) trim(fgets(STDIN));
echo "Anni: ";
$anni = (int) trim(fgets(STDIN));
$capitalefinale = $capitaleiniziale * (1 + $tasso) ** $anni;
echo "Dopo  ", $anni, " anni il capitale e' ", $capitalefinale, "\n";
echo "Dopo  ", $anni, " anni il capitale e' ", round($capitalefinale, 2);
{% endhighlight %}

La funzione **round($valore, $cifre)** arrotonda un valore ad una certa cifra decimale, esattamente come in Python.

#### Esercizio 14:
scrivi un programma che letta la distanza e il tempo calcoli la velocità di un corpo arrotondata alla terza cifra decimale. (Velocità = spazio / tempo)

#### Esercizio 15:
scrivi un programma che letto un numero rappresentante un valore imponibile calcoli l'iva e scriva il totale composto da imponibile sommato all'iva.

#### Esercizio 16:
scrivi un programma che letto un valore in euro scriva il valore di vecchie lire corrispondente. (1 euro = 1927,36 lire)

#### Esercizio 17:
scrivi un programma che letta una stringa di testo ed un numero intero, scriva la stringa di testo tante volte quanto è grande il numero.

#### Esercizio 18:
Scrivi un programma PHP che letto il raggio di una sfera ne calcoli il volume e scriva il risultato

#### Esercizio 19:
Scrivi un programma PHP che letto un numero capisca se è compreso tra 0 e 100 o tra 100 e 1000 o tra 1000 e 2000, e scriva l'intervallo di appartenenza.

#### Esercizio 20:
Scrivi un programma PHP che calcoli la somma di tre numeri interi, se i numeri sono uguali allora calcoli il triplo della somma e scriva il risultato.

#### Esercizio 21:
Scrivi un programma PHP che letti due numeri n e m scriva il numero più grande

#### Esercizio 22:
Scrivi un programma PHP che letti tre numeri l, n e m scriva il numero più grande

#### Esercizio 23:
Scrivere un programma PHP che letta una distanza da percorrere, letto quanta distanza una automobile copra per litro di carburante e letto il prezzo per litro del carburante calcoli e scriva la quantità di litri necessari a coprire la distanza e il costo da sostenere per coprire la distanza

#### Esercizio 24:
Scrivere un programma che letti tre numeri calcoli se i tre numeri costituiscono una terna pitagorica e in caso positivo scriva: "I numeri costituiscono una terna pitagorica"

#### Esercizio 25:
Scrivere un programma che letti tre numeri rappresentanti la lunghezza dei lati di un triangolo scriva se il triangolo è isoscele, scaleno o equilatero.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete eseguire il codice: dovete costruire la **tabella di tracciamento** del programma, indicando il valore di ogni variabile dopo ogni riga di codice, assumendo che l'utente digiti i valori indicati.

#### Esercizio 26:

Costruite la tabella di tracciamento del seguente programma, sapendo che l'utente digita "7" quando gli viene chiesto il lato (mostrate i valori di `$valorestringa`, `$lato` e `$area` dopo ogni riga):

{% highlight php %}
<?php
echo "scrivi la lunghezza del lato: ";
$valorestringa = trim(fgets(STDIN));
$lato = (int) $valorestringa;
$area = $lato * $lato;
echo "L'area del quadrato di lato ", $lato, " vale ", $area;
{% endhighlight %}

#### Esercizio 27:

Costruite la tabella di tracciamento del seguente programma, sapendo che l'utente digita nell'ordine "10" e "3" (mostrate i valori di `$a`, `$b` e `$risultato` dopo ogni riga):

{% highlight php %}
<?php
$a = (int) trim(fgets(STDIN));
$b = (int) trim(fgets(STDIN));
$risultato = $a;
$a = $b;
$b = $risultato;
$risultato = $a + $b;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 28:

Questo programma genera un errore di sintassi.

{% highlight php %}
<?php
echo "scrivi il tuo nome: ";
$nome = trim(fgets(STDIN);
echo "Ciao ", $nome;
{% endhighlight %}

#### Esercizio 29:

Questo programma non genera errori (a differenza di Python, PHP converte automaticamente le stringhe numeriche negli operatori aritmetici), ma contiene un **errore logico**: se l'utente digita 4, invece di stampare 16 stampa 44. Individuate quale operatore è stato scambiato con quale.

{% highlight php %}
<?php
echo "scrivi la lunghezza del lato: ";
$valorestringa = trim(fgets(STDIN));
$area = $valorestringa . $valorestringa;
echo "L'area del quadrato vale ", $area;
{% endhighlight %}

#### Esercizio 30:

Questo programma non genera errori ma contiene un **errore logico**: se l'utente digita 4, il programma dovrebbe stampare 16 ma stampa 8. Individuate l'errore.

{% highlight php %}
<?php
echo "scrivi la lunghezza del lato: ";
$valorestringa = trim(fgets(STDIN));
$lato = (int) $valorestringa;
$area = $lato + $lato;
echo "L'area del quadrato di lato ", $lato, " vale ", $area;
{% endhighlight %}
