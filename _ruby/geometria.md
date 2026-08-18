---
title: 'Dragonruby: geometria'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Nella pagina sulle [collisioni]({{ site.baseurl }}{% link _ruby/collisioni.md %}.html) abbiamo
visto **intersect_rect?** per capire se due sprite si toccano. Il modulo **Geometry** di
dragonruby offre anche altri metodi utili per calcolare distanze e direzioni tra i punti
del piano cartesiano, indispensabili ad esempio per far inseguire il giocatore da un nemico.

## Distanza tra due punti

**distance** calcola la distanza in pixel tra due punti, ciascuno rappresentato da un
dizionario con le chiavi x e y.

{% highlight ruby %}
def tick args
    giocatore = { x: 0, y: 0 }
    nemico = { x: 100, y: 100 }

    distanza = Geometry.distance(giocatore, nemico)

    args.outputs.labels << { x: 30, y: 30, text: "Distanza: #{distanza}" }
end
{% endhighlight %}

## Angolo tra due punti

**angle_to** calcola l'angolo, in gradi, che serve per andare dal primo punto al secondo.
E' l'informazione di cui abbiamo bisogno per far ruotare un nemico verso il giocatore, o
per far muovere un proiettile lungo una direzione.

{% highlight ruby %}
def tick args
    nemico = { x: 0, y: 0 }
    giocatore = { x: 100, y: 100 }

    angolo = Geometry.angle_to(nemico, giocatore) # 45 gradi

    args.outputs.sprites << {
        x: nemico.x,
        y: nemico.y,
        w: 80,
        h: 80,
        path: 'sprites/nemico.png',
        angle: angolo
    }
end
{% endhighlight %}

Esiste anche la variante **mixin**, che si chiama direttamente sul dizionario del punto di
partenza:

{% highlight ruby %}
angolo = nemico.angle_to(giocatore)
{% endhighlight %}

## Muoversi verso un punto

Conoscendo l'angolo possiamo scomporlo in uno spostamento sull'asse x e uno spostamento
sull'asse y, usando le funzioni trigonometriche **seno e coseno**. E' quello che serve per
far inseguire il giocatore da un nemico ad una data velocità.

{% highlight ruby %}
def tick args
    args.state.nemico ||= { x: 0, y: 0 }
    velocita = 3

    angolo = Geometry.angle_to(args.state.nemico, args.state.giocatore)

    args.state.nemico.x += velocita * Math.cos(angolo.to_radians)
    args.state.nemico.y += velocita * Math.sin(angolo.to_radians)
end
{% endhighlight %}

## Trovare tutte le collisioni

**intersect_rect?** risponde solo true o false su una singola coppia di rettangoli. Quando
un proiettile deve essere controllato contro una lista intera di nemici, è più comodo
**find_all_intersect_rect**, che restituisce direttamente tutti gli elementi della lista che
collidono con il rettangolo indicato.

{% highlight ruby %}
def tick args
    args.state.nemici ||= [
        { x: 0, y: 0, w: 100, h: 100 },
        { x: 200, y: 200, w: 100, h: 100 }
    ]

    proiettile = { x: 50, y: 50, w: 20, h: 20 }

    nemici_colpiti = Geometry.find_all_intersect_rect(proiettile, args.state.nemici)

    nemici_colpiti.each { |nemico| nemico.morto = true }
end
{% endhighlight %}
