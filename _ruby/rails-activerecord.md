---
title: 'Ruby on Rails: ActiveRecord e i modelli'
date: '2026-08-27T08:05:00+02:00'
author: Fabio Mattei
layout: page
---

## Cos'è ActiveRecord

**ActiveRecord** è lo strato di Rails che si occupa di collegare gli oggetti
Ruby (i **model**) alle tabelle di un database relazionale. È un **ORM**
(Object-Relational Mapping): grazie ad esso non serve quasi mai scrivere SQL
a mano, perché ogni tabella del database corrisponde ad una classe Ruby, e
ogni riga della tabella corrisponde ad un oggetto di quella classe.

## Convenzioni tra classe e tabella

ActiveRecord collega automaticamente classe e tabella seguendo una
convenzione sui nomi:

* la classe si scrive al **singolare** e con l'iniziale maiuscola: `Articolo`;
* la tabella corrispondente si chiama al **plurale** e minuscola: `articoli`.

{% highlight ruby %}
# app/models/articolo.rb
class Articolo < ApplicationRecord
end
{% endhighlight %}

Da notare che, a differenza delle classi Ruby "normali", un model
ActiveRecord non ha bisogno che vengano dichiarati gli attributi: vengono
dedotti automaticamente dalle colonne della tabella del database (che
creeremo con le migrazioni, nella prossima pagina).

## La console Rails

Per provare i comandi di ActiveRecord senza dover creare viste o controller,
si può usare la **console**, una sessione Ruby interattiva già collegata
all'applicazione e al database:

{% highlight bash %}
bin/rails console
{% endhighlight %}

## Creare record

{% highlight ruby %}
Articolo.create(titolo: "Il mio primo post", testo: "Ciao a tutti!")

# oppure, in due passaggi
articolo = Articolo.new(titolo: "Secondo post", testo: "...")
articolo.save
{% endhighlight %}

## Leggere record

{% highlight ruby %}
Articolo.all                          # tutti gli articoli
Articolo.find(3)                      # l'articolo con id 3 (errore se non esiste)
Articolo.find_by(titolo: "Ciao")      # il primo che soddisfa la condizione (nil se non esiste)
Articolo.where(pubblicato: true)      # tutti quelli con pubblicato = true
Articolo.order(:titolo)               # ordinati per titolo
Articolo.first
Articolo.last
Articolo.count
{% endhighlight %}

## Modificare record

{% highlight ruby %}
articolo = Articolo.find(3)
articolo.titolo = "Titolo modificato"
articolo.save

# oppure, in un solo passaggio
articolo.update(titolo: "Titolo modificato")
{% endhighlight %}

## Cancellare record

{% highlight ruby %}
articolo = Articolo.find(3)
articolo.destroy
{% endhighlight %}

## Combinare le condizioni

I metodi di ActiveRecord si possono concatenare tra loro, perché ognuno
restituisce a sua volta una collezione su cui si può continuare ad
interrogare il database:

{% highlight ruby %}
Articolo.where(pubblicato: true).order(created_at: :desc).limit(5)
{% endhighlight %}
