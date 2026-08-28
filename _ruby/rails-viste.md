---
title: 'Ruby on Rails: viste, layout e helper'
date: '2026-08-27T08:04:00+02:00'
author: Fabio Mattei
layout: page
---

## Cosa sono le viste

Le **viste** sono i file che definiscono cosa vede l'utente nel browser.
Si trovano dentro `app/views/`, organizzate in una sottocartella per ogni
controller, e sono normali file HTML arricchiti da un po' di codice Ruby:
questa sintassi si chiama **ERB** (Embedded RuBy) e i file hanno estensione
`.html.erb`.

## Sintassi ERB

Esistono due tag principali:

* `<% codice ruby %>`: esegue del codice Ruby ma **non stampa** nulla
  (utile per cicli, condizioni...);
* `<%= codice ruby %>`: esegue il codice e **stampa** il risultato nella
  pagina HTML.

{% highlight erb %}
<h1>Elenco articoli</h1>

<ul>
  <% @articoli.each do |articolo| %>
    <li><%= articolo.titolo %></li>
  <% end %>
</ul>
{% endhighlight %}

Da notare come la variabile `@articoli`, valorizzata nell'azione `index` del
controller (vista nella pagina precedente), sia già disponibile qui: è
proprio questo il collegamento automatico tra controller e view.

## Il layout

Per non dover ripetere in ogni pagina l'intestazione HTML, il menu, il
footer, eccetera, Rails usa un **layout** comune, che si trova di norma in
`app/views/layouts/application.html.erb`:

{% highlight erb %}
<!DOCTYPE html>
<html>
  <head>
    <title>Il mio blog</title>
    <%= csrf_meta_tags %>
    <%= stylesheet_link_tag "application" %>
  </head>
  <body>
    <nav>Il mio blog</nav>

    <%= yield %>

  </body>
</html>
{% endhighlight %}

La parola chiave `yield` indica il punto in cui verrà inserito il contenuto
della view specifica richiesta dal controller (ad esempio
`articoli/index.html.erb`).

## link_to: creare collegamenti

Invece di scrivere gli indirizzi a mano, si usa l'helper `link_to`, che
sfrutta i path helper generati dal routing (visti nella pagina precedente):

{% highlight erb %}
<%= link_to "Vedi articolo", articolo_path(articolo) %>
<%= link_to "Nuovo articolo", new_articolo_path %>
<%= link_to "Elimina", articolo_path(articolo), data: { turbo_method: :delete } %>
{% endhighlight %}

## Le partial: riusare pezzi di HTML

Quando un pezzo di view viene ripetuto in più punti (ad esempio la scheda di
un singolo articolo) conviene estrarlo in una **partial**: un file che, per
convenzione, inizia con un underscore.

{% highlight erb %}
<%# app/views/articoli/_articolo.html.erb %>
<div class="articolo">
  <h2><%= articolo.titolo %></h2>
  <p><%= articolo.testo %></p>
</div>
{% endhighlight %}

Si richiama con `render`, sia una sola volta sia su una collezione:

{% highlight erb %}
<%= render @articolo %>

<%# oppure, per mostrare tutti gli articoli di un array: %>
<%= render @articoli %>
{% endhighlight %}
