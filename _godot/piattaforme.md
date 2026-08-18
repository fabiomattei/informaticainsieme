---
title: 'Godot: esempio, un platform con salto'
date: '2026-08-18T12:45:00+01:00'
author: Fabio Mattei
layout: page
---

Nessuno degli esempi visti finora ha ancora usato la **gravità**: nella
[pallina che rimbalza]({{ site.baseurl }}{% link _godot/pallina.md %}.html) e nei
giochi che ne derivano la velocità verticale resta sempre costante, e cambia segno
soltanto quando si tocca un bordo. In un gioco a piattaforme, invece, la velocità
verticale del personaggio aumenta di continuo verso il basso, proprio come nella
realtà, ed è quello che permette di saltare e poi ricadere seguendo una traiettoria
curva.

#### Preparazione della scena
Crea una scena con un `Node2D` come radice e collegaci uno script.

## La gravità

Aggiungiamo al giocatore una velocità verticale, che ad ogni passo fisico aumenta
di una piccola quantità costante verso il basso: è esattamente la gravità. Ricorda
che in Godot l'asse Y **cresce verso il basso**, quindi "cadere" significa
**aumentare** `dy`, non diminuirlo come in Dragonruby.

{% highlight gdscript %}
extends Node2D

const GRAVITA = 960.0    # pixel al secondo, al quadrato

var giocatore = Rect2(100, 100, 50, 60)
var dy = 0.0

func _physics_process(delta):
    dy += GRAVITA * delta
    giocatore.position.y += dy * delta

    queue_redraw()

func _draw():
    draw_rect(giocatore, Color(0.3, 0.6, 1))
{% endhighlight %}

Provando questo codice da solo il personaggio cadrebbe all'infinito, sempre più
veloce, dato che nulla lo ferma: ci servono delle piattaforme su cui atterrare.

## Le piattaforme

Le piattaforme sono semplici rettangoli, come i
[solidi]({{ site.baseurl }}{% link _godot/forme.md %}.html) già visti: una grande
piattaforma per il terreno, e alcune più piccole per creare dei livelli da
scalare.

{% highlight gdscript %}
var piattaforme = [
    Rect2(0, 680, 1280, 40),      # il terreno (vicino al fondo, Y grande)
    Rect2(300, 500, 200, 30),
    Rect2(700, 370, 200, 30),
    Rect2(200, 250, 200, 30)
]

func _draw():
    for p in piattaforme:
        draw_rect(p, Color(0.35, 0.25, 0.15))
{% endhighlight %}

## Atterrare sulle piattaforme

Il giocatore deve fermarsi non appena tocca la parte superiore di una piattaforma
mentre sta cadendo, invece di attraversarla. Controlliamo la collisione con
`.intersects()`, ma solo se il giocatore si sta muovendo verso il basso
(**dy >= 0**, dato che qui "verso il basso" è positivo): questo evita che,
saltando verso l'alto, venga fermato dal fondo della piattaforma invece di poterci
passare sopra in un salto successivo.

{% highlight gdscript %}
var a_terra = false

func _physics_process(delta):
    a_terra = false
    for p in piattaforme:
        if giocatore.intersects(p) and dy >= 0:
            giocatore.position.y = p.position.y - giocatore.size.y
            dy = 0
            a_terra = true
{% endhighlight %}

`a_terra` ricorda se il giocatore in questo momento sta poggiando su una
piattaforma: ci servirà per decidere se può saltare oppure no.

## Saltare

Un salto è semplicemente un'improvvisa velocità verticale **negativa** (verso
l'alto, dato l'asse Y invertito), che verrà poi progressivamente ridotta a zero e
invertita dalla gravità, dando la classica traiettoria a parabola. Si può saltare
solo se in questo momento si è `a_terra`.

{% highlight gdscript %}
func _physics_process(delta):
    if a_terra and Input.is_action_just_pressed("ui_accept"):
        dy = -420
{% endhighlight %}

## Muoversi a sinistra e a destra

Il movimento orizzontale non è influenzato dalla gravità, quindi resta identico a
quello visto nella pagina sull'[input]({{ site.baseurl }}{% link _godot/input.md %}.html).

{% highlight gdscript %}
func _physics_process(delta):
    var direzione = Input.get_axis("ui_left", "ui_right")
    giocatore.position.x += direzione * 360 * delta
{% endhighlight %}

## Il gioco completo

{% highlight gdscript %}
extends Node2D

const GRAVITA = 960.0

var giocatore = Rect2(100, 100, 50, 60)
var dy = 0.0
var a_terra = false

var piattaforme = [
    Rect2(0, 680, 1280, 40),
    Rect2(300, 500, 200, 30),
    Rect2(700, 370, 200, 30),
    Rect2(200, 250, 200, 30)
]

func _physics_process(delta):
    # movimento orizzontale
    var direzione = Input.get_axis("ui_left", "ui_right")
    giocatore.position.x += direzione * 360 * delta

    # gravità
    dy += GRAVITA * delta
    giocatore.position.y += dy * delta

    # atterraggio sulle piattaforme
    a_terra = false
    for p in piattaforme:
        if giocatore.intersects(p) and dy >= 0:
            giocatore.position.y = p.position.y - giocatore.size.y
            dy = 0
            a_terra = true

    # salto
    if a_terra and Input.is_action_just_pressed("ui_accept"):
        dy = -420

    # se cade fuori dallo schermo, ricomincia dall'inizio
    if giocatore.position.y > 900:
        giocatore.position = Vector2(100, 100)
        dy = 0

    queue_redraw()

func _draw():
    for p in piattaforme:
        draw_rect(p, Color(0.35, 0.25, 0.15))
    draw_rect(giocatore, Color(0.3, 0.6, 1))
{% endhighlight %}

## Come continuare

* far seguire il giocatore dalla [telecamera]({{ site.baseurl }}{% link _godot/telecamera.md %}.html)
  invece di tenerla ferma, per costruire un livello molto più lungo di un solo
  schermo
* aggiungere delle monete da raccogliere, con la stessa tecnica del cibo vista in
  [Snake]({{ site.baseurl }}{% link _godot/snake.md %}.html)
* aggiungere dei nemici che si muovono avanti e indietro su una piattaforma, come
  nella pagina sullo [spawn dei nemici]({{ site.baseurl }}{% link _godot/spawn.md %}.html)
* far muovere una piattaforma avanti e indietro nel tempo usando un
  [Tween]({{ site.baseurl }}{% link _godot/easing.md %}.html), per creare
  piattaforme mobili
