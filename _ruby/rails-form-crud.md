---
title: 'Ruby on Rails: form e CRUD completo'
date: '2026-08-27T08:09:00+02:00'
author: Fabio Mattei
layout: page
---

## Il CRUD

In questa ultima pagina del manuale mettiamo insieme tutto quello visto
finora (routing, controller, viste, ActiveRecord e validazioni) per costruire
un **CRUD** completo per il model `Articolo`: le quattro operazioni di base
di ogni applicazione che gestisce dati, ovvero **C**reate, **R**ead,
**U**pdate, **D**elete.

## Il form con form_with

Rails genera i form HTML con l'helper `form_with`, collegato direttamente ad
un oggetto ActiveRecord. Lo stesso form funziona sia per creare un nuovo
articolo sia per modificarne uno esistente, perché Rails capisce da solo se
l'oggetto è nuovo o già salvato:

{% highlight erb %}
<%# app/views/articoli/_form.html.erb %>
<%= form_with model: articolo do |form| %>

  <div>
    <%= form.label :titolo %>
    <%= form.text_field :titolo %>
  </div>

  <div>
    <%= form.label :testo %>
    <%= form.text_area :testo %>
  </div>

  <%= form.submit %>

<% end %>
{% endhighlight %}

Le viste `new` e `edit` riusano la stessa partial:

{% highlight erb %}
<%# app/views/articoli/new.html.erb %>
<h1>Nuovo articolo</h1>
<%= render "form", articolo: @articolo %>
{% endhighlight %}

## Strong parameters

Per motivi di sicurezza, Rails non permette di salvare direttamente tutti i
dati arrivati da un form (`params`): un utente malintenzionato potrebbe
provare ad inviare campi extra non previsti. Per questo si dichiara
esplicitamente quali campi sono ammessi, con `params.require...permit`:

{% highlight ruby %}
private

def articolo_params
  params.require(:articolo).permit(:titolo, :testo)
end
{% endhighlight %}

## Il controller completo

{% highlight ruby %}
# app/controllers/articoli_controller.rb
class ArticoliController < ApplicationController
  before_action :trova_articolo, only: [:show, :edit, :update, :destroy]

  def index
    @articoli = Articolo.all
  end

  def show
  end

  def new
    @articolo = Articolo.new
  end

  def create
    @articolo = Articolo.new(articolo_params)

    if @articolo.save
      redirect_to @articolo, notice: "Articolo creato con successo"
    else
      render :new, status: :unprocessable_entity
    end
  end

  def edit
  end

  def update
    if @articolo.update(articolo_params)
      redirect_to @articolo, notice: "Articolo aggiornato"
    else
      render :edit, status: :unprocessable_entity
    end
  end

  def destroy
    @articolo.destroy
    redirect_to articoli_path, notice: "Articolo eliminato"
  end

  private

  def trova_articolo
    @articolo = Articolo.find(params[:id])
  end

  def articolo_params
    params.require(:articolo).permit(:titolo, :testo)
  end
end
{% endhighlight %}

Da notare come `create` e `update` controllino sempre il risultato di
`save`/`update` (che restituiscono `false` se le validazioni non passano,
come visto nella pagina precedente): solo se l'operazione è andata a buon
fine si fa un `redirect_to`, altrimenti si ri-mostra il form con `render`,
in modo che l'utente veda i messaggi di errore.

## Mostrare gli errori nella view

{% highlight erb %}
<% if articolo.errors.any? %>
  <div class="errori">
    <h2><%= pluralize(articolo.errors.count, "errore") %>:</h2>
    <ul>
      <% articolo.errors.full_messages.each do |messaggio| %>
        <li><%= messaggio %></li>
      <% end %>
    </ul>
  </div>
<% end %>
{% endhighlight %}

## Lo scaffold: generare tutto in automatico

Rails offre anche un comando che genera in un colpo solo model, migrazione,
controller, viste e rotte per un CRUD completo, utile per partire
velocemente o per studiare come è organizzato il codice generato:

{% highlight bash %}
bin/rails generate scaffold Articolo titolo:string testo:text
bin/rails db:migrate
{% endhighlight %}

## Conclusione

Con queste dieci pagine abbiamo percorso tutta la struttura essenziale di
una applicazione Ruby on Rails: dalla richiesta del browser, attraverso il
routing e il controller, fino ai dati salvati nel database con ActiveRecord
e restituiti all'utente tramite le viste. Da qui in avanti la strada
migliore per approfondire è costruire un piccolo progetto personale,
applicando questi stessi passaggi.
