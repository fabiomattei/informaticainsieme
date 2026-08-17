---
title: 'Lavorare con i repository remoti: remote, push, pull, fetch'
date: '2026-08-17T10:30:00+02:00'
author: Fabio Mattei
layout: page
---

![Repository locale e remoto collegati da push e pull](/images/git/comandi-remoti/comandi-remoti.svg){:class="aside-image"}

Finora abbiamo visto comandi che riguardano solo il repository locale. Questa pagina spiega come sincronizzarsi con un repository remoto, ad esempio uno ospitato su GitHub.

## git remote - gestire i collegamenti remoti

Per vedere quali remote sono collegati al repository corrente:

{% highlight shell %}
git remote -v
{% endhighlight %}

L'opzione `-v` (*verbose*) mostra anche gli indirizzi. Il nome convenzionale del remote principale è `origin`, assegnato automaticamente quando si clona un repository.

Per collegare manualmente un remote a un repository creato in locale con `git init`:

{% highlight shell %}
git remote add origin https://github.com/utente/nome-progetto.git
{% endhighlight %}

## git push - inviare i commit al remote

Per inviare i commit fatti in locale al repository remoto:

{% highlight shell %}
git push origin main
{% endhighlight %}

`origin` è il nome del remote, `main` è il nome del branch da inviare. La prima volta che si invia un nuovo branch conviene usare l'opzione `-u`, che lo collega (*upstream*) al branch remoto corrispondente:

{% highlight shell %}
git push -u origin main
{% endhighlight %}

Da quel momento in poi, sarà sufficiente scrivere semplicemente:

{% highlight shell %}
git push
{% endhighlight %}

perché Git ricorda già a quale remote e a quale branch inviare i commit.

## git fetch - scaricare senza applicare

`git fetch` scarica dal remote i nuovi commit e branch, ma **non li integra** nel proprio lavoro: si limita ad aggiornare lo stato che Git conosce del remote, permettendo di vedere cosa è cambiato prima di deciderne l'integrazione.

{% highlight shell %}
git fetch origin
{% endhighlight %}

## git pull - scaricare e integrare

`git pull` è, in pratica, l'unione di due comandi: prima esegue un `fetch` per scaricare i nuovi commit, poi un `merge` per integrarli automaticamente nel branch corrente.

{% highlight shell %}
git pull origin main
{% endhighlight %}

Se si è già collegati al branch remoto (ad esempio dopo un `push -u`), è sufficiente:

{% highlight shell %}
git pull
{% endhighlight %}

È buona abitudine eseguire `git pull` **prima** di iniziare a lavorare, per assicurarsi di partire dalla versione più aggiornata del progetto, evitando così di trovarsi con conflitti evitabili più avanti.

![git fetch si ferma al branch di tracciamento origin/main, git pull prosegue fino al merge sul branch locale](/images/git/comandi-remoti/fetch-pull.svg){:class="half-image"}

## Un flusso di lavoro tipico

Un ciclo di lavoro completo con un repository remoto assomiglia a questo:

{% highlight shell %}
git pull                              # mi aggiorno con le modifiche altrui
git switch -c nuova-funzionalita      # creo un branch per il mio lavoro
# ... modifico i file ...
git add .
git commit -m "Aggiunta la ricerca per data"
git push -u origin nuova-funzionalita # invio il branch al remote
{% endhighlight %}

A questo punto, il branch è disponibile su GitHub e pronto per essere trasformato in una **pull request**, come vedremo nell'ultima parte di questa sezione dedicata all'interfaccia web di GitHub.
