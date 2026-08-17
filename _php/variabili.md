---
title: Variabili
date: '2026-08-17T09:10:00+01:00'
author: Fabio Mattei
layout: page
---

![Diagramma di variabili come cassetti etichettati che contengono un valore](/images/php/variabili/variabili.svg){:class="aside-image"}

Le variabili sono **contenitori di informazioni**. Possiamo pensare ad una variabile come ad un cassetto, dotato di etichetta, che può contenere una informazione.
Lo pensiamo come un cassetto dotato di etichetta perché ogni variabile ha un nome, il nome ci serve per ricordare cosa contiene e per trovarla facilmente tra
le tante variabili che andremo a creare per ciascun nostro programma.

In PHP il nome di ogni variabile inizia sempre con il simbolo **$**. Questo è il primo grande cambiamento rispetto a linguaggi come Python: il simbolo dollaro fa parte del nome della variabile, non è un operatore.

Al fine di creare o inizializzare una variabile PHP si avvale dell'operatore di assegnazione indicato dal simbolo **=**, e ogni istruzione termina con il **punto e virgola** `;`, in questo modo:

{% highlight php %}
<?php
$x = 5;
$y = "John";
$z = 3.564;
{% endhighlight %}

Facciamo bene attenzione, questa non è una equazione matematica ma una assegnazione, si legge in questo modo: **assegno alla variabile x il numero 5**.
Significa che nella memoria del computer verrà conservato uno spazio (il cassetto) che avrà come etichetta la **$x**. All'interno di questo spazio sarà
posto il **numero 5**.

Quindi ricordiamo:
* il simbolo alla sinistra del simbolo = è il **nome della variabile** (che inizia sempre con $)
* il simbolo alla destra del simbolo = il **valore da conservare** all'interno della variabile
* ogni istruzione termina con **;**

A differenza di Python, in PHP il codice va sempre racchiuso tra i tag `<?php` e (facoltativamente) `?>`. Se il file contiene solo codice PHP, il tag di chiusura può essere omesso.

## Regole per i nomi delle variabili

* deve iniziare con il simbolo $ seguito da una lettera o dal simbolo underscore (_)
* non può iniziare con un numero
* può contenere solo caratteri alfanumerici e underscore (A-z, 0-9, e _ )
* i nomi delle variabili sono case-sensitive ($eta e $Eta sono due variabili diverse)

## Esercizi

#### Esercizio 1:
Copia il seguente codice nell'editor. Una volta finito eseguilo da riga di comando con `php nomefile.php`.

{% highlight php %}
<?php
$nome = "Mario";
$eta = 15;
echo $nome;
echo $eta;
{% endhighlight %}

#### Esercizio 2:
Crea tre variabili: il tuo nome, la tua età e la classe che frequenti, quindi stampale a video usando `echo`.

#### Esercizio 3:
Crea due variabili numeriche `$a` e `$b`, scambia i loro valori (senza indovinare i valori a mano, usando una terza variabile di appoggio) e stampali.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, cioè una tabella che mostra come cambia il valore di ogni variabile ad ogni istruzione eseguita.

#### Esercizio 4:

Costruite la tabella di tracciamento del seguente programma (mostrate il valore di `$x` dopo ogni riga):

{% highlight php %}
<?php
$x = 3;
$x = $x + 2;
$x = $x * 2;
{% endhighlight %}

#### Esercizio 5:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$a` e `$b` dopo ogni riga):

{% highlight php %}
<?php
$a = 2;
$b = 5;
$a = $b;
$b = $a;
{% endhighlight %}

Cosa noti di strano nel risultato finale? Come lo risolveresti?

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 6:

{% highlight php %}
<?php
x = 5;
echo $x;
{% endhighlight %}

#### Esercizio 7:

{% highlight php %}
<?php
$nome = "Giacomo"
echo $nome;
{% endhighlight %}

#### Esercizio 8:

{% highlight php %}
<?php
$1nome = "Giacomo";
echo $1nome;
{% endhighlight %}
