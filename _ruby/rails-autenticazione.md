---
title: 'Ruby on Rails: autenticazione degli utenti'
date: '2026-08-27T08:12:00+02:00'
author: Fabio Mattei
layout: page
---

## Il model Utente

Rails include, di serie, tutto il necessario per gestire un login sicuro
basato su email e password, tramite il metodo `has_secure_password`, che si
appoggia alla gemma **bcrypt** (già presente di default nel `Gemfile`, basta
togliere il commento).

Si genera il model con una colonna speciale, `password_digest`, che conterrà
la password **cifrata** (mai salvare le password in chiaro nel database!):

{% highlight bash %}
bin/rails generate model Utente email:string password_digest:string
bin/rails db:migrate
{% endhighlight %}

{% highlight ruby %}
# app/models/utente.rb
class Utente < ApplicationRecord
  has_secure_password
  validates :email, presence: true, uniqueness: true
end
{% endhighlight %}

`has_secure_password` aggiunge automaticamente al model due metodi virtuali,
`password` e `password_confirmation`, oltre al metodo `authenticate`, che
verifica se una password in chiaro corrisponde a quella salvata:

{% highlight ruby %}
utente = Utente.create(email: "mario@example.com", password: "segreto123")

utente.authenticate("sbagliata")   # => false
utente.authenticate("segreto123")  # => restituisce l'utente stesso
{% endhighlight %}

## Registrazione

{% highlight ruby %}
# app/controllers/utenti_controller.rb
class UtentiController < ApplicationController
  def new
    @utente = Utente.new
  end

  def create
    @utente = Utente.new(utente_params)
    if @utente.save
      session[:utente_id] = @utente.id
      redirect_to root_path, notice: "Registrazione completata"
    else
      render :new, status: :unprocessable_entity
    end
  end

  private

  def utente_params
    params.require(:utente).permit(:email, :password, :password_confirmation)
  end
end
{% endhighlight %}

## Login e logout con le sessioni

Il **login** consiste semplicemente nel ritrovare l'utente e, se le
credenziali sono corrette, salvarne l'id nella **sessione**: un piccolo
spazio di dati che Rails mantiene per ogni visitatore tra una richiesta e
l'altra (tipicamente tramite un cookie cifrato).

{% highlight ruby %}
# app/controllers/sessioni_controller.rb
class SessioniController < ApplicationController
  def new
  end

  def create
    utente = Utente.find_by(email: params[:email])

    if utente&.authenticate(params[:password])
      session[:utente_id] = utente.id
      redirect_to root_path, notice: "Accesso effettuato"
    else
      flash.now[:alert] = "Email o password non corretti"
      render :new, status: :unprocessable_entity
    end
  end

  def destroy
    session[:utente_id] = nil
    redirect_to root_path, notice: "Sei uscito"
  end
end
{% endhighlight %}

## current_user, disponibile ovunque

Per non dover ripetere la ricerca dell'utente in ogni controller, si
definisce un metodo di comodo in `ApplicationController`, reso disponibile
anche alle viste con `helper_method`:

{% highlight ruby %}
# app/controllers/application_controller.rb
class ApplicationController < ActionController::Base
  helper_method :utente_corrente

  private

  def utente_corrente
    @utente_corrente ||= Utente.find_by(id: session[:utente_id])
  end
end
{% endhighlight %}

{% highlight erb %}
<% if utente_corrente %>
  Ciao, <%= utente_corrente.email %>!
  <%= link_to "Esci", sessione_path(utente_corrente), data: { turbo_method: :delete } %>
<% else %>
  <%= link_to "Accedi", new_sessione_path %>
<% end %>
{% endhighlight %}

## Proteggere le pagine riservate

Con un `before_action` si possono bloccare le azioni che richiedono
l'accesso, in qualunque controller:

{% highlight ruby %}
class ArticoliController < ApplicationController
  before_action :richiedi_accesso, only: [:new, :create, :edit, :update, :destroy]

  private

  def richiedi_accesso
    unless utente_corrente
      redirect_to new_sessione_path, alert: "Devi accedere prima"
    end
  end
end
{% endhighlight %}

Per applicazioni più complesse (recupero password, conferma email, accesso
tramite Google...) esistono gemme dedicate come **Devise**, ma capire prima
questo meccanismo "fatto a mano" aiuta molto a capire cosa fanno davvero
quelle gemme sotto al cofano.
