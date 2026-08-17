---
title: 'Tipi di variabile'
date: '2026-08-17T09:15:00+01:00'
author: Fabio Mattei
layout: page
---

Una informazione, conservata in una variabile, ha sempre un tipo associato. **Il tipo della variabile determina l'insieme di valori che una variabile può assumere e le operazioni che possono manipolare tali valori**.

- interi (`int`) Es: 1, 5, 7, 1983, 20003
- numeri razionali (`float`) Es: 1.2, 1.4, 6.7, 7.0
- valori booleani (`bool`) Es: true, false
- valori stringa (`string`) Es: "ciao", 'Buongiorno'
- array (`array`) Es: [1, 2, 3]
- il valore nullo (`null`)

A differenza di Python, PHP non ha un tipo `complex` nativo nel linguaggio di base.

## Operazioni su numeri interi e numeri a virgola mobile

Per assegnare un numero ad una variabile basta utilizzare l'operatore = come visto nella pagina dedicata alle [variabili]({{ site.baseurl }}{% link _php/variabili.md %}.html).

{% highlight php %}
<?php
$M = 3;      // assegna a $M il numero intero 3
$M = 3.0;    // assegna a $M il numero razionale (float) 3.0
{% endhighlight %}

Le operazioni possibili su di una variabile di tipo **int** o **float** sono:

- $M+$M → somma
- $M\*$M → prodotto
- $M/$M → divisione (in PHP, come in Python 3, restituisce sempre un float se il risultato non è esatto)
- intdiv($M, $M) → quoziente intero
- $M%$M → modulo (resto della divisione intera, solo tra interi)
- $M\*\*$M → potenza

#### Esercizio 1:
copia il seguente codice nell'editor. Una volta finito eseguilo con `php nomefile.php`.

{% highlight php %}
<?php
$a = 2;
$b = 3;
$area = $a * $b;
$perimetro = $a * 2 + $b * 2;
echo $area, " ", $perimetro;
{% endhighlight %}

#### Operazioni sulle stringhe di testo

In informatica una stringa di testo è **una sequenza di caratteri con un ordine stabilito**. Facciamo qualche esempio:

{% highlight php %}
<?php
$M = "Prova";
$N = "casa";
{% endhighlight %}

Le operazioni possibili su di una variabile stringa sono:

- $M.$N → concatena la stringa $M ed $N (es. "Provacasa"). In PHP l'operatore di concatenazione è il **punto** `.`, non il `+`
- str_repeat($M, 3) → concatena 3 volte la stringa $M (es. "ProvaProvaProva")
- strlen($M) → restituisce la lunghezza di $M (es. 5)
- $M\[0\]···$M\[strlen($M)-1\] → restituisce i singoli caratteri della stringa. es: ($M\[0\]→P)

{% highlight php %}
<?php
$nome = "Giacomo";                  // assegnamento
$cognome = "Leopardi";              // assegnamento
$nome_cognome = $nome . $cognome;   // concatenazione "GiacomoLeopardi"
$nome_ripetuto = str_repeat($nome, 3); // ripetizione "GiacomoGiacomoGiacomo"
$lunghezza = strlen($nome);         // lunghezza 7
$iniziale = $nome[0];               // carattere G
{% endhighlight %}

#### Esercizio 2:
copia il seguente codice nell'editor ed eseguilo

{% highlight php %}
<?php
$a = 2;
$b = 3;
$area = $a * $b;
$perimetro = $a * 2 + $b * 2;
echo $area, " ", $perimetro;
{% endhighlight %}

#### Esercizio 3:
copia il seguente codice nell'editor ed eseguilo

{% highlight php %}
<?php
$a = " c i a o ";
$b = " mondo ";
$stringaconcatenata = $a . $b;
echo $stringaconcatenata;
{% endhighlight %}

In questi esempi incontriamo per la prima volta l'istruzione [echo]({{ site.baseurl }}{% link _php/print.md %}.html) che ci permette di stampare sul video il valore di una variabile.

#### Esercizio 4:
Stampare a video il perimetro di un quadrato avente lato l=4

#### Esercizio 5:
Stampare a video l'area di un quadrato avente lato l=5

#### Esercizio 6:
Stampare a video n volte, con n=10, la stringa s
 (es. con s="ciao" stamperà "ciaociaociaociaociaociaociaociaociaociao")

#### Esercizio 7:
Stampare a video una stringa lunga 4 caratteri al contrario (es. se s="lodi", il programma stampa "idol")

#### Esercizio 8:
Supponete di correre 10 km in 42 min e 42 sec. Stampate la vostra velocità media in km/minuto, km/h, miglia/minuto e miglia/h.

- Calcolate quanti secondi ci sono in 42 minuti e 42 secondi.
- A quante miglia corrispondono 10 km? (Suggerimento: ci sono 1,61 km in un miglio)
- La vostra velocità media è calcolata come distanza/tempo

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, cioè una tabella che mostra come cambia il valore (ed eventualmente il tipo) di ogni variabile ad ogni istruzione eseguita.

#### Esercizio 9:

Costruite la tabella di tracciamento del seguente programma (mostrate il valore e il tipo di `$M` dopo ogni riga):

{% highlight php %}
<?php
$M = 3;
$M = $M + 1.5;
$M = $M * 2;
{% endhighlight %}

#### Esercizio 10:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$nome`, `$cognome` e `$nome_cognome` dopo ogni riga):

{% highlight php %}
<?php
$nome = "Giacomo";
$cognome = "Leopardi";
$nome_cognome = $nome . $cognome;
$nome = "Alessandro";
$nome_cognome = $nome . $cognome;
{% endhighlight %}

#### Esercizio 11:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$a`, `$area` e `$perimetro` dopo ogni riga):

{% highlight php %}
<?php
$a = 2;
$b = 3;
$area = $a * $b;
$perimetro = $a * 2 + $b * 2;
$a = 5;
$area = $a * $b;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 12:

{% highlight php %}
<?php
$a = 2;
$b = 3;
$area = $a*$b
echo $area;
{% endhighlight %}

#### Esercizio 13:

{% highlight php %}
<?php
$nome = "Giacomo;
$cognome = "Leopardi";
echo $nome . $cognome;
{% endhighlight %}

#### Esercizio 14:

{% highlight php %}
<?php
$nome = "Giacomo";
$lunghezza = strlen $nome;
echo $lunghezza;
{% endhighlight %}
