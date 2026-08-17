---
title: 'La condizione'
date: '2026-08-17T09:40:00+01:00'
author: Fabio Mattei
layout: page
---

## Il costrutto if

Il costrutto **if**
ci consente di cambiare la sequenza logica di istruzioni da eseguire in un programma. Il blocco di istruzioni racchiuso tra graffe viene eseguito se la condizione risulta verificata.

#### Esercizio 1:
copia il seguente codice nell'editor. Una volta finito eseguilo.

{% highlight php %}
<?php
$a = 33;
$b = 200;
if ($b > $a) {
    echo "b e' maggiore di a";
}
{% endhighlight %}

A differenza di Python, in PHP la condizione va sempre racchiusa tra **parentesi tonde**, e il blocco di istruzioni da eseguire va racchiuso tra **parentesi graffe** `{ }`. L'indentazione (gli spazi che precedono l'istruzione echo) in PHP **non ha alcun significato per l'interprete**: serve solo a rendere il codice leggibile per noi umani. Sono le graffe a delimitare i blocchi di codice, non gli spazi.

#### Esercizio 2:
copia il seguente codice nell'editor. Una volta finito eseguilo.

{% highlight php %}
<?php
$a = 33;
$b = 200;
if ($b > $a)
    echo "b e' maggiore di a";
{% endhighlight %}

