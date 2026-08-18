---
title: 'Dragonruby: la telecamera'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Finora abbiamo sempre disegnato gli sprite usando direttamente le coordinate del
[piano cartesiano]({{ site.baseurl }}{% link _ruby/sprites.md %}.html) largo 1280 e alto 720
pixel. Ma un livello di gioco è spesso molto più grande dello schermo: pensiamo ad un gioco
a piattaforme che si estende per migliaia di pixel verso destra. Dobbiamo quindi introdurre
il concetto di **telecamera**: una finestra che segue il giocatore e mostra soltanto la
porzione di mondo di gioco che gli sta vicino.

Dragonruby non fornisce un oggetto telecamera pronto all'uso: si tratta di una tecnica che
possiamo implementare noi stessi con poche righe di codice, spostando tutti gli sprite sullo
schermo di una quantità uguale e contraria alla posizione della telecamera.

## Il principio

Immaginiamo che il giocatore si trovi nel mondo di gioco alla posizione x = 2000. Per farlo
apparire al centro dello schermo (x = 640) dobbiamo disegnarlo non alle sue coordinate reali,
ma a quelle coordinate meno la posizione della telecamera.

{% highlight ruby %}
def tick args
    args.state.giocatore ||= { x: 2000, y: 300 }
    args.state.telecamera ||= { x: 0, y: 0 }

    # la telecamera segue il giocatore, mantenendolo al centro dello schermo
    args.state.telecamera.x = args.state.giocatore.x - 640
    args.state.telecamera.y = args.state.giocatore.y - 360

    args.outputs.sprites << {
        x: args.state.giocatore.x - args.state.telecamera.x,
        y: args.state.giocatore.y - args.state.telecamera.y,
        w: 80,
        h: 80,
        path: 'sprites/dragon-0.png'
    }
end
{% endhighlight %}

Da notare che il giocatore, in questo esempio, resta sempre fermo al centro dello schermo:
è il mondo di gioco che scorre sotto di lui, non lui che si muove sullo schermo.

## Disegnare tutti gli sprite tenendo conto della telecamera

Con più di uno sprite conviene scrivere un piccolo metodo di supporto che applichi la
sottrazione della telecamera, per non doverla ripetere per ogni sprite.

{% highlight ruby %}
def applica_telecamera args, sprite
    {
        **sprite,
        x: sprite.x - args.state.telecamera.x,
        y: sprite.y - args.state.telecamera.y
    }
end

def tick args
    args.state.telecamera ||= { x: 0, y: 0 }
    args.state.nemici ||= [
        { x: 2200, y: 300, w: 80, h: 80, path: 'sprites/dragon-0.png' },
        { x: 2500, y: 300, w: 80, h: 80, path: 'sprites/dragon-0.png' }
    ]

    args.state.nemici.each do |nemico|
        args.outputs.sprites << applica_telecamera(args, nemico)
    end
end
{% endhighlight %}

## Limitare la telecamera ai confini del livello

Se il giocatore si avvicina all'inizio o alla fine del livello, la telecamera non deve
mostrare l'area vuota che sta oltre i confini del mondo di gioco. Basta bloccare il valore
della telecamera tra un minimo e un massimo con **clamp**.

{% highlight ruby %}
def tick args
    larghezza_livello = 4000

    args.state.telecamera.x = (args.state.giocatore.x - 640).clamp(0, larghezza_livello - 1280)
end
{% endhighlight %}

In questo modo, quando il giocatore è vicino al bordo sinistro o destro del livello, la
telecamera si ferma invece di continuare a seguirlo, e lo schermo mostra sempre una porzione
piena di mondo di gioco.
