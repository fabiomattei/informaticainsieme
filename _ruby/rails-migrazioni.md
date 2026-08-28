---
title: 'Ruby on Rails: migrazioni e database'
date: '2026-08-27T08:06:00+02:00'
author: Fabio Mattei
layout: page
---

## Cosa sono le migrazioni

Le **migrazioni** sono il modo in cui Rails gestisce le modifiche alla
struttura del database (creare tabelle, aggiungere colonne, eccetera) in
modo ordinato e tracciabile nel tempo, un po' come fa git con il codice
sorgente. Ogni migrazione è un file Ruby con un nome che inizia con un
timestamp, salvato in `db/migrate/`.

## Generare un model con la sua migrazione

Il modo più comodo per iniziare è generare model e migrazione insieme:

{% highlight bash %}
bin/rails generate model Articolo titolo:string testo:text pubblicato:boolean
{% endhighlight %}

Questo comando crea sia il file `app/models/articolo.rb` sia una migrazione
in `db/migrate/`, già pronta con le colonne indicate:

{% highlight ruby %}
# db/migrate/20260827080600_create_articoli.rb
class CreateArticoli < ActiveRecord::Migration[7.1]
  def change
    create_table :articoli do |t|
      t.string :titolo
      t.text :testo
      t.boolean :pubblicato, default: false

      t.timestamps
    end
  end
end
{% endhighlight %}

Il metodo `t.timestamps` aggiunge automaticamente due colonne, `created_at`
e `updated_at`, gestite in automatico da Rails.

## Eseguire le migrazioni

Scrivere una migrazione non modifica ancora il database: per applicarla
davvero serve il comando:

{% highlight bash %}
bin/rails db:migrate
{% endhighlight %}

Rails tiene traccia di quali migrazioni sono già state eseguite, così anche
se si esegue il comando più volte non verranno applicate due volte le stesse
modifiche.

## Aggiungere una colonna in un secondo momento

Se il database esiste già e serve aggiungere una nuova colonna, si genera
una nuova migrazione dedicata:

{% highlight bash %}
bin/rails generate migration AggiungiAutoreAdArticoli autore:string
{% endhighlight %}

{% highlight ruby %}
class AggiungiAutoreAdArticoli < ActiveRecord::Migration[7.1]
  def change
    add_column :articoli, :autore, :string
  end
end
{% endhighlight %}

E poi, come sempre:

{% highlight bash %}
bin/rails db:migrate
{% endhighlight %}

## schema.rb

Dopo ogni migrazione, Rails aggiorna automaticamente il file
`db/schema.rb`: una fotografia sempre aggiornata della struttura completa
del database. Non va modificato a mano: viene rigenerato automaticamente
dalle migrazioni.

## Annullare una migrazione

Se ci si accorge di un errore nell'ultima migrazione eseguita, la si può
annullare con:

{% highlight bash %}
bin/rails db:rollback
{% endhighlight %}

A questo punto si può correggere il file della migrazione e rieseguire
`bin/rails db:migrate`.
