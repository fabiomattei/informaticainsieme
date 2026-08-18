---
title: 'Dragonruby: esempio, Pong'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Come anticipato nella pagina sulla [pallina che rimbalza]({{ site.baseurl }}{% link _ruby/pallina.md %}.html),
lo stesso principio usato per farla rimbalzare sui bordi dello schermo basta, con una
piccola aggiunta, a costruire uno dei videogiochi più famosi della storia: **Pong**. Ci
servono soltanto due racchette che il giocatore controlla con la tastiera, e la pallina che
deve rimbalzare anche contro di loro.

## Le racchette

Ogni racchetta è un semplice rettangolo verticale, posizionato sul lato sinistro o destro
dello schermo. Usiamo i tasti **W** e **S** per la racchetta di sinistra, e le freccette
**su** e **giù** per la racchetta di destra, esattamente come visto nella pagina
sull'[input]({{ site.baseurl }}{% link _ruby/input.md %}.html).

{% highlight ruby %}
def tick args
    args.state.racchetta_sinistra ||= { x: 40, y: 300, w: 20, h: 120 }
    args.state.racchetta_destra ||= { x: 1220, y: 300, w: 20, h: 120 }

    if args.inputs.keyboard.key_held.w
        args.state.racchetta_sinistra.y += 8
    elsif args.inputs.keyboard.key_held.s
        args.state.racchetta_sinistra.y -= 8
    end

    if args.inputs.keyboard.key_held.up
        args.state.racchetta_destra.y += 8
    elsif args.inputs.keyboard.key_held.down
        args.state.racchetta_destra.y -= 8
    end

    args.outputs.solids << args.state.racchetta_sinistra.merge(r: 255, g: 255, b: 255)
    args.outputs.solids << args.state.racchetta_destra.merge(r: 255, g: 255, b: 255)
end
{% endhighlight %}

**merge** crea una copia del dizionario della racchetta aggiungendo le chiavi del colore,
senza doverle salvare permanentemente nello stato: ci servono solo al momento di disegnare.

## La pallina, come nell'esempio precedente

Riprendiamo la pallina che si muove e rimbalza sopra e sotto, esattamente come nella pagina
precedente: cambia solo che, invece di rimbalzare anche a sinistra e a destra, quando la
pallina supera il bordo sinistro o destro significa che una racchetta non è arrivata in
tempo, quindi la pallina è persa.

{% highlight ruby %}
def tick args
    args.state.pallina ||= { x: 630, y: 350, w: 20, h: 20, dx: 6, dy: 4 }
    pallina = args.state.pallina

    pallina.x += pallina.dx
    pallina.y += pallina.dy

    if pallina.y < 0 || pallina.y + pallina.h > 720
        pallina.dy *= -1
    end

    args.outputs.solids << pallina.merge(r: 255, g: 255, b: 255)
end
{% endhighlight %}

## Rimbalzare contro le racchette

Usiamo **intersect_rect?**, visto nella pagina sulle [collisioni]({{ site.baseurl }}{% link _ruby/collisioni.md %}.html),
per capire se la pallina ha toccato una racchetta. In quel caso invertiamo **dx**, cioè la
velocità orizzontale, esattamente come abbiamo invertito dy per il rimbalzo sopra e sotto.

{% highlight ruby %}
if pallina.intersect_rect?(args.state.racchetta_sinistra) ||
   pallina.intersect_rect?(args.state.racchetta_destra)
    pallina.dx *= -1
end
{% endhighlight %}

## Il punteggio

Quando la pallina esce completamente dallo schermo a sinistra o a destra, un giocatore ha
segnato un punto contro l'altro, e la pallina va rimessa al centro per ricominciare lo
scambio.

{% highlight ruby %}
args.state.punti_sinistra ||= 0
args.state.punti_destra ||= 0

if pallina.x < 0
    args.state.punti_destra += 1
    pallina.x = 630
    pallina.y = 350
    pallina.dx = 6
elsif pallina.x > 1280
    args.state.punti_sinistra += 1
    pallina.x = 630
    pallina.y = 350
    pallina.dx = -6
end

args.outputs.labels << {
    x: 640,
    y: 690,
    text: "#{args.state.punti_sinistra}   -   #{args.state.punti_destra}",
    anchor_x: 0.5,
    size_px: 40
}
{% endhighlight %}

## Il gioco completo

Mettendo insieme tutti i pezzi visti sopra otteniamo il gioco completo:

{% highlight ruby %}
def tick args
    args.state.racchetta_sinistra ||= { x: 40, y: 300, w: 20, h: 120 }
    args.state.racchetta_destra ||= { x: 1220, y: 300, w: 20, h: 120 }
    args.state.pallina ||= { x: 630, y: 350, w: 20, h: 20, dx: 6, dy: 4 }
    args.state.punti_sinistra ||= 0
    args.state.punti_destra ||= 0

    pallina = args.state.pallina

    # movimento delle racchette
    if args.inputs.keyboard.key_held.w
        args.state.racchetta_sinistra.y += 8
    elsif args.inputs.keyboard.key_held.s
        args.state.racchetta_sinistra.y -= 8
    end

    if args.inputs.keyboard.key_held.up
        args.state.racchetta_destra.y += 8
    elsif args.inputs.keyboard.key_held.down
        args.state.racchetta_destra.y -= 8
    end

    # movimento della pallina
    pallina.x += pallina.dx
    pallina.y += pallina.dy

    # rimbalzo sopra e sotto
    if pallina.y < 0 || pallina.y + pallina.h > 720
        pallina.dy *= -1
    end

    # rimbalzo contro le racchette
    if pallina.intersect_rect?(args.state.racchetta_sinistra) ||
       pallina.intersect_rect?(args.state.racchetta_destra)
        pallina.dx *= -1
    end

    # punteggio
    if pallina.x < 0
        args.state.punti_destra += 1
        pallina.x = 630
        pallina.y = 350
        pallina.dx = 6
    elsif pallina.x > 1280
        args.state.punti_sinistra += 1
        pallina.x = 630
        pallina.y = 350
        pallina.dx = -6
    end

    # disegno
    args.outputs.solids << args.state.racchetta_sinistra.merge(r: 255, g: 255, b: 255)
    args.outputs.solids << args.state.racchetta_destra.merge(r: 255, g: 255, b: 255)
    args.outputs.solids << pallina.merge(r: 255, g: 255, b: 255)
    args.outputs.labels << {
        x: 640,
        y: 690,
        text: "#{args.state.punti_sinistra}   -   #{args.state.punti_destra}",
        anchor_x: 0.5,
        size_px: 40
    }
end
{% endhighlight %}

## Come continuare

Questo Pong è volutamente minimale. Alcuni miglioramenti che si possono provare da soli,
riprendendo le pagine già viste in questa sezione:

* far aumentare leggermente la velocità della pallina ad ogni rimbalzo, per rendere gli
  scambi più lunghi progressivamente più difficili
* aggiungere un suono quando la pallina rimbalza, come visto per i [suoni]({{ site.baseurl }}{% link _ruby/suoni.md %}.html)
* mostrare una schermata di [game over]({{ site.baseurl }}{% link _ruby/scene.md %}.html) quando un giocatore arriva a 5 punti
* impedire alle racchette di uscire dallo schermo, con la stessa tecnica di **clamp** vista
  nella pagina sulla [telecamera]({{ site.baseurl }}{% link _ruby/telecamera.md %}.html)
