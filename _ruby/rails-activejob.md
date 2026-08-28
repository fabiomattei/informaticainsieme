---
title: 'Ruby on Rails: job in background con ActiveJob'
date: '2026-08-27T08:16:00+02:00'
author: Fabio Mattei
layout: page
---

## Perché serve un job in background

Alcune operazioni (inviare una email, generare un file pesante, chiamare un
servizio esterno lento) possono richiedere diversi secondi. Se venissero
eseguite direttamente dentro una azione del controller, l'utente resterebbe
con il browser bloccato in attesa fino al termine. **ActiveJob** permette di
"mettere in coda" queste operazioni ed eseguirle in un secondo momento,
lasciando l'applicazione libera di rispondere subito.

## Generare un job

{% highlight bash %}
bin/rails generate job invia_notifica
{% endhighlight %}

{% highlight ruby %}
# app/jobs/invia_notifica_job.rb
class InviaNotificaJob < ApplicationJob
  queue_as :default

  def perform(commento)
    NotificaMailer.nuovo_commento(commento).deliver_now
  end
end
{% endhighlight %}

Il metodo `perform` contiene il codice che verrà effettivamente eseguito;
può accettare qualunque parametro, purché serializzabile (un record
ActiveRecord, una stringa, un numero...).

## Mettere in coda un job

{% highlight ruby %}
InviaNotificaJob.perform_later(@commento)
{% endhighlight %}

* **perform_later**: accoda il job per l'esecuzione in background (è
  esattamente quello che fa `deliver_later` visto nella pagina precedente,
  che internamente usa proprio ActiveJob);
* **perform_now**: esegue il job subito, in modo sincrono (utile soprattutto
  nei test).

## Gli adapter

ActiveJob è solo una interfaccia comune: chi esegue davvero i job in coda è
un **adapter** esterno, configurabile in `config/application.rb` o nei file
di ambiente:

{% highlight ruby %}
config.active_job.queue_adapter = :async
{% endhighlight %}

* **:async**: adapter minimo, integrato in Rails, che esegue i job in un
  thread separato dello stesso processo. Comodo per iniziare, ma i job
  vengono persi se il server si riavvia;
* **:solid_queue**, **:sidekiq**, **:good_job**: adapter più robusti, pensati
  per la produzione, che salvano i job (tipicamente su database o Redis) in
  modo che sopravvivano ai riavvii e possano essere eseguiti da processi
  worker dedicati.

## Job programmati nel tempo

Un job può anche essere accodato per essere eseguito non subito, ma dopo un
certo intervallo, o ad un orario preciso:

{% highlight ruby %}
InviaNotificaJob.set(wait: 10.minutes).perform_later(@commento)
InviaNotificaJob.set(wait_until: Date.tomorrow.noon).perform_later(@commento)
{% endhighlight %}
