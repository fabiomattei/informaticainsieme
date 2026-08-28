---
title: 'Ruby on Rails: creare una API in JSON'
date: '2026-08-27T08:17:00+02:00'
author: Fabio Mattei
layout: page
---

## Rails non serve solo per pagine HTML

Finora abbiamo sempre visto controller che rispondono con una pagina HTML
disegnata da una view ERB. Ma Rails può rispondere, con la stessa identica
struttura di model e controller, anche con dati in formato **JSON**: il
formato più usato per far comunicare tra loro applicazioni diverse (ad
esempio un frontend scritto con JavaScript, oppure una app mobile).

## render json

Il modo più semplice per restituire JSON è usare `render json:` al posto di
`render` (o lasciare che Rails la scelga da solo):

{% highlight ruby %}
class ArticoliController < ApplicationController

  def index
    @articoli = Articolo.all
    render json: @articoli
  end

  def show
    @articolo = Articolo.find(params[:id])
    render json: @articolo
  end

end
{% endhighlight %}

ActiveRecord sa convertirsi automaticamente in JSON: chiamando
`render json: @articolo` Rails richiama, dietro le quinte, il metodo
`as_json` che serializza tutti gli attributi del record.

## Rispondere sia in HTML che in JSON

Un controller può gestire entrambi i formati, scegliendo automaticamente in
base a cosa richiede il client (l'estensione nell'indirizzo, oppure
l'header HTTP `Accept`):

{% highlight ruby %}
def show
  @articolo = Articolo.find(params[:id])

  respond_to do |format|
    format.html # disegna app/views/articoli/show.html.erb, come sempre
    format.json { render json: @articolo }
  end
end
{% endhighlight %}

Con questa configurazione, `/articoli/1` restituisce la pagina HTML,
mentre `/articoli/1.json` restituisce lo stesso articolo in formato JSON.

## Gestire gli errori in JSON

{% highlight ruby %}
def create
  @articolo = Articolo.new(articolo_params)

  if @articolo.save
    render json: @articolo, status: :created
  else
    render json: { errori: @articolo.errors.full_messages }, status: :unprocessable_entity
  end
end
{% endhighlight %}

Da notare l'uso dei codici di stato HTTP (`:created` per una risorsa creata
con successo, `:unprocessable_entity` quando la validazione fallisce):
sono parte fondamentale del "contratto" di una API ben progettata.

## Rails in modalità API

Se un progetto è pensato **fin dall'inizio** per essere solo una API (senza
mai disegnare pagine HTML, ad esempio perché il frontend sarà una
applicazione separata in JavaScript), Rails offre una modalità dedicata,
più leggera, da usare già al momento della creazione del progetto:

{% highlight bash %}
rails new mia_api --api
{% endhighlight %}

In questa modalità Rails non genera le cartelle per le viste HTML, non
include i middleware pensati per il browser (come la protezione dai
cookie di sessione) e i generatori (`rails generate scaffold`...) creano
solo controller e model orientati a restituire JSON.