Questo codice, a differenza di Python (dove l'assenza di indentazione genera un errore), **è perfettamente valido in PHP**: se il blocco `if` contiene una sola istruzione, le graffe sono facoltative e quell'unica istruzione appartiene comunque all'if. È comunque buona pratica usare sempre le graffe, anche per un blocco di una sola riga, per evitare errori quando in futuro si aggiungono altre istruzioni.

![Diagramma di flusso della condizione if/elseif/else](/images/php/la-condizione/la-condizione.svg){:class="half-image"}

####  Operatori di confronto

Sono gli operatori che possiamo utilizzare all'interno di una condizione.

- Uguale: `$a == $b`
- Identico (valore e tipo): `$a === $b`
- Diverso: `$a != $b`
- Minore: `$a < $b`
- Minore o uguale: `$a <= $b`
- Maggiore: `$a > $b`
- Maggiore o uguale: `$a >= $b`

L'operatore `===` non ha equivalente diretto in Python: confronta sia il valore sia il **tipo**. `"5" == 5` restituisce `true` (i valori sono numericamente uguali), ma `"5" === 5` restituisce `false` (una stringa non è mai identica ad un intero).

##  Il costrutto elseif

Importante per dare una alternativa in caso la proposizione contenuta nel primo if non venga verificata. Si scrive **tutto attaccato**: `elseif`, non `else if` (anche se PHP accetta entrambe le forme).

#### Esercizio 3:
copia il seguente codice nell'editor. Una volta finito eseguilo.

{% highlight php %}
<?php
$a = 33;
$b = 33;
if ($b > $a) {
    echo "b e' piu' grande di a";
} elseif ($a == $b) {
    echo "a e b sono uguali";
}
{% endhighlight %}

## Il costrutto else

Nel caso nessuna delle proposizioni contenute nelle condizioni precedenti sia stata verificata, si esegue il codice contenuto nel blocco else:

#### Esercizio 4:
copia il seguente codice nell'editor. Una volta finito eseguilo.

{% highlight php %}
<?php
$a = 33;
$b = 33;
if ($b > $a) {
    echo "b e' piu' grande di a";
} elseif ($a == $b) {
    echo "a e b sono uguali";
} else {
    echo "a e' piu' grande di b";
}
{% endhighlight %}

#### Esercizio 5:
scrivi un programma che lette due stringhe di testo ne scriva la prima in ordine alfabetico

#### Esercizio 6:
scrivi un programma che letti due numeri scriva il più grande tra i due

#### Esercizio 7:
scrivi un programma che letto un numero intero determini se è pari o dispari (utilizzare l'operatore resto: %)

#### Esercizio 8:
Dati 4 numeri in input determinare se la somma dei primi due è minore o uguale alla somma del terzo e del quarto

#### Esercizio 9:
Un'automobile percorre 20 km con un litro di benzina. Calcolare la spesa necessaria a percorrere 100 km. Se la spesa è maggiore di €100, applicare uno sconto del 5%

#### Esercizio 10:
Letti i lati di un triangolo determinare se è scaleno, isoscele o equilatero

#### Esercizio 11:
Letti due numeri naturali A e B, sottrarre il più piccolo dal più grande.

#### Esercizio 12:
Letti due numeri determinare se sono entrambi compresi tra 100 e 200

#### Esercizio 13:
Letto un numero intero, trovare il suo valore assoluto (funzione `abs()`).

#### Esercizio 14:
Letti due numeri interi A e B verificare se A è il quadrato di B

#### Esercizio 15:
Un'azienda elettrica ha stabilito le seguenti tariffe:

| KW/H         |                                                                                   |
|--------------|-----------------------------------------------------------------------------------|
| 0 – 500      | 20                                                                                |
| 501 – 1000   | 20 + 0,08 per ogni KW/H oltre i 500                                               |
| 1001 – oltre | 20 + 0,08 per ogni KW/H compreso tra 500 e 1000 + 0,05 per ogni KW/H oltre i 1000 |

Scrivere un programma che letto il consumo mensile calcoli e stampi l'importo della bolletta.

#### Esercizio 16:
Scrivi un programma PHP che legga il valore di una spesa e calcoli lo sconto secondo la seguente tabella:

| Spesa                |  Sconto        |
|----------------------|----------------|
| Al di sotto di 100 € | nessuno sconto |
| Tra 100 e 300        | sconto del 10% |
| Tra i 300 e i 500    | sconto del 15% |
| Tra i 500 e i 800    | sconto del 20% |

#### Esercizio 17:
Simona deve comperare le paste per il suo bar.
Le tariffe applicate dal pasticciere sono le seguenti: Se le paste sono fino a 20 il prezzo è di 1 euro
per ciascuna pasta Se le paste sono più di 20 ma meno di 40 il prezzo è di 0.9 euro per pasta.
Se le paste sono più di 40 ma meno di 60 il prezzo è di 0.8 euro per pasta Se le paste sono più di 100
il prezzo è di 0.6 euro per pasta Scrivi il programma che aiuta il panettiere a calcolare l'ammontare che
Simona dovrà pagare.

#### Esercizio 18:
Tenendo conto degli scaglioni fiscali definiti correntemente:

| Reddito         | Aliquota |
|-----------------|----------|
| 0 - 15000 €     | 23%      |
| 15001 - 28000 € | 25%      |
| 28001 - 50000 € | 35%      |
| 50001 in su   € | 43%      |

Scrivere un programma che letto il reddito di 5 cittadini italiani calcoli l'ammontare delle tasse che ciascun cittadino deve pagare ed il totale pagato da tutti i cittadini.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete eseguire il codice: dovete dire quale blocco di istruzioni viene eseguito e cosa viene stampato sulla console, motivando la risposta in base al valore delle variabili.

#### Esercizio 19:

Dato il seguente programma, indicate cosa viene stampato quando `$a = 15` e quando `$a = 25`:

{% highlight php %}
<?php
$a = 15;
if ($a < 10) {
    echo "piccolo";
} elseif ($a < 20) {
    echo "medio";
} else {
    echo "grande";
}
{% endhighlight %}

#### Esercizio 20:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$a`, `$b` e `$messaggio` dopo ogni riga), sapendo che `$a = 7` e `$b = 12`:

{% highlight php %}
<?php
$a = 7;
$b = 12;
if ($a > $b) {
    $messaggio = "a maggiore";
} elseif ($a == $b) {
    $messaggio = "uguali";
} else {
    $messaggio = "b maggiore";
}
echo $messaggio;
{% endhighlight %}

#### Esercizio 21:

Costruite la tabella di tracciamento del seguente programma per `$n = 8` (mostrate il valore di `$n` e quale ramo dell'if viene eseguito):

{% highlight php %}
<?php
$n = 8;
if ($n % 2 == 0) {
    $n = intdiv($n, 2);
    echo "pari, dimezzato:", $n;
} else {
    $n = $n * 3 + 1;
    echo "dispari:", $n;
}
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 22:

{% highlight php %}
<?php
$a = 33;
$b = 200;
if ($b > $a) {
    echo "b e' maggiore di a";
{% endhighlight %}

#### Esercizio 23:

{% highlight php %}
<?php
$a = 33;
$b = 33;
if ($b > $a) {
    echo "b e' piu' grande di a";
} elif ($a == $b) {
    echo "a e b sono uguali";
}
{% endhighlight %}

#### Esercizio 24:

{% highlight php %}
<?php
$a = 33;
$b = 33;
if ($b > $a) {
    echo "b e' piu' grande di a";
} elseif ($a == $b) {
    echo "a e b sono uguali";
else {
    echo "a e' piu' grande di b";
}
{% endhighlight %}

#### Esercizio 25:

Questo programma non contiene errori di sintassi (viene eseguito senza generare errori), ma contiene un **errore logico**: individuatelo e spiegate perché il messaggio stampato è sbagliato quando `$voto` vale, ad esempio, 6.

{% highlight php %}
<?php
$voto = 6;
if ($voto > 6) {
    echo "sufficiente";
} else {
    echo "insufficiente";
}
{% endhighlight %}
