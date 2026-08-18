---
title: 'Godot: movimenti ed effetti fluidi (Tween)'
date: '2026-08-18T11:15:00+01:00'
author: Fabio Mattei
layout: page
---

Negli esempi visti finora un nodo si muove di una quantità fissa (moltiplicata per
`delta`) finché un tasto resta premuto: il movimento parte e si ferma di scatto. Per
dare un aspetto più curato ai movimenti e alle transizioni — una Label che compare
con una dissolvenza, uno sprite che si sposta dolcemente da un punto all'altro —
Dragonruby offre `Easing.ease`, che calcola una percentuale di avanzamento nel
tempo. Godot risolve lo stesso problema con un oggetto dedicato: il **Tween**.

## Creare un Tween

Un Tween si crea da script con `create_tween()`, e si programma indicando **quale
proprietà** animare, **verso quale valore finale**, e **in quanto tempo**.

{% highlight gdscript %}
extends Node2D

func _ready():
    var tween = create_tween()
    tween.tween_property(self, "position:x", 1000, 2.0)
{% endhighlight %}

Questa riga anima la proprietà `x` della `position` del nodo corrente (`self`) fino
al valore 1000, impiegando 2 secondi. A differenza di `Easing.ease` in Dragonruby,
non serve calcolare manualmente la percentuale ad ogni frame dentro `_process`:
il Tween aggiorna da solo la proprietà, frame dopo frame, finché non raggiunge il
valore finale.

## Una dissolvenza (fade in)

#### Esercizio 1
Aggiungi una Label con del testo, poi collega questo script al nodo padre.

{% highlight gdscript %}
extends Node2D

func _ready():
    $Label.modulate.a = 0.0    # parte invisibile

    var tween = create_tween()
    tween.tween_property($Label, "modulate:a", 1.0, 2.0)
{% endhighlight %}

Il canale alpha (`a`) di `modulate` passa da 0.0 (invisibile) a 1.0 (opaco) in due
secondi: esattamente l'effetto ottenuto in Dragonruby moltiplicando `255` per la
percentuale calcolata con `Easing.ease`.

## Spostare un oggetto da un punto a un altro

#### Esercizio 2
Copia questo script su un nodo `Sprite2D` o `Node2D`.

{% highlight gdscript %}
extends Node2D

func _ready():
    position = Vector2(100, 300)

    var tween = create_tween()
    tween.tween_property(self, "position", Vector2(1000, 300), 1.5)
{% endhighlight %}

`tween_property` può animare anche un intero `Vector2` in un colpo solo, non solo
un singolo numero: non serve calcolare separatamente `x` e `y` come si farebbe
manualmente.

## Curve diverse per effetti diversi

Come il quarto parametro di `Easing.ease` in Dragonruby, il Tween accetta un tipo di
**transizione** e un **easing**, impostabili con `.set_trans()` e `.set_ease()`
prima di `.tween_property()`.

{% highlight gdscript %}
func _ready():
    var tween = create_tween()
    tween.set_trans(Tween.TRANS_QUAD)
    tween.set_ease(Tween.EASE_OUT)
    tween.tween_property(self, "position:x", 1000, 1.5)
{% endhighlight %}

Le famiglie di curve più comuni sono:

* `Tween.TRANS_LINEAR` — crescita lineare, velocità costante (equivalente a `:identity`)
* `Tween.TRANS_QUAD`, `TRANS_CUBIC`, `TRANS_QUART` — potenze crescenti, come in Dragonruby
* `Tween.TRANS_BOUNCE` — un rimbalzo alla fine del movimento
* `Tween.TRANS_ELASTIC` — un effetto elastico

e si combinano con:

* `Tween.EASE_IN` — l'effetto della curva si applica all'inizio del movimento
* `Tween.EASE_OUT` — l'effetto si applica alla fine (equivalente a `:smooth_stop_quad`)
* `Tween.EASE_IN_OUT` — l'effetto si applica sia all'inizio che alla fine

## Concatenare più animazioni

Chiamare `.tween_property()` più volte sullo stesso Tween mette le animazioni **in
sequenza**, una dopo l'altra: è utile per costruire piccole coreografie, ad esempio
un nemico che si sposta e poi torna indietro.

{% highlight gdscript %}
func _ready():
    var tween = create_tween()
    tween.tween_property(self, "position:x", 1000, 1.0)
    tween.tween_property(self, "position:x", 100, 1.0)
    tween.tween_callback(func(): print("Animazione conclusa"))
{% endhighlight %}

`tween_callback` esegue una funzione al termine della sequenza: è il modo per
sapere quando l'animazione è finita, senza doverlo controllare manualmente
confrontando `Kernel.tick_count` come si farebbe in Dragonruby.
