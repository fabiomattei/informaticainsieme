---
title: 'Dragonruby: far apparire i nemici nel tempo'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Un gioco diventa interessante quando la difficoltà aumenta man mano che si prosegue nella
partita. Il modo più semplice per farlo è far apparire nuovi nemici ad intervalli regolari,
e far diminuire quell'intervallo con il passare del tempo, così i nemici compaiono sempre
più frequentemente.

## Un nemico ogni N tick

Dato che la funzione **tick** viene chiamata 60 volte al secondo, possiamo far apparire un
nuovo nemico esattamente ogni N tick controllando il resto della divisione tra
**Kernel.tick_count** e N con l'operatore **%** (modulo).

{% highlight ruby %}
def tick args
    args.state.nemici ||= []

    # un nuovo nemico ogni 90 tick, cioè circa una volta e mezzo al secondo
    if Kernel.tick_count % 90 == 0
        args.state.nemici << {
            x: 1280,
            y: rand(720),
            w: 60,
            h: 60,
            path: 'sprites/dragon-0.png'
        }
    end

    args.state.nemici.each { |nemico| nemico.x -= 4 }

    args.outputs.sprites << args.state.nemici
end
{% endhighlight %}

Il nemico appare sempre al margine destro dello schermo (x: 1280) e ad un'altezza casuale,
poi si sposta verso sinistra ad ogni tick.

## Aumentare la difficoltà nel tempo

Per far comparire i nemici sempre più spesso man mano che la partita prosegue, l'intervallo
tra un nemico e l'altro non deve restare fisso: lo facciamo diminuire gradualmente, senza
però scendere sotto una soglia minima che renderebbe il gioco impossibile da giocare.

{% highlight ruby %}
def tick args
    args.state.nemici ||= []

    # l'intervallo scende da 90 a 20 tick nel corso della partita
    intervallo = (90 - Kernel.tick_count / 60).clamp(20, 90)

    if Kernel.tick_count % intervallo == 0
        args.state.nemici << {
            x: 1280,
            y: rand(720),
            w: 60,
            h: 60,
            path: 'sprites/dragon-0.png'
        }
    end

    args.state.nemici.each { |nemico| nemico.x -= 4 }
    args.state.nemici.reject! { |nemico| nemico.x < -60 }

    args.outputs.sprites << args.state.nemici
end
{% endhighlight %}

Da notare **reject!**, già visto nella pagina sulle [collisioni]({{ site.baseurl }}{% link _ruby/collisioni.md %}.html):
è importante togliere dall'array i nemici usciti dallo schermo, altrimenti l'array
continuerebbe a crescere per tutta la durata della partita, occupando sempre più memoria e
tempo di calcolo ad ogni tick.

## Un'ondata di nemici invece di uno alla volta

Un'alternativa che dà un ritmo diverso al gioco è, invece di far apparire un nemico alla
volta a intervalli brevi, far apparire un gruppo di nemici tutti insieme ad intervalli più
lunghi: è la tecnica delle **ondate** (in inglese **waves**), comune nei giochi arcade.

{% highlight ruby %}
def tick args
    args.state.nemici ||= []
    args.state.numero_ondata ||= 0

    if Kernel.tick_count % 300 == 0
        args.state.numero_ondata += 1

        (3 + args.state.numero_ondata).times do |indice|
            args.state.nemici << {
                x: 1280,
                y: 60 + indice * 80,
                w: 60,
                h: 60,
                path: 'sprites/dragon-0.png'
            }
        end
    end
end
{% endhighlight %}

Ogni ondata, oltre ad avvenire ogni 300 tick (5 secondi), contiene un nemico in più della
precedente: la partita diventa via via più impegnativa senza bisogno di modificare la
velocità o le altre caratteristiche dei nemici.
