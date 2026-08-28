---
title: 'Ruby on Rails: struttura di un progetto'
date: '2026-08-27T08:01:00+02:00'
author: Fabio Mattei
layout: page
---

## Le cartelle generate da rails new

Quando si crea una nuova applicazione con `rails new`, Rails genera una
struttura di cartelle molto precisa. Seguirla è importante perché è proprio
grazie a questa organizzazione che il framework riesce a collegare
automaticamente model, view e controller senza bisogno di configurazione
manuale (il principio "Convention over Configuration" visto nella pagina
precedente).

{% highlight text %}
mio_blog/
├── app/
│   ├── controllers/
│   ├── models/
│   ├── views/
│   ├── helpers/
│   └── assets/
├── bin/
├── config/
│   ├── routes.rb
│   ├── database.yml
│   └── environments/
├── db/
│   ├── migrate/
│   └── schema.rb
├── lib/
├── log/
├── public/
├── test/ (o spec/ se si usa RSpec)
├── tmp/
├── Gemfile
└── Gemfile.lock
{% endhighlight %}

## La cartella app/

È la cartella più importante, quella dove si scrive quasi tutto il codice
della propria applicazione:

* **app/models/**: le classi che rappresentano i dati (tipicamente collegate
  alle tabelle del database tramite ActiveRecord);
* **app/controllers/**: le classi che gestiscono le richieste HTTP;
* **app/views/**: i template HTML (con estensione `.html.erb`) che vengono
  mostrati all'utente;
* **app/helpers/**: metodi di supporto richiamabili dalle viste;
* **app/assets/**: fogli di stile, immagini e file JavaScript.

## La cartella config/

Contiene i file di configurazione dell'applicazione:

* **config/routes.rb**: definisce le rotte, cioè quali indirizzi web
  corrispondono a quali controller (lo vedremo in dettaglio nella prossima
  pagina);
* **config/database.yml**: la configurazione della connessione al database;
* **config/environments/**: impostazioni diverse per ogni ambiente
  (`development`, `test`, `production`).

## La cartella db/

Contiene tutto quello che riguarda il database:

* **db/migrate/**: le migrazioni, cioè i file che descrivono passo dopo passo
  come è cambiata la struttura del database nel tempo;
* **db/schema.rb**: una fotografia dello stato attuale del database, generata
  automaticamente da Rails.

## Il Gemfile

Il file `Gemfile`, posto nella cartella principale, elenca tutte le gem
(librerie Ruby) usate dal progetto. Dopo averlo modificato si esegue:

{% highlight bash %}
bundle install
{% endhighlight %}

per installare o aggiornare le librerie elencate.

## bin/rails: il comando principale

Quasi tutte le operazioni su un progetto Rails passano dal comando
`bin/rails` (o semplicemente `rails`, a seconda di come è installato). Alcuni
esempi utili fin da subito:

{% highlight bash %}
bin/rails server      # avvia il server di sviluppo
bin/rails console     # apre una console interattiva collegata all'applicazione
bin/rails routes      # elenca tutte le rotte definite
bin/rails generate     # genera codice (controller, model, migrazioni...)
{% endhighlight %}
