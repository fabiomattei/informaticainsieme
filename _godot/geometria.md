---
title: 'Godot: geometria'
date: '2026-08-18T11:50:00+01:00'
author: Fabio Mattei
layout: page
---

Nella pagina sulle [collisioni]({{ site.baseurl }}{% link _godot/collisioni.md %}.html)
abbiamo visto come Godot rilevi automaticamente il contatto tra oggetti. Il tipo
**Vector2**, usato ovunque per `position`, offre anche metodi utili per calcolare
distanze e direzioni tra i punti del piano — l'equivalente del modulo `Geometry` di
Dragonruby.

## Distanza tra due punti

`.distance_to()` calcola la distanza in pixel tra due `Vector2`.

{% highlight gdscript %}
extends Node2D

func _ready():
    var giocatore = Vector2(0, 0)
    var nemico = Vector2(100, 100)

    var distanza = giocatore.distance_to(nemico)
    print("Distanza: %s" % distanza)
{% endhighlight %}

## Angolo tra due punti

`.angle_to_point()` calcola l'angolo, in **radianti**, che serve per andare dal
primo punto al secondo — a differenza di Dragonruby, dove `angle_to` restituisce
gradi direttamente.

{% highlight gdscript %}
extends Node2D

func _physics_process(delta):
    var nemico = Vector2(0, 0)
    var giocatore = Vector2(100, 100)

    var angolo = nemico.angle_to_point(giocatore)
    rotation = angolo    # rotation vuole radianti, non gradi
{% endhighlight %}

Per convertire tra gradi e radianti, se servono altrove, si usano `rad_to_deg()` e
`deg_to_rad()`.

{% highlight gdscript %}
func _ready():
    print(rad_to_deg(PI))    # 180.0
    print(deg_to_rad(90))    # 1.5707963...
{% endhighlight %}

## Muoversi verso un punto

Per far inseguire il giocatore da un nemico non serve nemmeno calcolare l'angolo
esplicitamente: `Vector2` offre `.direction_to()`, che restituisce già un vettore
di lunghezza 1 puntato verso il bersaglio, pronto da moltiplicare per una velocità.

{% highlight gdscript %}
extends Node2D

var velocita = 150

func _physics_process(delta):
    var giocatore = get_node("../Giocatore").position
    var direzione = position.direction_to(giocatore)
    position += direzione * velocita * delta
{% endhighlight %}

Questo sostituisce, in una sola riga, il calcolo con seno e coseno che serviva in
Dragonruby dopo aver ottenuto l'angolo con `Geometry.angle_to`.

## Altre operazioni utili sui Vector2

| Operazione                    | Significato                                        |
|----------------------------------|-------------------------------------------------------|
| `a.distance_to(b)`               | distanza tra due punti                                 |
| `a.direction_to(b)`              | vettore unitario da `a` verso `b`                      |
| `a.angle_to_point(b)`            | angolo (radianti) da `a` verso `b`                     |
| `a.length()`                     | lunghezza del vettore (distanza dall'origine)          |
| `a.normalized()`                 | lo stesso vettore, ma di lunghezza 1                   |
| `a.lerp(b, t)`                   | punto intermedio tra `a` e `b`, con `t` da 0.0 a 1.0   |
| `a + b`, `a - b`, `a * n`        | somma, differenza, scalatura — operatori diretti       |

`a.lerp(b, t)` in particolare è imparentato con il [Tween]({{ site.baseurl }}{% link _godot/easing.md %}.html)
visto in precedenza: è la stessa idea di interpolazione, ma calcolata in un colpo
solo invece che animata nel tempo.

## Trovare tutte le collisioni in un'area

Dragonruby offre `find_all_intersect_rect` per trovare tutti gli elementi di una
lista che collidono con un rettangolo. In Godot lo stesso risultato si ottiene
interrogando direttamente il motore fisico con `get_overlapping_areas()` o
`get_overlapping_bodies()`, chiamati su un `Area2D`.

{% highlight gdscript %}
extends Area2D

func _physics_process(delta):
    var aree_toccate = get_overlapping_areas()
    for area in aree_toccate:
        if area.is_in_group("nemici"):
            area.queue_free()
{% endhighlight %}

A differenza di Dragonruby, questa lista è già calcolata dal motore fisico:
non serve confrontare manualmente ogni coppia di rettangoli.
