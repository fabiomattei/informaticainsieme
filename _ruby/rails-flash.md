---
title: 'Ruby on Rails: i messaggi flash'
date: '2026-08-27T08:10:00+02:00'
author: Fabio Mattei
layout: page
---

## Cosa sono i messaggi flash

Nella pagina sul CRUD abbiamo già usato `notice` e `alert` dentro
`redirect_to`, senza spiegarli nel dettaglio:

{% highlight ruby %}
redirect_to @articolo, notice: "Articolo creato con successo"
{% endhighlight %}

Questi sono **messaggi flash**: un piccolo messaggio che sopravvive per
**una sola richiesta successiva**, pensato apposta per comunicare
all'utente l'esito di un'operazione (salvataggio, cancellazione, errore...)
subito dopo un `redirect_to`. Dopo essere stato mostrato, il messaggio
scompare automaticamente, anche se l'utente ricarica la pagina.

## flash come hash

`notice` e `alert` sono in realtà due scorciatoie per le chiavi più comuni
di un hash chiamato `flash`. Si possono usare direttamente, con qualunque
nome di chiave:

{% highlight ruby %}
def create
  @articolo = Articolo.new(articolo_params)

  if @articolo.save
    flash[:notice] = "Articolo creato con successo"
    redirect_to @articolo
  else
    flash.now[:alert] = "Controlla i campi del form"
    render :new, status: :unprocessable_entity
  end
end
{% endhighlight %}

## flash vs flash.now

C'è una differenza importante tra le due forme:

* `flash[:chiave] = ...` insieme a `redirect_to`: il messaggio sopravvive
  fino alla **prossima** richiesta (quella generata dal redirect);
* `flash.now[:chiave] = ...` insieme a `render`: il messaggio è disponibile
  **solo per la view che sta per essere disegnata subito**, senza
  sopravvivere oltre.

La regola pratica è semplice: `flash` con `redirect_to`, `flash.now` con
`render`.

## Mostrare i messaggi nel layout

Per non dover ripetere il codice in ogni view, i messaggi flash si mostrano
una volta sola nel layout comune:

{% highlight erb %}
<%# app/views/layouts/application.html.erb %}
<body>
  <% flash.each do |tipo, messaggio| %>
    <div class="flash flash-<%= tipo %>">
      <%= messaggio %>
    </div>
  <% end %>

  <%= yield %>
</body>
{% endhighlight %}

In questo modo, qualunque controller imposti un messaggio flash (con
qualunque chiave), questo verrà mostrato automaticamente in cima ad ogni
pagina, senza bisogno di scrivere altro codice nelle singole view.
