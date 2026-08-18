---
title: 'Dragonruby: esempio, un platform con salto'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Nessuno degli esempi visti finora ha ancora usato la **gravità**: nella
[pallina che rimbalza]({{ site.baseurl }}{% link _ruby/pallina.md %}.html) e nei giochi
che ne derivano la velocità verticale resta sempre costante, e cambia segno soltanto quando
si tocca un bordo. In un gioco a piattaforme, invece, la velocità verticale del personaggio
diminuisce di continuo, proprio come nella realtà, ed è quello che permette di saltare e poi
ricadere seguendo una traiettoria curva.

## La gravità

Aggiungiamo al giocatore una velocità verticale **dy**, che ad ogni tick diminuisce di una
piccola quantità costante: è esattamente la gravità.

{% highlight ruby %}
GRAVITA = 0.4

def tick args
    args.state.giocatore ||= { x: 100, y: 100, w: 50, h: 60, dy: 0 }
    giocatore = args.state.giocatore

    giocatore.dy -= GRAVITA
    giocatore.y += giocatore.dy

    args.outputs.sprites << giocatore.merge(path: 'sprites/giocatore.png')
end
{% endhighlight %}

Provando questo codice da solo il personaggio cadrebbe all'infinito, sempre più veloce, dato
che nulla lo ferma: ci servono delle piattaforme su cui atterrare.

## Le piattaforme

Le piattaforme sono semplici rettangoli, come i [solidi]({{ site.baseurl }}{% link _ruby/forme.md %}.html)
già visti: una grande piattaforma per il terreno, e alcune più piccole per creare dei livelli
da scalare.

{% highlight ruby %}
def tick args
    args.state.piattaforme ||= [
        { x: 0, y: 0, w: 1280, h: 40 },      # il terreno
        { x: 300, y: 200, w: 200, h: 30 },
        { x: 700, y: 350, w: 200, h: 30 },
        { x: 200, y: 450, w: 200, h: 30 }
    ]

    args.outputs.solids << args.state.piattaforme.map { |p| p.merge(r: 90, g: 60, b: 40) }
end
{% endhighlight %}

## Atterrare sulle piattaforme

Il giocatore deve fermarsi non appena tocca la parte superiore di una piattaforma mentre
sta cadendo, invece di attraversarla. Controlliamo la collisione, come visto nella pagina
sulle [collisioni]({{ site.baseurl }}{% link _ruby/collisioni.md %}.html), ma solo se il
giocatore si sta muovendo verso il basso (**dy <= 0**): questo evita che, saltando da sotto
verso l'alto, venga fermato dal fondo della piattaforma invece di poterci passare sopra in
un salto successivo.

{% highlight ruby %}
def tick args
    args.state.a_terra = false

    args.state.piattaforme.each do |piattaforma|
        if giocatore.intersect_rect?(piattaforma) && giocatore.dy <= 0
            giocatore.y = piattaforma.y + piattaforma.h
            giocatore.dy = 0
            args.state.a_terra = true
        end
    end
end
{% endhighlight %}

**a_terra** ("a terra", cioè "sul terreno") ricorda se il giocatore in questo momento sta
poggiando su una piattaforma: ci servirà per decidere se può saltare oppure non.

## Saltare

Un salto è semplicemente un'improvvisa velocità verticale positiva, che verrà poi
progressivamente ridotta dalla gravità, dando la classica traiettoria a parabola. Si può
saltare solo se in questo momento si è **a_terra**: altrimenti il giocatore potrebbe
continuare a saltare a mezz'aria all'infinito.

{% highlight ruby %}
if args.state.a_terra && args.inputs.keyboard.key_down.space
    giocatore.dy = 12
end
{% endhighlight %}

## Muoversi a sinistra e a destra

Il movimento orizzontale non è influenzato dalla gravità, quindi resta identico a quello
visto nella pagina sull'[input]({{ site.baseurl }}{% link _ruby/input.md %}.html).

{% highlight ruby %}
if args.inputs.left
    giocatore.x -= 6
elsif args.inputs.right
    giocatore.x += 6
end
{% endhighlight %}

## Il gioco completo

{% highlight ruby %}
GRAVITA = 0.4

def tick args
    args.state.giocatore ||= { x: 100, y: 100, w: 50, h: 60, dy: 0 }
    args.state.piattaforme ||= [
        { x: 0, y: 0, w: 1280, h: 40 },
        { x: 300, y: 200, w: 200, h: 30 },
        { x: 700, y: 350, w: 200, h: 30 },
        { x: 200, y: 450, w: 200, h: 30 }
    ]

    giocatore = args.state.giocatore

    # movimento orizzontale
    if args.inputs.left
        giocatore.x -= 6
    elsif args.inputs.right
        giocatore.x += 6
    end

    # gravità
    giocatore.dy -= GRAVITA
    giocatore.y += giocatore.dy

    # atterraggio sulle piattaforme
    args.state.a_terra = false
    args.state.piattaforme.each do |piattaforma|
        if giocatore.intersect_rect?(piattaforma) && giocatore.dy <= 0
            giocatore.y = piattaforma.y + piattaforma.h
            giocatore.dy = 0
            args.state.a_terra = true
        end
    end

    # salto
    if args.state.a_terra && args.inputs.keyboard.key_down.space
        giocatore.dy = 12
    end

    # se cade fuori dallo schermo, ricomincia dall'inizio
    if giocatore.y < -200
        giocatore.x = 100
        giocatore.y = 100
        giocatore.dy = 0
    end

    # disegno
    args.outputs.solids << args.state.piattaforme.map { |p| p.merge(r: 90, g: 60, b: 40) }
    args.outputs.sprites << giocatore.merge(path: 'sprites/giocatore.png')
end
{% endhighlight %}

## Come continuare

* far seguire il giocatore dalla [telecamera]({{ site.baseurl }}{% link _ruby/telecamera.md %}.html)
  invece di tenerla ferma, per costruire un livello molto più lungo di un solo schermo
* aggiungere delle monete da raccogliere, con la stessa tecnica del cibo vista in
  [Snake]({{ site.baseurl }}{% link _ruby/snake.md %}.html): una lista di posizioni, che si
  rimuovono quando il giocatore le tocca
* aggiungere dei nemici che si muovono avanti e indietro su una piattaforma, e che fanno
  perdere se il giocatore li tocca, come nella pagina sullo [spawn dei nemici]({{ site.baseurl }}{% link _ruby/spawn.md %}.html)
* far muovere una piattaforma avanti e indietro nel tempo usando le tecniche di
  [easing]({{ site.baseurl }}{% link _ruby/easing.md %}.html), per creare piattaforme mobili
