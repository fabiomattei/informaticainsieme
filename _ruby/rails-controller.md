---
title: 'Ruby on Rails: controller e azioni'
date: '2026-08-27T08:03:00+02:00'
author: Fabio Mattei
layout: page
---

## Il ruolo del controller

Il **controller** è il collegamento tra le rotte e il resto
dell'applicazione: riceve la richiesta HTTP, eventualmente interroga il
model per ottenere o salvare dei dati, e infine decide quale view mostrare
(o dove reindirizzare l'utente).

Ogni metodo pubblico di un controller viene chiamato **azione**.

## Generare un controller

Rails offre un comando per creare velocemente un nuovo controller con alcune
azioni già pronte:

{% highlight bash %}
bin/rails generate controller Articoli index show
{% endhighlight %}

Questo comando crea il file `app/controllers/articoli_controller.rb`, le
relative viste vuote, e aggiunge automaticamente le rotte corrispondenti.

## Anatomia di un controller

{% highlight ruby %}
# app/controllers/articoli_controller.rb
class ArticoliController < ApplicationController

  def index
    @articoli = Articolo.all
  end

  def show
    @articolo = Articolo.find(params[:id])
  end

end
{% endhighlight %}

Ogni controller eredita da `ApplicationController`. Le variabili di istanza
(quelle che iniziano con `@`) definite in una azione sono automaticamente
visibili nella view corrispondente: `@articoli` definita nell'azione `index`
sarà disponibile all'interno di `app/views/articoli/index.html.erb`.

## L'oggetto params

Le informazioni contenute nell'indirizzo o inviate da un form arrivano al
controller tramite l'oggetto **params**, che si comporta come un hash:

{% highlight ruby %}
def show
  # se l'indirizzo è /articoli/5, params[:id] vale "5"
  @articolo = Articolo.find(params[:id])
end
{% endhighlight %}

## render e redirect_to

Al termine di una azione, Rails di norma disegna automaticamente la view con
lo stesso nome dell'azione (convenzione!). È comunque possibile controllare
il comportamento in modo esplicito:

{% highlight ruby %}
class ArticoliController < ApplicationController

  def show
    @articolo = Articolo.find_by(id: params[:id])

    if @articolo.nil?
      redirect_to articoli_path, alert: "Articolo non trovato"
    else
      render :show
    end
  end

end
{% endhighlight %}

* **render** disegna una view (per default, quella con lo stesso nome
  dell'azione);
* **redirect_to** invia il browser verso un altro indirizzo, facendo
  ripartire una nuova richiesta HTTP.

## before_action

Spesso più azioni di uno stesso controller hanno bisogno di eseguire lo
stesso codice preliminare, ad esempio caricare l'articolo indicato
nell'indirizzo. Per evitare di ripetere il codice si usa `before_action`:

{% highlight ruby %}
class ArticoliController < ApplicationController
  before_action :trova_articolo, only: [:show, :edit, :update, :destroy]

  def show
  end

  private

  def trova_articolo
    @articolo = Articolo.find(params[:id])
  end
end
{% endhighlight %}
