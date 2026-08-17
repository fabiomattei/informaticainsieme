---
title: 'Comandi vari utili'
date: '2026-07-26T10:20:00+02:00'
author: Fabio Mattei
layout: page
---

![Finestra del Prompt personalizzata con titolo, colori e cronologia comandi](/images/cmd/comandi-vari/comandi-vari.svg){:class="aside-image"}

In questa pagina trovi alcuni comandi che non rientrano nelle categorie viste finora, ma che sono comunque comodi da conoscere: permettono di personalizzare la finestra del Prompt, gestire i file "nascosti" e velocizzare la scrittura dei comandi.

## title - cambia il titolo della finestra

{% highlight shell %}
title Il mio Prompt
{% endhighlight %}

Cambia il testo che compare in cima alla finestra del Prompt dei comandi. Utile soprattutto dentro uno script batch, per capire subito quale finestra è quella giusta se ne hai aperte diverse.

## color - cambia i colori della finestra

{% highlight shell %}
color 0A
{% endhighlight %}

Cambia il colore dello sfondo e del testo. Il comando vuole due caratteri: il primo per lo sfondo, il secondo per il testo. Ogni carattere può essere una cifra da `0` a `9` o una lettera da `A` a `F` e rappresenta un colore (per esempio `0` è nero, `A` è verde chiaro, `F` è bianco brillante).

{% highlight shell %}
color        senza parametri, riporta i colori a quelli di default
{% endhighlight %}

## start - avvia un programma, un file o un sito web

{% highlight shell %}
start notepad.exe
{% endhighlight %}

Apre il Blocco note in una finestra separata, senza bloccare il Prompt (cioè si può continuare a digitare altri comandi subito, senza dover chiudere prima il programma appena aperto).

Si può usare anche per aprire un file con il programma associato, oppure direttamente un sito web:

{% highlight shell %}
start appunti.txt
start https://www.informaticainsieme.it
{% endhighlight %}

## exit - chiude il Prompt (o uno script)

{% highlight shell %}
exit
{% endhighlight %}

Chiude la finestra del Prompt dei comandi. Se usato dentro uno script batch, `exit` interrompe subito lo script (a meno di usare l'opzione `/b`, che invece esce solo dallo script senza chiudere anche la finestra del Prompt che lo aveva lanciato):

{% highlight shell %}
exit /b
{% endhighlight %}

## attrib - vedi e cambia gli attributi di un file

Ogni file, oltre al contenuto, ha alcuni "attributi" che ne descrivono lo stato, ad esempio se è nascosto o se è di sola lettura. Il comando `attrib`, usato senza parametri, mostra questi attributi:

{% highlight shell %}
attrib appunti.txt
{% endhighlight %}

Per aggiungere o togliere un attributo si usa `+` (aggiungi) o `-` (togli), seguito da una lettera:

{% highlight shell %}
attrib +h appunti.txt      nasconde il file (h = hidden)
attrib -h appunti.txt      lo rende di nuovo visibile
attrib +r appunti.txt      lo rende di sola lettura (r = read-only), non si può più modificare né cancellare
attrib -r appunti.txt      toglie la sola lettura
{% endhighlight %}

## doskey - ritrovare i comandi già digitati

Mentre si usa il Prompt, i tasti freccia **su** e **giù** permettono di scorrere tra i comandi digitati in precedenza, senza doverli riscrivere da capo. Premendo `F7` si apre invece un piccolo elenco con la cronologia di tutti i comandi usati in quella sessione, tra cui scegliere con le frecce.

Il comando `doskey` permette di vedere questa cronologia anche in forma di semplice elenco testuale:

{% highlight shell %}
doskey /history
{% endhighlight %}

`doskey` serve anche per creare delle "scorciatoie" personali, chiamate macro: un nome breve che, quando digitato, esegue al posto tuo un comando (magari lungo o che usi spesso).

{% highlight shell %}
doskey elenco=dir /w
{% endhighlight %}

Da questo momento, digitando semplicemente `elenco`, verrà eseguito `dir /w`. Attenzione: queste macro valgono solo per la sessione corrente del Prompt e vanno perse chiudendo la finestra.
