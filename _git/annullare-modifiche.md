---
title: 'Annullare le modifiche: restore, reset, revert, stash'
date: '2026-08-17T10:15:00+02:00'
author: Fabio Mattei
layout: page
---

![Quale comando usare per annullare una modifica: restore, reset, revert o stash](/images/git/annullare-modifiche/annullare-modifiche.svg){:class="aside-image"}

Uno dei motivi principali per cui si usa Git è la possibilità di **tornare indietro**. Esistono però comandi diversi a seconda di cosa, esattamente, si vuole annullare.

## git restore - annullare modifiche non ancora confermate

Per riportare un file modificato nella working directory allo stato dell'ultimo commit, scartando le modifiche non salvate:

{% highlight shell %}
git restore nomefile.py
{% endhighlight %}

Se invece un file è già stato aggiunto alla staging area (con `git add`) e si vuole toglierlo da lì, senza però perdere le modifiche fatte (che restano nella working directory):

{% highlight shell %}
git restore --staged nomefile.py
{% endhighlight %}

## git reset - spostare indietro la cronologia

`git reset` sposta il branch corrente (e il puntatore HEAD) a un commit precedente. Esistono tre modalità, a seconda di cosa si vuole fare delle modifiche successive a quel commit:

{% highlight shell %}
git reset --soft HEAD~1
{% endhighlight %}

Annulla l'ultimo commit, ma mantiene tutte le modifiche in staging area: utile per "disfare" un commit fatto per sbaglio e correggerlo.

{% highlight shell %}
git reset --mixed HEAD~1
{% endhighlight %}

È il comportamento predefinito (equivalente a scrivere solo `git reset HEAD~1`): annulla l'ultimo commit e toglie le modifiche dalla staging area, ma le lascia nella working directory.

{% highlight shell %}
git reset --hard HEAD~1
{% endhighlight %}

Annulla l'ultimo commit ed **elimina definitivamente** anche le modifiche corrispondenti dalla working directory. È un comando distruttivo: le modifiche scartate non sono recuperabili con i normali comandi Git, quindi va usato con cautela.

`HEAD~1` indica "un commit prima di HEAD"; si può usare `HEAD~2`, `HEAD~3`... per tornare più indietro, oppure indicare direttamente l'hash di un commit specifico.

![Le tre modalità di git reset e dove restano le modifiche dopo aver annullato l'ultimo commit](/images/git/annullare-modifiche/reset-modes.svg){:class="half-image"}

## git revert - annullare un commit creandone uno nuovo

A differenza di `reset`, che riscrive la cronologia spostando indietro il branch, `git revert` **non elimina** il commit da annullare: crea un nuovo commit che applica le modifiche opposte a quelle del commit indicato.

{% highlight shell %}
git revert a3f5c9e
{% endhighlight %}

Questo approccio è più sicuro su un branch condiviso con altre persone (ad esempio `main`), perché non modifica commit già esistenti né la cronologia già scaricata da altri: si limita ad aggiungerne uno nuovo che "corregge il tiro".

## git stash - mettere da parte le modifiche temporaneamente

Capita di avere modifiche in corso, non ancora pronte per un commit, e di dover improvvisamente cambiare branch (ad esempio per correggere un bug urgente). `git stash` mette da parte tutte le modifiche non confermate, riportando la working directory allo stato dell'ultimo commit, senza però perderle:

{% highlight shell %}
git stash
{% endhighlight %}

Per vedere l'elenco delle modifiche messe da parte:

{% highlight shell %}
git stash list
{% endhighlight %}

Per far tornare l'ultima modifica messa da parte nella working directory:

{% highlight shell %}
git stash pop
{% endhighlight %}

`git stash pop` riapplica le modifiche e le rimuove dall'elenco dello stash; se invece si vuole riapplicarle mantenendole comunque nell'elenco (per poterle riusare altrove), si usa `git stash apply`.

## Quale comando usare?

* Modifiche **non ancora salvate** in un file → `git restore`
* Vuoi **cambiare branch temporaneamente** senza fare commit → `git stash`
* Vuoi annullare un commit **non ancora condiviso** con altri → `git reset`
* Vuoi annullare un commit **già condiviso** (già inviato al remote) → `git revert`
