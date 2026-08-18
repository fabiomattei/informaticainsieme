---
title: 'Dragonruby: esempio, Breakout'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Partendo dal [Pong]({{ site.baseurl }}{% link _ruby/pong.md %}.html) appena visto basta
sostituire la seconda racchetta con una griglia di mattoncini da distruggere per ottenere
un altro classico: **Breakout** (conosciuto anche come **Arkanoid**). La racchetta e il
rimbalzo della pallina restano identici, cambia solo contro cosa la pallina rimbalza.

## La racchetta

Una sola racchetta, in basso, che il giocatore muove a sinistra e a destra.

{% highlight ruby %}
def tick args
    args.state.racchetta ||= { x: 560, y: 40, w: 160, h: 20 }

    if args.inputs.left
        args.state.racchetta.x -= 8
    elsif args.inputs.right
        args.state.racchetta.x += 8
    end

    args.outputs.solids << args.state.racchetta.merge(r: 255, g: 255, b: 255)
end
{% endhighlight %}

## La pallina

Esattamente come nella [pallina che rimbalza]({{ site.baseurl }}{% link _ruby/pallina.md %}.html):
rimbalza sopra, a sinistra e a destra, e rimbalza anche contro la racchetta, come nel Pong.

{% highlight ruby %}
def tick args
    args.state.pallina ||= { x: 630, y: 300, w: 20, h: 20, dx: 4, dy: 5 }
    pallina = args.state.pallina

    pallina.x += pallina.dx
    pallina.y += pallina.dy

    if pallina.x < 0 || pallina.x + pallina.w > 1280
        pallina.dx *= -1
    end

    if pallina.y + pallina.h > 720
        pallina.dy *= -1
    end

    if pallina.intersect_rect?(args.state.racchetta)
        pallina.dy *= -1
    end

    args.outputs.solids << pallina.merge(r: 255, g: 255, b: 255)
end
{% endhighlight %}

## La griglia di mattoncini

All'inizio della partita generiamo una griglia di mattoncini, un po' come abbiamo generato
i nemici nella pagina sullo [spawn dei nemici]({{ site.baseurl }}{% link _ruby/spawn.md %}.html),
ma tutti insieme in un colpo solo, prima ancora che inizi il gioco.

{% highlight ruby %}
def crea_mattoncini
    mattoncini = []

    5.times do |riga|
        8.times do |colonna|
            mattoncini << {
                x: 40 + colonna * 150,
                y: 500 + riga * 35,
                w: 140,
                h: 25,
                r: 250 - riga * 30,
                g: 120 + riga * 20,
                b: 90
            }
        end
    end

    mattoncini
end

def tick args
    args.state.mattoncini ||= crea_mattoncini

    args.outputs.solids << args.state.mattoncini
end
{% endhighlight %}

Ogni riga della griglia riceve un colore leggermente diverso, calcolato a partire dal
numero della riga, così la griglia risulta più leggibile a colpo d'occhio.

## Distruggere un mattoncino

Quando la pallina tocca un mattoncino, quel mattoncino va rimosso e la pallina deve
rimbalzare, esattamente come rimbalza contro la racchetta. Usiamo **find**, che restituisce
il primo elemento dell'array per cui il blocco è vero, oppure nil se nessuno lo soddisfa.

{% highlight ruby %}
def tick args
    args.state.punteggio ||= 0

    mattoncino_colpito = args.state.mattoncini.find { |m| pallina.intersect_rect?(m) }

    if mattoncino_colpito
        args.state.mattoncini.delete(mattoncino_colpito)
        pallina.dy *= -1
        args.state.punteggio += 10
        args.outputs.sounds << "sounds/mattoncino.wav"
    end
end
{% endhighlight %}

A differenza degli esempi precedenti, dove si marcavano più elementi con **morto = true**
e poi si rimuovevano tutti insieme con **reject!**, qui basta **delete** perché ad ogni
tick la pallina può toccare al massimo un mattoncino: non c'è bisogno del pattern
"marca e poi rimuovi".

