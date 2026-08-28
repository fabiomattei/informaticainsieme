---
title: 'Ruby on Rails: popolare il database con i seed'
date: '2026-08-27T08:11:00+02:00'
author: Fabio Mattei
layout: page
---

## A cosa servono i seed

Durante lo sviluppo è scomodo dover creare a mano, ogni volta, degli
articoli di prova dalla console per poter vedere qualcosa nell'applicazione.
Rails offre un file dedicato, **db/seeds.rb**, pensato apposta per
descrivere dei dati di partenza in modo ripetibile.

## Scrivere i seed

{% highlight ruby %}
# db/seeds.rb
Articolo.create!(
  titolo: "Benvenuti nel blog",
  testo: "Questo è il primo articolo di esempio."
)

Articolo.create!(
  titolo: "Come funzionano le migrazioni",
  testo: "In questo articolo parliamo di ActiveRecord..."
)
{% endhighlight %}

Per eseguire il file e popolare il database:

{% highlight bash %}
bin/rails db:seed
{% endhighlight %}

## Evitare duplicati: find_or_create_by

Il problema di `create!` è che, eseguendo il comando più volte, si
finirebbe per creare gli stessi articoli più volte. Per rendere i seed
**idempotenti** (eseguibili più volte senza effetti indesiderati) si usa
`find_or_create_by`, che crea il record solo se non esiste già uno con
quei valori:

{% highlight ruby %}
Articolo.find_or_create_by!(titolo: "Benvenuti nel blog") do |articolo|
  articolo.testo = "Questo è il primo articolo di esempio."
end
{% endhighlight %}

## Seed più complessi, con associazioni

Ricordando le associazioni viste in precedenza, si possono generare anche
dati collegati tra loro:

{% highlight ruby %}
articolo = Articolo.find_or_create_by!(titolo: "Benvenuti nel blog") do |a|
  a.testo = "Primo post!"
end

articolo.commenti.find_or_create_by!(testo: "Complimenti per il blog!")
articolo.commenti.find_or_create_by!(testo: "Molto interessante.")
{% endhighlight %}

## Ricreare il database da zero

Durante lo sviluppo è comodo, ogni tanto, ripartire da un database pulito
e ripopolato con i seed. Il comando `db:reset` cancella il database, lo
ricrea dalle migrazioni e infine esegue i seed:

{% highlight bash %}
bin/rails db:reset
{% endhighlight %}
