---
title: Caricamento dei dati da file e scrittura dati su file in PHP
date: '2026-08-17T10:30:00+01:00'
author: Fabio Mattei
layout: page
---

## Caricare i dati da file

Fino ad oggi abbiamo ricevuto dati dall'utente utilizzando `fgets(STDIN)` ma la console non rappresenta l'unico canale di input possibile, in effetti è possibile ricevere in input informazioni da un file di testo. Un file di testo altro non è che una sequenza di simboli ASCII, riconosciamo questi file dall'estensione del file **.txt**.

Il file viene aperto attraverso la chiamata alla funzione **fopen**, che prende come parametri il nome del file e il tipo di operazione che vogliamo fare sul file (chiamata **mode**):

* r apre un file per la lettura (default)
* w apre un file per la scrittura. Se il file non esiste ne crea uno nuovo, se il file esiste lo sovrascrive.
* x apre un file limitatamente per la creazione. Se il file esiste già l'operazione fallisce.
* a apre un file per la scrittura aggiungendo informazioni alla fine senza sovrascrivere le informazioni già presenti. Se il file non esiste lo crea.
* b apre il file in modalità binaria (da aggiungere al mode, es. "rb").
* + apre un file simultaneamente per la lettura e per la scrittura (es. "r+").

{% highlight php %}
<?php
$f = fopen("ilmiofilediinput.txt", "r"); // apre in lettura il file ilmiofilediinput.txt
$linee = file($f);                       // ottengo un array di stringhe in cui ogni stringa è una linea del file di testo
fclose($f);                              // chiude il file appena aperto
{% endhighlight %}

In alternativa, se ci basta leggere l'intero contenuto del file in un colpo solo (senza dividerlo per righe), possiamo usare la funzione **file_get_contents**, che apre, legge e chiude il file per noi:

{% highlight php %}
<?php
$contenuto = file_get_contents("ilmiofilediinput.txt");
echo $contenuto;
{% endhighlight %}

E se ci serve leggere riga per riga senza caricare tutto l'array in memoria, possiamo usare `fgets()` dentro un ciclo, esattamente come abbiamo fatto con `STDIN`:

{% highlight php %}
<?php
$f = fopen("ilmiofilediinput.txt", "r");
while (($riga = fgets($f)) !== false) {
    echo $riga;
}
fclose($f);
{% endhighlight %}

## Scrivere i dati su file

Se sono nella necessità di scrivere dati su di un file posso utilizzare la funzione fopen passando il parametro **w** per indicare che ho bisogno di scrivere sul file.
Andrò a scrivere nel file utilizzando la funzione **fwrite**, ma nota bene: `fwrite` accetta soltanto stringhe di testo, quindi eventuali numeri vanno convertiti in stringa (spesso implicitamente, grazie all'operatore `.`).

Se devo scrivere una stringa di testo posso fare questo:
{% highlight php %}
<?php
$scritta = "La mia stringa di testo";
$f = fopen("sunnyoutput.txt", "w");
fwrite($f, $scritta);
fclose($f);
{% endhighlight %}

Se devo scrivere un numero intero, PHP lo converte automaticamente in stringa senza bisogno di un cast esplicito.

{% highlight php %}
<?php
$numero_passi = 5;
$f = fopen("sunnyoutput.txt", "w");
fwrite($f, (string) $numero_passi);
fclose($f);
{% endhighlight %}

In alternativa, per scrivere l'intero contenuto in un colpo solo, esiste la funzione **file_put_contents**, che apre, scrive e chiude il file per noi:

{% highlight php %}
<?php
file_put_contents("sunnyoutput.txt", "La mia stringa di testo");
{% endhighlight %}

## Esercizi

### Esercizio 1:

Crea un file chiamato *numeri.txt* contenente 10 numeri interi. Apri il file, leggi il suo contenuto, raddoppia tutti i numeri e scrivili nel file *raddoppiati.txt*.

Esempio file input numeri.txt:
{% highlight shell %}
3
5
7
{% endhighlight %}

Esempio file output raddoppiati.txt:
{% highlight shell %}
6
10
14
{% endhighlight %}


### Esercizio 2:

Crea un file chiamato *letteremiste.txt* contenente lettere una per ciascuna riga. Apri il file, leggi il suo contenuto e scrivi un file *letterefiltrate.txt* che contenga soltanto le vocali contenute nel primo file.

Esempio file input letteremiste.txt:
{% highlight shell %}
a
b
e
m
{% endhighlight %}

Esempio file output letterefiltrate.txt:
{% highlight shell %}
a
e
{% endhighlight %}

### Esercizio 3:

Crea un file chiamato numeri.txt contenente 10 numeri interi. Apri il file, leggi il suo contenuto e calcola la somma, la media, il massimo e il minimo e scrivi l'output nel file statistiche.txt

Esempio file input numeri.txt:

3
5
7

Esempio file output statistiche.txt:

Somma: 15
Media: 5
Massimo: 7
Minimo: 3

### Esercizio 3:

Crea un file chiamato numeri.txt contenente 10 coppie di numeri interi separati da uno spazio. Apri il file, leggi il suo contenuto e calcola la somma dei numeri riga per riga e scrivi l'output nel file somme.txt

Esempio file input numeri.txt:

3 2
5 1
7 4

Esempio file output somme.txt:

5
6
11
