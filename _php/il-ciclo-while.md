---
title: 'Il ciclo While'
date: '2026-08-17T09:50:00+01:00'
author: Fabio Mattei
layout: page
---

![Diagramma di flusso del ciclo while con controllo in testa](/images/php/il-ciclo-while/il-ciclo-while.svg){:class="aside-image"}

Il costrutto **while** in PHP permette di eseguire dei comandi un certo numero di volte. Si chiama ciclo perché i comandi contenuti al suo interno vengono ripetuti **ciclicamente**.

La sintassi è la seguente:

{% highlight php %}
<?php
while (<espressione booleana>) {
    <comando 1>
    <comando 2>
    <comando 3>
}
{% endhighlight %}

While è un ciclo con **controllo in testa**, questo significa che *il controllo sull'espressione booleana viene fatto prima di entrare nel ciclo*.

Le istruzioni all'interno del ciclo vengono eseguite se il risultato dell'espressione booleana è **vero**. In caso contrario (espressione booleana falsa) il blocco comandi interno al ciclo viene ignorato e l'esecuzione del programma continua con la prima istruzione successiva al ciclo, cioè quella che segue la graffa di chiusura.

Scrivi ed esegui il seguente programma:

{% highlight php %}
<?php
$cont = 0;
while ($cont < 10) {
    $cont = $cont + 1;
    echo $cont, "\n";
}
{% endhighlight %}

| cont | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|---|

Questo programma ci mostra un ciclo le cui istruzioni contenute all'interno vengono eseguite *fintanto che $cont < 10*. La **variabile $cont** si dice **contatore** dato che il suo scopo è contare il numero di volte che il ciclo è stato eseguito. Questo costrutto è molto diffuso.

Facciamo un secondo esempio:

{% highlight php %}
<?php
$cont = 0;
while ($cont < 10) {
    $cont = $cont + 1;
}
echo $cont;
{% endhighlight %}

Noterai che l'istruzione `echo $cont;` non è posta all'interno del ciclo (non è contenuta tra le graffe) dato che si trova dopo la graffa di chiusura del while. Questo ciclo scriverà sulla console il solo numero 10.

Ricorda che in PHP sono le **graffe**, non l'indentazione, a delimitare cosa appartiene al ciclo!

{% highlight php %}
<?php
$cont = 0;
while ($cont < 10) {
    $cont = $cont + 1;
    echo $cont, " ";
}
{% endhighlight %}

Cosa noti?

Scriviamo ora un programma che calcoli la somma dei primi 10 numeri interi

{% highlight php %}
<?php
$n = 10;
$cont = 1;             // variabile contatore
$acc = 0;               // variabile accumulatore
while ($cont <= $n) {
    $acc = $acc + $cont; // incremento l'accumulatore di cont
    $cont = $cont + 1;   // incremento il contatore di 1
}
echo $acc;              // al termine del ciclo scrivo il valore di acc
{% endhighlight %}

Nel precedente esempio abbiamo introdotto il concetto di **accumulatore**. Si dice accumulatore una variabile che *accumula dopo ciascuna esecuzione delle istruzioni all'interno del ciclo i risultati di un calcolo*. Nell'esempio ad $acc viene sommato di volta in volta il contenuto della variabile contatore. Possiamo vedere come le variabili si comportano in una tabella di tracciamento:

| **n** | 10 |  |  |  |  |  |  |  |  |  |  |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **cont** | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
| **acc** | 0 | 1 | 3 | 6 | 10 | 15 | 21 | 28 | 36 | 45 | 55 |

{% highlight php %}
<?php
// Questo programma calcola la sequenza di Fibonacci.
$acc_a = 0;
$acc_b = 1;
$cont = 0;
$max_cont = 20;
while ($cont < $max_cont) {
    $cont = $cont + 1;
    // Occorre tenere traccia finché ci sono cambiamenti
    $vecchio_acc_a = $acc_a;
    $vecchio_acc_b = $acc_b;
    $acc_a = $vecchio_acc_b;
    $acc_b = $vecchio_acc_a + $vecchio_acc_b;
    echo $vecchio_acc_a, " ";
}
echo "\n";
{% endhighlight %}

L'algoritmo di Fibonacci funziona su di un gioco di due variabili accumulatori: $acc_a e $acc_b.

{% highlight php %}
<?php
// Attende sino a quando non viene inserita la giusta password.
// Usate Control-C per fermare il programma senza password.
// Notate che se non viene inserita la giusta password, il ciclo
// while prosegue all'infinito.
$password = "foobar";
// Notate il simbolo != (diverso).
while ($password != "unicorn") {
    echo "Password: ";
    $password = trim(fgets(STDIN));
}
echo "Benvenuto";
{% endhighlight %}

#### Esercizio1:

Scrivere un programma che letto un numero intero N calcoli la somma di tutti i numeri da 1 ad N

#### Esercizio 2:

Scrivere un programma che letto un numero intero N ed un numero intero M (con N<M) calcoli la somma di tutti i numeri da N ad M (estremi inclusi)

