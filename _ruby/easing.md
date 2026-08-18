---
title: 'Dragonruby: movimenti ed effetti fluidi (easing)'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Negli esempi visti finora un personaggio si muove di una quantità fissa di pixel ad ogni
tick in cui un tasto resta premuto: il movimento parte e si ferma di scatto. Per dare un
aspetto più curato ai movimenti e alle transizioni (una label che compare con una dissolvenza,
un pulsante che si sposta, uno sprite che rimbalza) è utile calcolare una percentuale di
avanzamento nel tempo e usarla per interpolare tra un valore iniziale e un valore finale.
Questa tecnica si chiama **easing**.

## Il metodo Easing.ease

**Easing.ease** calcola quella percentuale, restituendo un valore float compreso tra 0 e 1,
a partire da quattro informazioni: il tick in cui l'animazione è iniziata, il tick corrente,
la durata dell'animazione in numero di tick, e il tipo di curva da applicare.

{% highlight ruby %}
def tick args
    inizio = 60
    durata = 120

    percentuale = Easing.ease(inizio, Kernel.tick_count, durata, :identity)

    args.outputs.labels << {
        x: 640,
        y: 320,
        text: "#{(percentuale * 100).to_i}%"
    }
end
{% endhighlight %}

Con la curva **:identity** la percentuale cresce in modo lineare, cioè costante nel tempo.
Prima che l'animazione inizi la percentuale resta a 0, e una volta superata la durata
indicata resta ferma a 1: non serve nessun controllo aggiuntivo per fermare l'animazione.

## Una dissolvenza (fade in)

Usando la percentuale per calcolare la trasparenza **a** di una label o di uno sprite si
ottiene un effetto di dissolvenza, molto più gradevole di una scritta che appare di colpo.

{% highlight ruby %}
def tick args
    percentuale = Easing.ease(60, Kernel.tick_count, 120, :identity)

    args.outputs.labels << {
        x: 640,
        y: 320,
        text: "Livello 1",
        a: 255 * percentuale
    }
end
{% endhighlight %}

## Spostare un oggetto da un punto a un altro

Lo stesso principio si può usare per calcolare, istante per istante, la posizione di uno
sprite che si deve spostare da un punto di partenza ad un punto di arrivo in un tempo
prefissato.

{% highlight ruby %}
def tick args
    x_iniziale = 100
    x_finale = 1000

    percentuale = Easing.ease(60, Kernel.tick_count, 120, :smooth_start_quad)

    x = x_iniziale + (x_finale - x_iniziale) * percentuale

    args.outputs.sprites << {
        x: x,
        y: 300,
        w: 80,
        h: 80,
        path: 'sprites/dragon-0.png'
    }
end
{% endhighlight %}

## Curve diverse per effetti diversi

Il quarto parametro di **Easing.ease** permette di scegliere come la percentuale cresce nel
tempo, non necessariamente in modo lineare:

* :identity - crescita lineare, velocità costante
* :flip - crescita lineare invertita (parte da 1 e arriva a 0)
* :quad, :cube, :quart, :quint - partenza lenta che accelera (potenze crescenti)
* :smooth_start_quad - variante che rende l'inizio del movimento più dolce
* :smooth_stop_quad - variante che rende la fine del movimento più dolce, come se l'oggetto
  frenasse dolcemente invece di fermarsi di scatto

Provare a cambiare il simbolo passato come ultimo argomento nell'esempio precedente è il modo
più semplice per capire visivamente la differenza tra le varie curve.
