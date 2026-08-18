---
title: 'Dragonruby: esempio, la pallina che rimbalza'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Prima di andare avanti con i singoli argomenti, vediamo un piccolo esempio completo che
mette insieme quello che abbiamo visto nell'[introduzione]({{ site.baseurl }}{% link _ruby/sprites.md %}.html):
il **tick**, lo **stato del gioco** e l'**output**. L'obiettivo è disegnare una pallina che
si muove sullo schermo e che rimbalza quando arriva ad un bordo, un po' come il classico
logo di un lettore DVD che rimbalza tra gli angoli.

## Posizione e velocità

Oltre alla posizione (x, y) la pallina ha bisogno di una **velocità**, cioè di quanti pixel
si sposta ad ogni tick lungo l'asse x e lungo l'asse y. Chiamiamo questi due valori **dx** e
**dy** (la lettera d sta per "delta", cioè "variazione").

{% highlight ruby %}
def tick args
    args.state.pallina ||= {
        x: 600,
        y: 300,
        w: 40,
        h: 40,
        dx: 6,
        dy: 4
    }
end
{% endhighlight %}

## Muovere la pallina

Ad ogni tick aggiungiamo la velocità alla posizione: è esattamente lo stesso principio
visto per il movimento del drago nella pagina sull'[input]({{ site.baseurl }}{% link _ruby/input.md %}.html),
solo che qui la pallina si muove sempre da sola, senza bisogno che il giocatore prema nessun
tasto.

{% highlight ruby %}
def tick args
    args.state.pallina ||= { x: 600, y: 300, w: 40, h: 40, dx: 6, dy: 4 }
    pallina = args.state.pallina

    pallina.x += pallina.dx
    pallina.y += pallina.dy
end
{% endhighlight %}

## Rimbalzare sui bordi dello schermo

Il piano cartesiano del nostro gioco è largo 1280 pixel e alto 720, come visto
nell'introduzione. La pallina deve rimbalzare quando il suo bordo sinistro esce a sinistra
(x minore di 0), quando il suo bordo destro esce a destra (x + w maggiore di 1280), e in
modo analogo su y per l'alto e il basso.

Rimbalzare significa semplicemente **invertire il segno della velocità**: se la pallina si
stava muovendo verso destra (dx positivo), dopo il rimbalzo si muoverà verso sinistra (dx
negativo), e viceversa.

{% highlight ruby %}
def tick args
    args.state.pallina ||= { x: 600, y: 300, w: 40, h: 40, dx: 6, dy: 4 }
    pallina = args.state.pallina

    pallina.x += pallina.dx
    pallina.y += pallina.dy

    if pallina.x < 0 || pallina.x + pallina.w > 1280
        pallina.dx *= -1
    end

    if pallina.y < 0 || pallina.y + pallina.h > 720
        pallina.dy *= -1
    end

    args.outputs.solids << {
        x: pallina.x,
        y: pallina.y,
        w: pallina.w,
        h: pallina.h,
        r: 255,
        g: 80,
        b: 80
    }
end
{% endhighlight %}

Abbiamo disegnato la pallina con un semplice **solid** rosso, visto nella pagina sulle
[forme e primitive grafiche]({{ site.baseurl }}{% link _ruby/forme.md %}.html): dragonruby
non disegna cerchi, quindi in un gioco vero si userebbe uno sprite con l'immagine di una
pallina rotonda al posto del quadrato, semplicemente sostituendo **args.outputs.solids**
con **args.outputs.sprites** e aggiungendo la proprietà **path**.

## Un tocco in più: cambiare colore ad ogni rimbalzo

Per rendere l'esempio un po' più vivace possiamo far cambiare colore alla pallina ogni
volta che rimbalza, invece di lasciarla sempre rossa.

{% highlight ruby %}
def tick args
    args.state.pallina ||= {
        x: 600, y: 300, w: 40, h: 40, dx: 6, dy: 4,
        r: 255, g: 80, b: 80
    }
    pallina = args.state.pallina

    pallina.x += pallina.dx
    pallina.y += pallina.dy

    rimbalzata = false

    if pallina.x < 0 || pallina.x + pallina.w > 1280
        pallina.dx *= -1
        rimbalzata = true
    end

    if pallina.y < 0 || pallina.y + pallina.h > 720
        pallina.dy *= -1
        rimbalzata = true
    end

    if rimbalzata
        pallina.r = rand(256)
        pallina.g = rand(256)
        pallina.b = rand(256)
    end

    args.outputs.solids << {
        x: pallina.x,
        y: pallina.y,
        w: pallina.w,
        h: pallina.h,
        r: pallina.r,
        g: pallina.g,
        b: pallina.b
    }
end
{% endhighlight %}

Questo piccolo esempio, anche se molto semplice, contiene già tutti gli ingredienti di base
di un videogioco: uno stato che si aggiorna nel tempo, delle regole che reagiscono a delle
condizioni (in questo caso i bordi dello schermo), e un output che cambia di conseguenza.
Sostituendo i bordi dello schermo con un altro sprite, come visto nella pagina sulle
[collisioni]({{ site.baseurl }}{% link _ruby/collisioni.md %}.html), lo stesso principio
serve a far rimbalzare una palla contro una racchetta, come nel classico gioco Pong.
