---
title: Caricamento dei dati da file e scrittura dati su file in python
date: '2020-01-10T10:24:27+01:00'
author: Fabio Mattei
layout: page
---

## Caricare i dati da file

Fino ad oggi abbiamo ricevuto dati dall'utente utilizzando la funzione **input()** ma la console non rappresenta l'unico canale di input possibile, in effetti è possibile ricevere in input informazioni da un file di testo. Un file di testo altro non è che una sequenza di simboli ASCII, riconosciamo questi file dall'estensione del file **.txt**.

Il file viene letto attraverso la chiamata alla funzione open che prende come parametri il nome del file il tipo di operazione che vogliamo fare sul file:

* r apre un file per la lettura (default)
* w apre un file per la scrittura. Se il file non esiste ne crea uno nuovo se il file esiste lo sovrascrive.
* x apre un file limitatamente per la creazione. Se il file esiste già l'operazione fallisce.
* a apre un file per la scrittura aggiungendo informazioni alla fine senza sovrascrivere le informazioni già presenti. Se il file non esiste lo crea.
* t apre il file in modealità testuale. (default)		
* b apre il file in modealità binaria.		
* + Apre un file simultaneamente per la lettura e per la scrittura.

{% highlight shell %}
f = open("ilmiofilediinput.txt", "r")  # apre in lettura il file ilmiofilediinput.txt
linee = f.readlines()                  # ottengo una lista di stringhe in cui ogni strina è una linea del file di testo
f.close()                              # chiude il file appena aperto
{% endhighlight %}


{% highlight shell %}

{% endhighlight %}


## Scrivere i dati su file

Se sono nella necessità di scrivere dati su di un file posso utilizzare la funzione open passando il parametro **w** per indicare che ho bisogno di scrivere sul file.
Andrò a scrivere nel file utilizzando il metodo **write** del **file handler**, ma nota bene la funzione write accetta soltanto stringhe di testo quindi eventuali numeri vanno convertiti in stringa attraverso la funzione str.

{% highlight shell %}
f = open("sunnyoutput.txt", "w")
f.write(str(numero_passi))
f.close()
{% endhighlight %}

## Esercizi 

#### Esercizio 1:

