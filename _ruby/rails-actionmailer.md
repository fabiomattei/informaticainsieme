---
title: 'Ruby on Rails: inviare email con ActionMailer'
date: '2026-08-27T08:15:00+02:00'
author: Fabio Mattei
layout: page
---

## Cos'è ActionMailer

**ActionMailer** permette di inviare email seguendo lo stesso schema già
visto per controller e viste: un **mailer** è come un controller, con delle
azioni che, invece di disegnare una pagina HTML da mostrare nel browser,
compongono una email da spedire.

## Generare un mailer

{% highlight bash %}
bin/rails generate mailer NotificaMailer nuovo_commento
{% endhighlight %}

Questo comando crea il file `app/mailers/notifica_mailer.rb` e le viste
corrispondenti (una versione testuale e una HTML) in
`app/views/notifica_mailer/`.

## Il mailer

{% highlight ruby %}
# app/mailers/notifica_mailer.rb
class NotificaMailer < ApplicationMailer

  def nuovo_commento(commento)
    @commento = commento
    @articolo = commento.articolo

    mail(
      to: "autore@esempio.it",
      subject: "Nuovo commento su \"#{@articolo.titolo}\""
    )
  end

end
{% endhighlight %}

## La view della email

{% highlight erb %}
<%# app/views/notifica_mailer/nuovo_commento.html.erb %}
<h1>Nuovo commento</h1>

<p>
  <%= @commento.testo %>
</p>

<p>
  <%= link_to "Vai all'articolo", articolo_url(@articolo) %>
</p>
{% endhighlight %}

Da notare che nelle email si usa `articolo_url` (con l'indirizzo completo,
comprensivo di dominio) e non `articolo_path`: un link relativo non avrebbe
senso all'interno di un client di posta.

## Spedire la email

Dal controller, tipicamente dopo aver salvato un nuovo commento:

{% highlight ruby %}
def create
  @commento = @articolo.commenti.new(commento_params)

  if @commento.save
    NotificaMailer.nuovo_commento(@commento).deliver_later
    redirect_to @articolo, notice: "Commento aggiunto"
  else
    render :new, status: :unprocessable_entity
  end
end
{% endhighlight %}

* **deliver_later**: mette l'invio in coda per essere eseguito in background
  (lo vedremo nella prossima pagina, dedicata ad ActiveJob), senza far
  attendere l'utente;
* **deliver_now**: invia subito la email, bloccando la richiesta fino a
  quando l'invio non è completato (da evitare quasi sempre in produzione).

## Configurazione in sviluppo

In ambiente di sviluppo, per non inviare davvero email mentre si sta
programmando, Rails offre la gemma **letter_opener**: invece di spedire il
messaggio, lo apre automaticamente nel browser così da poterlo vedere.

{% highlight ruby %}
# config/environments/development.rb
config.action_mailer.delivery_method = :letter_opener
config.action_mailer.perform_deliveries = true
{% endhighlight %}

In produzione, invece, si configura un vero servizio SMTP (o un servizio
come SendGrid, Mailgun...) dentro `config/environments/production.rb`.
