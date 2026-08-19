---
title: 'Dragonruby: pulsanti cliccabili'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Le interfacce dei videogiochi sono fatte da pulsanti o bottoni da premere. 
Nel menu del nostro [stato del gioco]({{ site.baseurl }}{% link _ruby/scene.md %}.html)
abbiamo finora chiesto al giocatore di premere un tasto della tastiera ma sui computer con
mouse, e soprattutto su dispositivi touch, è più naturale offrire dei veri pulsanti da
premere o cliccare. Dragonruby non fornisce un widget "pulsante" già pronto, ma possiamo costruirlo
facilmente con quello che già conosciamo: un rettangolo, una label sovrapposta al rettangolo, e il rilevamento del
click del mouse visto nella pagina sull'[input]({{ site.baseurl }}{% link _ruby/input.md %}.html).

## Disegnare un pulsante

Un pulsante è semplicemente un rettangolo con una scritta sovrapposta al centro.
Definirlo graficamente è una cosa abbastanza semplice.

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

Un click va considerato valido solo se viene effettuato all'interno al rettangolo del pulsante.
Per questo non ci basta monitorare l'evento **button_left** che restituisce True per tutto il tempo
in cui il bottone del mouse viene premuto. 
Noi dobbiamo sapere se il bottone viene premuto quando si trova dentro al rettangolo, per questo
monitoriamo l'evento **key_down** e lo facciamo nel modo seguente:

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

Diamo alla nostra interfaccia un piccolo tocco di originalità facendo in modo che,
quando il puntatore del mouse passa sopra il pulsante, questo cambia di colore, anche se non viene
premuto, così il giocatore capisce che quell'area dello schermo è cliccabile.

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
