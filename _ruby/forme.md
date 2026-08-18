---
title: 'Dragonruby: forme e primitive grafiche'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Oltre alle labels e agli sprites, dragonruby permette di disegnare direttamente delle forme
geometriche di base, senza dover caricare nessuna immagine. Sono utili per prototipare
velocemente un gioco, disegnare lo sfondo, le barre di vita o le hitbox durante il debug.

Le liste da usare sono:

* args.outputs.solids: rettangoli pieni
* args.outputs.borders: rettangoli vuoti, solo il contorno
* args.outputs.lines: segmenti

## Solidi (solids)

Un rettangolo pieno si definisce con x, y, w, h per la posizione e le dimensioni, e r, g, b, a
per il colore e la trasparenza (valori da 0 a 255).

{% highlight ruby %}
def tick args
    args.outputs.solids << {
        x: 0,
        y: 0,
        w: 100,
        h: 100,
        r: 0,
        g: 255,
        b: 0,
        a: 255
    }
end
{% endhighlight %}

Esiste anche una forma più compatta che utilizza un array al posto del dizionario:

{% highlight ruby %}
def tick args
    # [x, y, w, h, r, g, b, a]
    args.outputs.solids << [0, 0, 100, 100, 0, 255, 0, 255]
end
{% endhighlight %}

## Bordi (borders)

I bordi funzionano esattamente come i solidi, ma disegnano soltanto il contorno del
rettangolo, lasciando l'interno trasparente. Sono utili ad esempio per disegnare la hitbox
di un personaggio mentre stiamo testando il gioco.

{% highlight ruby %}
def tick args
    args.outputs.borders << {
        x: 100,
        y: 100,
        w: 160,
        h: 90,
        r: 255,
        g: 255,
        b: 255,
        a: 255
    }
end
{% endhighlight %}

## Linee (lines)

Una linea si definisce con il punto di partenza (x, y) e il punto di arrivo (x2, y2), oltre
al colore.

{% highlight ruby %}
def tick args
    args.outputs.lines << {
        x: 100,
        y: 100,
        x2: 150,
        y2: 150,
        r: 0,
        g: 0,
        b: 0,
        a: 255
    }
end
{% endhighlight %}

## Perché sono utili

Queste primitive non servono solo per creare grafica "vera" nel gioco finito: sono
soprattutto uno strumento di lavoro. Prima di avere gli sprite disegnati da un artista si
può prototipare tutto il gameplay con dei semplici rettangoli colorati, e in fase di debug
si possono sovrapporre dei borders alle hitbox degli sprite per verificare visivamente se le
collisioni vengono calcolate correttamente.
