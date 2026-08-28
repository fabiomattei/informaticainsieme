---
title: 'Ruby on Rails: associazioni tra modelli'
date: '2026-08-27T08:07:00+02:00'
author: Fabio Mattei
layout: page
---

## Perché servono le associazioni

Quasi nessuna applicazione reale ha una sola tabella isolata: un blog ha
articoli **e** commenti, un negozio ha prodotti **e** ordini. ActiveRecord
mette a disposizione dei metodi che descrivono le relazioni tra le tabelle
in modo dichiarativo, senza dover scrivere query SQL con `JOIN` a mano.

## belongs_to e has_many

Il caso più comune è la relazione **uno-a-molti**: un articolo ha molti
commenti, ma ogni commento appartiene ad un solo articolo.

Prima si genera la migrazione per il commento, con un riferimento
all'articolo:

{% highlight bash %}
bin/rails generate model Commento testo:text articolo:references
{% endhighlight %}

Il tipo speciale `references` crea automaticamente la colonna
`articolo_id` nella tabella `commenti`, oltre ad un indice per velocizzare
le ricerche.

{% highlight bash %}
bin/rails db:migrate
{% endhighlight %}

Poi si dichiarano le associazioni nei due model:

{% highlight ruby %}
# app/models/articolo.rb
class Articolo < ApplicationRecord
  has_many :commenti
end
{% endhighlight %}

{% highlight ruby %}
# app/models/commento.rb
class Commento < ApplicationRecord
  belongs_to :articolo
end
{% endhighlight %}

## Usare l'associazione

Una volta dichiarata, l'associazione mette a disposizione dei metodi molto
comodi:

{% highlight ruby %}
articolo = Articolo.find(1)

# leggere tutti i commenti di un articolo
articolo.commenti

# creare un nuovo commento già collegato all'articolo
articolo.commenti.create(testo: "Bell'articolo!")

# risalire dal commento all'articolo
commento = Commento.first
commento.articolo.titolo
{% endhighlight %}

## has_one

Simile a `has_many`, ma per una relazione **uno-a-uno**. Ad esempio, ogni
utente ha un solo profilo:

{% highlight ruby %}
class Utente < ApplicationRecord
  has_one :profilo
end

class Profilo < ApplicationRecord
  belongs_to :utente
end
{% endhighlight %}

## has_many :through — relazioni molti-a-molti

Quando due tabelle sono collegate da una relazione **molti-a-molti** (ad
esempio articoli e tag: un articolo può avere più tag, e un tag può essere
usato in più articoli) serve una terza tabella "ponte":

{% highlight ruby %}
class Articolo < ApplicationRecord
  has_many :articoli_tag
  has_many :tag, through: :articoli_tag
end

class Tag < ApplicationRecord
  has_many :articoli_tag
  has_many :articoli, through: :articoli_tag
end

class ArticoloTag < ApplicationRecord
  belongs_to :articolo
  belongs_to :tag
end
{% endhighlight %}

## dependent: :destroy

Quando si cancella un articolo, è opportuno decidere cosa succede ai
commenti collegati. L'opzione `dependent: :destroy` fa in modo che
cancellando l'articolo vengano cancellati automaticamente anche tutti i
suoi commenti, evitando dati "orfani" nel database:

{% highlight ruby %}
class Articolo < ApplicationRecord
  has_many :commenti, dependent: :destroy
end
{% endhighlight %}