#### Esercizio 3:

Scrivere un programma che letto un numero intero N calcoli il fattoriale di N ( 1 * 2 * 3 * 4 …. * N)

#### Esercizio 4:

Scrivere un programma che letto un numero intero N ed un numero intero M (con N<M) calcoli la somma di tutti i numeri pari compresi tra N ed M

#### Esercizio 5:

Scrivere un programma PHP che letti 10 numeri calcoli l'ammontare dei soli numeri pari.

#### Esercizio 6:

Scrivere un programma che letti N numeri in ingresso calcoli il numero massimo e il numero minimo inseriti

#### Esercizio 7:

Scrivere un programma che letti N numeri in ingresso calcoli la somma di tutti i numeri mostrando le somme parziali ogni 3 numeri inseriti

#### Esercizio 8:

Scrivere un programma che letti N numeri in ingresso calcoli la somma di tutti i numeri mostrando le somme parziali ogni 3 numeri inseriti

#### Esercizio 9:

Scrivere un programma che letto un numero intero N calcoli la somma di tutte le cifre dispari che lo compongono

#### Esercizio 10:

Scrivere un programma che letto un numero intero N scriva la tavola pitagorica della moltiplicazione del numero prescelto

#### Esercizio 11:

Scrivere un programma che scriva la tavola pitagorica della moltiplicazione completa

#### Esercizio 12:

Leonardo Pisano propose nel tredicesimo secolo il seguente problema:

Immaginiamo di chiudere una coppia di conigli in un recinto sapendo che per ogni coppia di conigli valgono le seguenti condizioni:

- Inizia a generare dal secondo mese di età
- Genera una nuova coppia ogni mese
- Non muore mai;

Quante coppie di conigli ci saranno dopo un anno?

E quante dopo un numero di mesi N, letto in input?

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, cioè una tabella che mostra riga per riga come cambia il valore di ogni variabile ad ogni iterazione del ciclo, esattamente come abbiamo fatto per l'esempio dell'accumulatore.

#### Esercizio 13:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$cont` e `$acc` ad ogni iterazione):

{% highlight php %}
<?php
$cont = 1;
$acc = 1;
while ($cont <= 5) {
    $acc = $acc * $cont;
    $cont = $cont + 1;
}
echo $acc;
{% endhighlight %}

Qual è il valore stampato alla fine? Che calcolo sta eseguendo questo programma?

#### Esercizio 14:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$a`, `$b` e `$cont` ad ogni iterazione):

{% highlight php %}
<?php
$a = 1;
$b = 10;
$cont = 0;
while ($a < $b) {
    $a = $a + 2;
    $b = $b - 1;
    $cont = $cont + 1;
}
echo $cont, " ", $a, " ", $b;
{% endhighlight %}

Attenzione: la condizione del ciclo dipende da due variabili che cambiano entrambe ad ogni iterazione.

#### Esercizio 15:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$n`, `$cont` e `$somma` ad ogni iterazione):

{% highlight php %}
<?php
$n = 8;
$cont = 1;
$somma = 0;
while ($cont <= $n) {
    if ($cont % 2 == 0) {
        $somma = $somma + $cont;
    }
    $cont = $cont + 1;
}
echo $somma;
{% endhighlight %}

#### Esercizio 16:

Costruite la tabella di tracciamento del seguente programma. Fate attenzione: in questo caso il ciclo potrebbe non venire mai eseguito, oppure eseguito un numero di volte diverso da quello che vi aspettate.

{% highlight php %}
<?php
$x = 20;
$cont = 0;
while ($x > 1) {
    $x = intdiv($x, 2);
    $cont = $cont + 1;
}
echo $cont;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 17:

{% highlight php %}
<?php
$cont = 0;
while ($cont < 5) {
    echo $cont;
    $cont = $cont + 1;
{% endhighlight %}

#### Esercizio 18:

{% highlight php %}
<?php
$cont = 0;
while ($cont < 5 {
    echo $cont;
    $cont = $cont + 1;
}
{% endhighlight %}

#### Esercizio 19:

{% highlight php %}
<?php
$cont = 0;
while ($cont < 5) {
    echo $cont;
    $cont = $cont + 1;
    print "Fine ciclo", "extra";
}
{% endhighlight %}

#### Esercizio 20:

{% highlight php %}
<?php
$cont = 0;
while ($cont < 5) {
    echo $cont;
    $cont = $cont 1;
}
{% endhighlight %}

#### Esercizio 21:

{% highlight php %}
<?php
$cont = 0;
while ($cont < 5) {
    echo $cont;
    $cont = $cont + 1;
    if $cont == 3
        echo "Siamo a meta";
}
{% endhighlight %}

#### Esercizio 22:

Questo programma non contiene errori di sintassi (viene eseguito senza generare errori), ma contiene un **errore logico**: individuatelo e spiegate perché il ciclo non termina mai.

{% highlight php %}
<?php
$cont = 0;
while ($cont < 5) {
    echo $cont;
    $cont = 0;
}
{% endhighlight %}
