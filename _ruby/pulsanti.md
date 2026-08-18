---
title: 'Dragonruby: pulsanti cliccabili'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Nel menu del nostro [stato del gioco]({{ site.baseurl }}{% link _ruby/scene.md %}.html)
abbiamo finora chiesto al giocatore di premere un tasto della tastiera. Su computer con
mouse, e soprattutto su dispositivi touch, è più naturale offrire dei veri pulsanti da
cliccare. Dragonruby non fornisce un widget "pulsante" già pronto, ma possiamo costruirlo
facilmente con quello che già conosciamo: un rettangolo, una label, e il rilevamento del
click del mouse visto nella pagina sull'[input]({{ site.baseurl }}{% link _ruby/input.md %}.html).

## Disegnare un pulsante

Un pulsante è semplicemente un rettangolo con una scritta sovrapposta al centro.

{% highlight ruby %}
def disegna_pulsante args, pulsante
    args.outputs.solids << {
        x: pulsante.x,
        y: pulsante.y,
        w: pulsante.w,
        h: pulsante.h,
        r: 60,
        g: 60,
        b: 90
    }

    args.outputs.labels << {
        x: pulsante.x + pulsante.w / 2,
        y: pulsante.y + pulsante.h / 2,
        text: pulsante.testo,
        anchor_x: 0.5,
        anchor_y: 0.5,
        r: 255,
        g: 255,
        b: 255
    }
end
{% endhighlight %}

## Capire se il pulsante è stato cliccato

Un click va considerato valido solo se accade nell'istante stesso in cui il tasto del mouse
viene premuto (**key_down**, non semplicemente **button_left**, che resterebbe vero per
tutta la durata della pressione) e se in quell'istante il puntatore si trova dentro al
rettangolo del pulsante.

{% highlight ruby %}
def pulsante_cliccato? args, pulsante
    args.inputs.mouse.key_down.left &&
        args.inputs.mouse.inside_rect?(pulsante.x, pulsante.y, pulsante.w, pulsante.h)
end
{% endhighlight %}

## Un pulsante "Gioca" nel menu

Uniamo i due metodi per costruire il pulsante che fa iniziare la partita.

{% highlight ruby %}
def tick_menu args
    args.state.pulsante_gioca ||= { x: 540, y: 300, w: 200, h: 80, testo: "Gioca" }

    disegna_pulsante args, args.state.pulsante_gioca

    if pulsante_cliccato? args, args.state.pulsante_gioca
        args.state.scena = :gioco
        args.state.punteggio = 0
    end
end
{% endhighlight %}

## Un effetto al passaggio del mouse

Un piccolo tocco che rende l'interfaccia più viva è cambiare il colore del pulsante quando
il puntatore ci passa sopra, anche senza cliccare, così il giocatore capisce che quell'area
dello schermo è cliccabile.

{% highlight ruby %}
def disegna_pulsante args, pulsante
    sopra_il_pulsante = args.inputs.mouse.inside_rect?(pulsante.x, pulsante.y, pulsante.w, pulsante.h)

    args.outputs.solids << {
        x: pulsante.x,
        y: pulsante.y,
        w: pulsante.w,
        h: pulsante.h,
        r: sopra_il_pulsante ? 90 : 60,
        g: sopra_il_pulsante ? 90 : 60,
        b: sopra_il_pulsante ? 130 : 90
    }

    args.outputs.labels << {
        x: pulsante.x + pulsante.w / 2,
        y: pulsante.y + pulsante.h / 2,
        text: pulsante.testo,
        anchor_x: 0.5,
        anchor_y: 0.5
    }
end
{% endhighlight %}
