---
title: 'Ruby on Rails: caricare file con ActiveStorage'
date: '2026-08-27T08:14:00+02:00'
author: Fabio Mattei
layout: page
---

## Cos'è ActiveStorage

**ActiveStorage** è la libreria integrata in Rails per gestire il caricamento
di file (immagini, PDF, allegati...) e collegarli ai model, senza dover
scrivere a mano la gestione dei percorsi sul disco o su un servizio cloud.

## Installazione

ActiveStorage crea due tabelle di appoggio nel database (per tenere traccia
dei file caricati e dei collegamenti ai record):

{% highlight bash %}
bin/rails active_storage:install
bin/rails db:migrate
{% endhighlight %}

## Collegare un file ad un model

Per permettere ad un articolo di avere una immagine di copertina, basta una
riga nel model:

{% highlight ruby %}
# app/models/articolo.rb
class Articolo < ApplicationRecord
  has_one_attached :copertina
end
{% endhighlight %}

Se invece un record può avere più file collegati (ad esempio più foto in una
galleria), si usa `has_many_attached`:

{% highlight ruby %}
class Articolo < ApplicationRecord
  has_many_attached :immagini
end
{% endhighlight %}

## Il campo file nel form

{% highlight erb %}
<%= form_with model: articolo do |form| %>
  <%= form.label :titolo %>
  <%= form.text_field :titolo %>

  <%= form.label :copertina %>
  <%= form.file_field :copertina %>

  <%= form.submit %>
<% end %>
{% endhighlight %}

Perché il file venga effettivamente inviato al server, ricordarsi di
permettere il campo tra gli strong parameters:

{% highlight ruby %}
def articolo_params
  params.require(:articolo).permit(:titolo, :testo, :copertina)
end
{% endhighlight %}

## Mostrare il file caricato

{% highlight erb %}
<% if articolo.copertina.attached? %>
  <%= image_tag articolo.copertina %>
<% end %>
{% endhighlight %}

Il metodo `.attached?` verifica che sia stato effettivamente caricato un
file, evitando errori quando un articolo non ha ancora una copertina.

## Dove vengono salvati i file

Per default, in sviluppo, ActiveStorage salva i file sul disco locale
(dentro `storage/`). Il file di configurazione `config/storage.yml` permette
di collegare, in produzione, un servizio cloud come Amazon S3, Google Cloud
Storage o simili, cambiando solo la configurazione e senza dover riscrivere
il codice dell'applicazione: un ottimo esempio del principio "Convention
over Configuration" applicato anche all'archiviazione dei file.