## Vincere e perdere

Il giocatore vince quando l'array dei mattoncini è vuoto, e perde quando la pallina scende
sotto il fondo dello schermo senza che la racchetta l'abbia intercettata.

{% highlight ruby %}
def tick args
    if args.state.mattoncini.empty?
        args.outputs.labels << { x: 640, y: 400, text: "Hai vinto!", anchor_x: 0.5 }
    end

    if pallina.y < 0
        args.outputs.labels << { x: 640, y: 400, text: "Game Over", anchor_x: 0.5 }
    end
end
{% endhighlight %}

## Il gioco completo

{% highlight ruby %}
def crea_mattoncini
    mattoncini = []
    5.times do |riga|
        8.times do |colonna|
            mattoncini << {
                x: 40 + colonna * 150,
                y: 500 + riga * 35,
                w: 140,
                h: 25,
                r: 250 - riga * 30,
                g: 120 + riga * 20,
                b: 90
            }
        end
    end
    mattoncini
end

def tick args
    args.state.racchetta ||= { x: 560, y: 40, w: 160, h: 20 }
    args.state.pallina ||= { x: 630, y: 300, w: 20, h: 20, dx: 4, dy: 5 }
    args.state.mattoncini ||= crea_mattoncini
    args.state.punteggio ||= 0
    args.state.finita ||= false

    pallina = args.state.pallina

    if args.state.finita
        args.outputs.labels << {
            x: 640, y: 400,
            text: args.state.mattoncini.empty? ? "Hai vinto!" : "Game Over",
            anchor_x: 0.5
        }
        return
    end

    # movimento della racchetta
    if args.inputs.left
        args.state.racchetta.x -= 8
    elsif args.inputs.right
        args.state.racchetta.x += 8
    end

    # movimento della pallina
    pallina.x += pallina.dx
    pallina.y += pallina.dy

    if pallina.x < 0 || pallina.x + pallina.w > 1280
        pallina.dx *= -1
    end
    if pallina.y + pallina.h > 720
        pallina.dy *= -1
    end
    if pallina.intersect_rect?(args.state.racchetta)
        pallina.dy *= -1
    end

    # collisione con i mattoncini
    mattoncino_colpito = args.state.mattoncini.find { |m| pallina.intersect_rect?(m) }
    if mattoncino_colpito
        args.state.mattoncini.delete(mattoncino_colpito)
        pallina.dy *= -1
        args.state.punteggio += 10
        args.outputs.sounds << "sounds/mattoncino.wav"
    end

    # fine della partita
    args.state.finita = args.state.mattoncini.empty? || pallina.y < 0

    # disegno
    args.outputs.solids << args.state.racchetta.merge(r: 255, g: 255, b: 255)
    args.outputs.solids << pallina.merge(r: 255, g: 255, b: 255)
    args.outputs.solids << args.state.mattoncini
    args.outputs.labels << { x: 20, y: 700, text: "Punteggio: #{args.state.punteggio}" }
end
{% endhighlight %}

## Come continuare

* far ripartire la pallina da una posizione casuale sopra la racchetta invece di terminare
  subito la partita quando la si perde, tenendo un contatore di vite come nello
  [space shooter]({{ site.baseurl }}{% link _ruby/sparatutto.md %}.html)
* aggiungere delle piccole particelle quando un mattoncino si rompe, come visto nella pagina
  sulle [particelle]({{ site.baseurl }}{% link _ruby/particelle.md %}.html)
* salvare il [punteggio più alto]({{ site.baseurl }}{% link _ruby/salvataggio.md %}.html)
  ottenuto tra una partita e l'altra
* trasformare il messaggio finale in una vera schermata di [game over]({{ site.baseurl }}{% link _ruby/scene.md %}.html)
  da cui si può ricominciare una nuova partita
