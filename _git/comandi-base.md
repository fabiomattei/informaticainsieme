---
title: 'I comandi di base: status, add, commit, diff, log'
date: '2026-08-17T09:45:00+02:00'
author: Fabio Mattei
layout: page
---

![Il ciclo quotidiano di Git: da file modificati a staging area a cronologia](/images/git/comandi-base/comandi-base.svg){:class="aside-image"}

Questi sono i comandi che si usano più spesso, giorno per giorno, lavorando con Git.

## git status - vedere lo stato del repository

Mostra quali file sono stati modificati, quali sono nuovi (non ancora tracciati da Git) e quali sono già pronti per il prossimo commit (cioè già in staging area):

{% highlight shell %}
git status
{% endhighlight %}

È il comando più usato in assoluto: conviene digitarlo spesso, per capire sempre in che situazione ci si trova prima di procedere.

## git add - aggiungere alla staging area

Per spostare le modifiche di un file dalla working directory alla staging area, cioè "segnarle" come pronte per il prossimo commit:

{% highlight shell %}
git add nomefile.py
{% endhighlight %}

Per aggiungere più file contemporaneamente:

{% highlight shell %}
git add file1.py file2.py
{% endhighlight %}

Per aggiungere **tutti** i file modificati o nuovi in un colpo solo:

{% highlight shell %}
git add .
{% endhighlight %}

## git commit - creare un commit

Una volta che la staging area contiene le modifiche desiderate, si crea un commit con:

{% highlight shell %}
git commit -m "Aggiunta la validazione del form di login"
{% endhighlight %}

L'opzione `-m` permette di scrivere il messaggio del commit direttamente sulla riga di comando. Il messaggio dovrebbe descrivere **cosa** è cambiato e, se utile, **perché**: sarà fondamentale per capire la cronologia del progetto mesi (o anni) dopo.

Una scorciatoia comoda, quando si vogliono aggiungere e confermare in un colpo solo tutte le modifiche a file **già tracciati** da Git (non funziona per i file nuovi):

{% highlight shell %}
git commit -am "Corretto il calcolo del totale"
{% endhighlight %}

## git diff - vedere le differenze

Per vedere nel dettaglio, riga per riga, cosa è cambiato nei file rispetto all'ultimo commit:

{% highlight shell %}
git diff
{% endhighlight %}

Le righe rimosse sono precedute da `-`, quelle aggiunte da `+`. Per vedere invece le differenze delle modifiche già messe in staging area (quindi già "aggiunte" con `git add`):

{% highlight shell %}
git diff --staged
{% endhighlight %}

![git diff confronta working directory e staging, git diff --staged confronta staging e ultimo commit](/images/git/comandi-base/diff.svg){:class="half-image"}

## git log - vedere la cronologia

Mostra l'elenco dei commit, dal più recente al più vecchio, con autore, data e messaggio:

{% highlight shell %}
git log
{% endhighlight %}

Una versione più compatta, con un commit per riga, molto usata per avere una visione d'insieme veloce:

{% highlight shell %}
git log --oneline
{% endhighlight %}

Per vedere anche una rappresentazione grafica dei branch e dei merge:

{% highlight shell %}
git log --oneline --graph --all
{% endhighlight %}

## Un flusso di lavoro tipico

Mettendo insieme questi comandi, il ciclo quotidiano di lavoro con Git è quasi sempre lo stesso:

{% highlight shell %}
git status                          # controllo cosa è cambiato
git add nomefile.py                 # metto in staging le modifiche
git status                          # ricontrollo
git commit -m "Descrizione chiara"  # confermo il commit
git log --oneline                   # verifico la cronologia
{% endhighlight %}
