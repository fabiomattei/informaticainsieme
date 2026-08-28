---
title: 'Ruby on Rails: mettere online la applicazione (deploy)'
date: '2026-08-27T08:18:00+02:00'
author: Fabio Mattei
layout: page
---

## Sviluppo vs produzione

Finora abbiamo sempre lavorato in ambiente **development**: il server
integrato di Rails (`bin/rails server`), pensato per essere comodo durante
la scrittura del codice (mostra errori dettagliati, ricarica il codice
automaticamente...) ma non adatto a ricevere visite reali. Per pubblicare
l'applicazione online serve prepararla per l'ambiente **production**, più
lento da avviare ma molto più veloce, sicuro e stabile in esercizio.

## Le variabili d'ambiente e le credenziali

Password del database, chiavi API di servizi esterni, e altre informazioni
sensibili non vanno mai scritte direttamente nel codice (che finisce
tipicamente su un repository git, magari pubblico). Rails offre un sistema
di **credenziali cifrate**:

{% highlight bash %}
bin/rails credentials:edit
{% endhighlight %}

Questo comando apre un file cifrato (`config/credentials.yml.enc`) dove
salvare i valori segreti; la chiave per decifrarlo (`config/master.key`)
**non va mai messa su git** (di norma è già esclusa in `.gitignore` di
default).

## Precompilare gli assets

I fogli di stile e i file JavaScript, in produzione, vengono uniti,
minificati e "impacchettati" con un nome che include un codice univoco
(utile per la cache del browser). Questa operazione si chiama
**precompilazione**:

{% highlight bash %}
RAILS_ENV=production bin/rails assets:precompile
{% endhighlight %}

Nella maggior parte delle piattaforme di hosting questo passaggio avviene
automaticamente durante il deploy, senza doverlo lanciare a mano.

## Il database in produzione

Il file `config/database.yml` contiene una configurazione separata per
l'ambiente `production`, che tipicamente legge i dati di connessione da
variabili d'ambiente invece che scriverli in chiaro:

{% highlight yaml %}
production:
  adapter: postgresql
  url: <%= ENV["DATABASE_URL"] %>
{% endhighlight %}

Al primo avvio in produzione (e ogni volta che si aggiungono nuove
migrazioni) va eseguito:

{% highlight bash %}
RAILS_ENV=production bin/rails db:migrate
{% endhighlight %}

## Dove pubblicare l'applicazione

Esistono diverse strade per mettere online una applicazione Rails, dalla
più semplice alla più flessibile:

* **piattaforme "a consegna" (PaaS)**, come Render o Heroku: basta
  collegare il repository git, la piattaforma si occupa di build, database
  e certificati HTTPS;
* **Kamal**, lo strumento di deploy incluso di default nelle applicazioni
  Rails recenti, pensato per pubblicare l'app come container Docker su un
  proprio server (anche un semplice VPS), con un solo comando
  (`kamal deploy`);
* **server tradizionale**, configurando a mano un server delle
  applicazioni (Puma, incluso di default) dietro un reverse proxy come
  Nginx: la strada più flessibile ma anche quella che richiede più
  conoscenze di amministrazione di sistema.

Per i primi progetti personali, una piattaforma PaaS è di gran lunga la
scelta più semplice per vedere online, in pochi minuti, la propria
applicazione.
