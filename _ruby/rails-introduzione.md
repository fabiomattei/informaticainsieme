---
title: 'Ruby on Rails: introduzione'
date: '2026-08-27T08:00:00+02:00'
author: Fabio Mattei
layout: page
---

## Cos'è Ruby on Rails

**Ruby on Rails** (spesso chiamato semplicemente **Rails**) è un framework per lo
sviluppo di applicazioni web, scritto in linguaggio Ruby. È nato nel 2004 ed è
famoso per aver reso popolari due principi che semplificano moltissimo la vita
di chi programma:

* **Convention over Configuration** (convenzione sulla configurazione): se si
  seguono le convenzioni di Rails (nomi di file, di cartelle, di classi...) non
  serve scrivere quasi nessuna riga di configurazione, perché il framework
  "indovina" da solo come collegare le varie parti dell'applicazione.
* **DRY - Don't Repeat Yourself** (non ripeterti): ogni informazione deve
  essere definita una sola volta nel codice, evitando duplicazioni.

Rails è un framework di tipo **MVC** (Model - View - Controller), un modo di
organizzare il codice che separa:

* il **Model**: i dati e la logica per accedervi (tipicamente collegato ad un
  database);
* la **View**: quello che l'utente vede, ovvero le pagine HTML;
* il **Controller**: la logica che riceve le richieste del browser, chiede i
  dati al model e sceglie quale view mostrare.

## Installazione

Per usare Rails serve prima di tutto Ruby installato sul proprio computer.
Rails stesso è distribuito come **gem**, cioè come libreria Ruby installabile
con il gestore di pacchetti `gem`.

{% highlight bash %}
gem install rails
{% endhighlight %}

Per verificare che l'installazione sia andata a buon fine:

{% highlight bash %}
rails --version
{% endhighlight %}

## Creare la prima applicazione

Per creare un nuovo progetto si usa il comando `rails new` seguito dal nome
che si vuole dare all'applicazione:

{% highlight bash %}
rails new mio_blog
cd mio_blog
{% endhighlight %}

Questo comando genera automaticamente tutte le cartelle e i file necessari
per far partire una applicazione web completa (li vedremo nel dettaglio nella
prossima pagina).

## Avviare il server

Rails include un server web integrato, pensato per lo sviluppo. Per avviarlo:

{% highlight bash %}
bin/rails server
# oppure, in forma abbreviata
bin/rails s
{% endhighlight %}

A questo punto, aprendo il browser all'indirizzo
[http://localhost:3000](http://localhost:3000) si vedrà la pagina di
benvenuto di Rails: l'applicazione è online e pronta per essere sviluppata.

## Cosa impareremo

In questo piccolo manuale, composto da dieci pagine, vedremo passo dopo passo
come costruire una applicazione Rails completa: la struttura delle cartelle,
il routing, i controller, le viste, i modelli con ActiveRecord, le migrazioni,
le associazioni tra tabelle, le validazioni e, per finire, come realizzare un
CRUD (Create, Read, Update, Delete) completo con i form.
