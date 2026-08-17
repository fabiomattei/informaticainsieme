---
title: 'Redirect e pipe'
date: '2026-07-26T10:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Pipe che collega due comandi e redirect che scrive l'output in un file](/images/cmd/pipe-e-redirect/pipe-e-redirect.svg){:class="aside-image"}

Ogni comando del Prompt riceve normalmente l'input da tastiera e scrive il proprio output (e gli eventuali errori) a schermo. Windows permette di **rediregere** questi tre canali verso file, oppure di collegare l'output di un comando all'input di un altro tramite una **pipe**.

I tre canali si chiamano, con la terminologia classica:

* **stdin** (standard input): ciò che il comando legge come input
* **stdout** (standard output): ciò che il comando scrive come risultato normale
* **stderr** (standard error): ciò che il comando scrive in caso di errore

## > - reindirizza l'output su un file (sovrascrive)

{% highlight shell %}
dir > elenco.txt
{% endhighlight %}

Invece di stampare a schermo il contenuto della cartella, lo scrive nel file `elenco.txt`. Se il file esiste già, viene **sovrascritto**.

## >> - reindirizza l'output su un file (accoda)

{% highlight shell %}
echo Nuova riga >> elenco.txt
{% endhighlight %}

Come `>`, ma se il file esiste il testo viene **aggiunto in fondo**, senza cancellare il contenuto precedente.

## < - reindirizza l'input da un file

{% highlight shell %}
sort < elenco.txt
{% endhighlight %}

Invece di attendere che l'utente digiti del testo da tastiera, il comando `sort` legge il proprio input direttamente dal file `elenco.txt`.

## 2> - reindirizza solo gli errori

{% highlight shell %}
dir cartella_inesistente 2> errori.txt
{% endhighlight %}

Il numero `2` identifica per convenzione lo stderr (il numero `1`, di solito omesso, identifica lo stdout). In questo modo l'eventuale messaggio di errore finisce nel file, mentre l'output normale continua a comparire a schermo.

Per rediregere sia output che errori nello stesso file:

{% highlight shell %}
dir cartella_inesistente > tutto.txt 2>&1
{% endhighlight %}

`2>&1` significa "manda lo stderr dove sta già andando lo stdout" (deve essere scritto dopo il redirect di stdout, altrimenti non funziona come atteso).

## Il device speciale NUL

Per scartare completamente un output (senza salvarlo da nessuna parte) lo si può rediregere verso `NUL`, l'equivalente Windows di `/dev/null`:

{% highlight shell %}
dir > NUL 2>&1
{% endhighlight %}

## | - la pipe

La **pipe** (in italiano si potrebbe tradurre "tubo") collega l'output di un comando all'input del comando successivo, senza passare da un file intermedio:

{% highlight shell %}
dir | more
{% endhighlight %}

Qui l'elenco prodotto da `dir` viene passato a `more`, che lo mostra una schermata alla volta (utile quando l'output è più lungo di quanto stia in una singola schermata).

Si possono incatenare più pipe di seguito:

{% highlight shell %}
tasklist | find "chrome"
{% endhighlight %}

In questo esempio `tasklist` elenca tutti i processi in esecuzione e `find` filtra solo le righe che contengono la parola "chrome", mostrando quindi solo i processi di Chrome. È l'equivalente Windows della combinazione `ps | grep` di Linux.

## sort e find, i comandi da usare più spesso in pipe

{% highlight shell %}
dir | sort              ordina alfabeticamente l'output di dir
tasklist | find "PID"    mostra solo le righe contenenti la parola "PID"
type appunti.txt | find "errore" /c    conta (opzione /c) quante righe contengono "errore"
{% endhighlight %}
