---
title: 'Ruby: classi'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Classi

In Ruby le classi si definiscono con la parola chiave `class` e si chiudono con `end`.
Il costruttore si chiama sempre `initialize`.

Nell'esempio seguente definiamo la classe `Cerchio` con un attributo di istanza `@raggio`
e un attributo di classe `@@pigreco`, condiviso da tutte le istanze.

{% highlight ruby %}
class Cerchio
  @@pigreco = 3.14

  def initialize(raggio)
    @raggio = raggio
  end

  def calcola_area
    @raggio ** 2 * @@pigreco
  end
end

# main
cerchio = Cerchio.new(4)
puts cerchio.calcola_area
{% endhighlight %}

### Ereditarietà

La dipendenza di ereditarietà tra classi si esprime con il simbolo `<`.
Il metodo `super` richiama il metodo omonimo della classe padre.

{% highlight ruby %}
class Persona
  def initialize(nome, cognome)
    @nome = nome
    @cognome = cognome
  end

  def saluta
    @nome + ' ' + @cognome
  end
end

class Studente < Persona
  def initialize(nome, cognome, scuola)
    super(nome, cognome)
    @scuola = scuola
  end

  def saluta
    @nome + ' ' + @cognome + ' frequento: ' + @scuola
  end
end

# main
mario = Studente.new('Mario', 'Rossi', 'Patini')
puts mario.saluta
{% endhighlight %}
