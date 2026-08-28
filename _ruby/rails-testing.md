---
title: 'Ruby on Rails: i test automatici'
date: '2026-08-27T08:13:00+02:00'
author: Fabio Mattei
layout: page
---

## Perché scrivere test

Un test automatico è un programma che verifica che un altro programma
funzioni come previsto. Scrivendo dei test si può modificare il codice in
futuro (aggiungere una funzionalità, sistemare un bug) con la tranquillità
di accorgersi subito se qualcosa che funzionava prima ha smesso di
funzionare.

Rails include di serie **Minitest** e genera automaticamente dei file di
test vuoti ogni volta che si usa `rails generate model` o
`rails generate controller`, dentro la cartella `test/`.

## Test di un model

I test sul model verificano soprattutto le validazioni e la logica scritta
a mano:

{% highlight ruby %}
# test/models/articolo_test.rb
require "test_helper"

class ArticoloTest < ActiveSupport::TestCase

  test "un articolo senza titolo non è valido" do
    articolo = Articolo.new(testo: "Testo senza titolo")
    assert_not articolo.valid?
  end

  test "un articolo con titolo e testo è valido" do
    articolo = Articolo.new(titolo: "Titolo", testo: "Testo")
    assert articolo.valid?
  end

end
{% endhighlight %}

## Le fixture: dati di prova

Per non dover creare a mano i dati necessari in ogni test, Rails usa le
**fixture**: file YAML che descrivono record di prova, caricati
automaticamente nel database di test prima di ogni esecuzione.

{% highlight yaml %}
# test/fixtures/articoli.yml
primo:
  titolo: "Primo articolo"
  testo: "Contenuto di prova"

secondo:
  titolo: "Secondo articolo"
  testo: "Altro contenuto"
{% endhighlight %}

Nei test, ogni riga della fixture è disponibile con il proprio nome:

{% highlight ruby %}
test "trova l'articolo dalla fixture" do
  articolo = articoli(:primo)
  assert_equal "Primo articolo", articolo.titolo
end
{% endhighlight %}

## Test di un controller (integration test)

Questi test simulano una vera richiesta HTTP, verificando la risposta del
server:

{% highlight ruby %}
# test/controllers/articoli_controller_test.rb
require "test_helper"

class ArticoliControllerTest < ActionDispatch::IntegrationTest

  test "la pagina index risponde con successo" do
    get articoli_path
    assert_response :success
  end

  test "creare un articolo valido reindirizza alla pagina show" do
    assert_difference("Articolo.count", 1) do
      post articoli_path, params: { articolo: { titolo: "Nuovo", testo: "Testo" } }
    end
    assert_redirected_to articolo_path(Articolo.last)
  end

  test "creare un articolo non valido non salva nulla" do
    assert_no_difference("Articolo.count") do
      post articoli_path, params: { articolo: { titolo: "" } }
    end
  end

end
{% endhighlight %}

## Eseguire i test

{% highlight bash %}
bin/rails test                          # esegue tutti i test
bin/rails test test/models/articolo_test.rb   # solo un file
{% endhighlight %}

Rails prepara automaticamente un database separato per i test
(`config/database.yml`, ambiente `test`), così eseguire i test non tocca
mai i dati di sviluppo o di produzione.
