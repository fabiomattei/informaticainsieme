---
title: 'Cercare e filtrare testo: find, findstr, sort e altri'
date: '2026-07-26T10:10:00+02:00'
author: Fabio Mattei
layout: page
---

Windows mette a disposizione alcuni comandi pensati per cercare, filtrare e ordinare testo, spesso usati in combinazione con la [pipe]({{ site.baseurl }}{% link _cmd/pipe-e-redirect.md %}.html) per elaborare l'output di altri comandi.

## find - cerca una stringa esatta

{% highlight shell %}
find "errore" appunti.txt
{% endhighlight %}

Cerca la stringa "errore" all'interno del file `appunti.txt` e mostra tutte le righe che la contengono. `find` cerca solo testo semplice (nessun carattere jolly o espressione regolare).

Opzioni principali:

{% highlight shell %}
find /i "errore" appunti.txt     ignora maiuscole/minuscole (case-insensitive)
find /v "errore" appunti.txt     mostra le righe che NON contengono la stringa
find /c "errore" appunti.txt     conta quante righe contengono la stringa (mostra solo il numero)
find /n "errore" appunti.txt     mostra anche il numero di riga
{% endhighlight %}

Si usa spesso in pipe, per filtrare l'output di un altro comando:

{% highlight shell %}
tasklist | find "chrome"
ipconfig | find "IPv4"
{% endhighlight %}

## findstr - cerca con caratteri jolly ed espressioni regolari

`findstr` è la versione più potente di `find`: supporta ricerche parziali, espressioni regolari ed è **case-sensitive** di default (al contrario di `find`).

{% highlight shell %}
findstr "errore" appunti.txt
{% endhighlight %}

Opzioni principali:

{% highlight shell %}
findstr /i "errore" appunti.txt      ignora maiuscole/minuscole
findstr /n "errore" appunti.txt      mostra il numero di riga
findstr /c:"errore critico" file.txt cerca la stringa esatta indicata (utile se contiene spazi)
findstr /s "errore" *.txt            cerca ricorsivamente in tutte le sottocartelle
findstr /r "err.*e" appunti.txt      usa un'espressione regolare (qui: "err" seguito da qualsiasi carattere e poi "e")
{% endhighlight %}

Si può cercare più di una parola contemporaneamente (funziona come un OR logico):

{% highlight shell %}
findstr "errore avviso" appunti.txt
{% endhighlight %}

Mostra tutte le righe che contengono almeno una delle due parole.

## sort - ordina righe di testo

{% highlight shell %}
sort elenco.txt
{% endhighlight %}

Legge il file (o l'input da una pipe) e ne stampa le righe in ordine alfabetico.

{% highlight shell %}
sort /r elenco.txt            ordine inverso (dalla Z alla A)
sort /+10 elenco.txt          ordina a partire dal decimo carattere di ogni riga
sort elenco.txt /o ordinato.txt   salva il risultato in un nuovo file
{% endhighlight %}

Esempio in pipe: ordinare l'elenco dei processi per nome

{% highlight shell %}
tasklist | sort
{% endhighlight %}

## more - visualizza il testo una schermata alla volta

{% highlight shell %}
more appunti.txt
{% endhighlight %}

Utile quando un file (o un output) è più lungo di una schermata: si avanza premendo la barra spaziatrice, riga per riga con Invio, e si esce con `Q`.

{% highlight shell %}
dir /s | more
{% endhighlight %}

## where - trova il percorso di un eseguibile

{% highlight shell %}
where python
{% endhighlight %}

Cerca il file `python.exe` in tutte le cartelle elencate nella variabile d'ambiente `%PATH%` e ne mostra il percorso completo. È l'equivalente Windows del comando `which` di Linux/macOS.

## clip - copia l'output negli appunti

{% highlight shell %}
dir | clip
{% endhighlight %}

Invece di stampare a schermo, invia l'output direttamente negli appunti di Windows: basta poi premere `Ctrl+V` in un qualsiasi programma per incollarlo.
