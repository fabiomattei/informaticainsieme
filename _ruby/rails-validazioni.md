---
title: 'Ruby on Rails: le validazioni'
date: '2026-08-27T08:08:00+02:00'
author: Fabio Mattei
layout: page
---

## Perché servono le validazioni

Un database, da solo, non garantisce che i dati inseriti abbiano senso: non
impedisce, ad esempio, di salvare un articolo senza titolo. Le
**validazioni** sono regole scritte nel model che vengono controllate
automaticamente da ActiveRecord prima di salvare un record, impedendo che
vengano scritti dati non validi.

## Un primo esempio

{% highlight ruby %}
# app/models/articolo.rb
class Articolo < ApplicationRecord
  validates :titolo, presence: true
end
{% endhighlight %}

Con questa validazione, provando a salvare un articolo senza titolo:

{% highlight ruby %}
articolo = Articolo.new(testo: "Testo senza titolo")
articolo.save     # => false, il salvataggio NON avviene
articolo.valid?   # => false
articolo.errors.full_messages  # => ["Titolo non può essere lasciato in bianco"]
{% endhighlight %}

Il metodo `save` restituisce `false` se il record non è valido, invece di
sollevare un errore: per questo è buona norma controllarne sempre il
risultato (lo vedremo bene nella pagina sui form).

## I validatori più comuni

{% highlight ruby %}
class Articolo < ApplicationRecord
  validates :titolo, presence: true, length: { minimum: 3, maximum: 100 }
  validates :testo, presence: true
  validates :slug, uniqueness: true
  validates :voto, numericality: { greater_than_or_equal_to: 0, less_than_or_equal_to: 10 }
  validates :email, format: { with: URI::MailTo::EMAIL_REGEXP }
end
{% endhighlight %}

* **presence**: il campo non può essere vuoto;
* **length**: controlla la lunghezza minima/massima di una stringa;
* **uniqueness**: il valore non deve già esistere in un altro record della
  stessa tabella (utile ad esempio per email o username);
* **numericality**: il valore deve essere un numero, con eventuali limiti;
* **format**: il valore deve rispettare una espressione regolare.

## Validazioni su più campi contemporaneamente

Si può applicare la stessa regola a più campi in una sola riga:

{% highlight ruby %}
validates :titolo, :testo, presence: true
{% endhighlight %}

## Messaggi di errore personalizzati

{% highlight ruby %}
validates :titolo, presence: { message: "è obbligatorio" }
{% endhighlight %}

## Validazioni personalizzate

Quando le regole predefinite non bastano, si può scrivere un metodo di
validazione personalizzato:

{% highlight ruby %}
class Articolo < ApplicationRecord
  validate :titolo_non_deve_contenere_parole_vietate

  private

  def titolo_non_deve_contenere_parole_vietate
    if titolo&.downcase&.include?("spam")
      errors.add(:titolo, "non può contenere la parola 'spam'")
    end
  end
end
{% endhighlight %}

## Perché non basta la validazione lato browser

Anche se in un form HTML si può richiedere che un campo sia obbligatorio
(con l'attributo `required`), la validazione lato server rimane comunque
indispensabile: un utente potrebbe disattivare JavaScript, usare strumenti
che inviano richieste direttamente al server, o semplicemente il browser
potrebbe avere un bug. Le validazioni di ActiveRecord sono l'ultima e più
affidabile linea di difesa contro dati non validi.
