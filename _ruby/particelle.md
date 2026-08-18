---
title: 'Dragonruby: un semplice sistema di particelle'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Un'esplosione, una scia di polvere dietro ai piedi del personaggio, delle scintille quando
un proiettile colpisce un muro: questi effetti si ottengono generando al momento giusto un
gruppo di piccoli elementi grafici, chiamati **particelle**, che si muovono per un tempo
breve e poi scompaiono. Non serve nessuna libreria: un sistema di particelle è semplicemente
un array nello stato del gioco, gestito con le tecniche che abbiamo già visto.

## Una particella è un dizionario

Ogni particella ricorda la propria posizione, la propria velocità e per quanti tick ancora
deve restare visibile.

{% highlight ruby %}
def crea_particella x, y
    {
        x: x,
        y: y,
        dx: (rand - 0.5) * 6,   # velocità orizzontale casuale
        dy: rand * 6,           # velocità verticale casuale
        vita: 30                # tick rimanenti prima di scomparire
    }
end
{% endhighlight %}

**rand** restituisce un numero casuale float compreso tra 0 e 1: sottraendo 0.5 otteniamo
un numero compreso tra -0.5 e 0.5, utile per far schizzare le particelle sia a sinistra che
a destra in modo imprevedibile.

## Generare un'esplosione

Quando accade l'evento che deve generare l'effetto, ad esempio la distruzione di un nemico,
creiamo tante particelle tutte nello stesso punto.

{% highlight ruby %}
def tick args
    args.state.particelle ||= []

    if args.inputs.keyboard.key_down.space
        20.times do
            args.state.particelle << crea_particella(640, 360)
        end
    end
end
{% endhighlight %}

## Aggiornare e disegnare le particelle

Ad ogni tick ciascuna particella si sposta in base alla propria velocità e perde un punto
di vita. Quando la vita arriva a 0 la particella va rimossa dall'array, esattamente come
abbiamo fatto nella pagina sulle
[collisioni]({{ site.baseurl }}{% link _ruby/collisioni.md %}.html) per gli sprite distrutti.

{% highlight ruby %}
def tick args
    args.state.particelle.each do |particella|
        particella.x += particella.dx
        particella.y += particella.dy
        particella.vita -= 1
    end

    args.state.particelle.reject! { |particella| particella.vita <= 0 }

    args.state.particelle.each do |particella|
        args.outputs.solids << {
            x: particella.x,
            y: particella.y,
            w: 4,
            h: 4,
            r: 255,
            g: 128,
            b: 0,
            a: particella.vita * 8  # svanisce progressivamente
        }
    end
end
{% endhighlight %}

Moltiplicando la trasparenza **a** per il valore di **vita** otteniamo un effetto di
dissolvenza: la particella diventa sempre più trasparente man mano che si avvicina alla
propria scomparsa, invece di sparire di colpo. Per una dissolvenza ancora più controllata si
può usare la tecnica dell'
[easing]({{ site.baseurl }}{% link _ruby/easing.md %}.html) vista in precedenza.
