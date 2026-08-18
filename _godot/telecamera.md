---
title: 'Godot: la telecamera (Camera2D)'
date: '2026-08-18T12:05:00+01:00'
author: Fabio Mattei
layout: page
---

Un livello di gioco è spesso molto più grande dello schermo. In Dragonruby la
telecamera andava implementata a mano, sottraendo la sua posizione dalle coordinate
di ogni sprite prima di disegnarlo. Godot offre un nodo pronto: **Camera2D**.

## Aggiungere una telecamera

#### Esercizio 1
Aggiungi un nodo `Camera2D` come figlio del nodo del giocatore. Nell'Inspector,
attiva la proprietà **Enabled** (o **Current** nelle versioni precedenti a Godot
4.3).

{% highlight gdscript %}
extends CharacterBody2D

func _ready():
    $Camera2D.enabled = true
{% endhighlight %}

Da questo momento, la vista del gioco segue automaticamente la posizione del nodo
padre della telecamera: non serve calcolare nessuna sottrazione manuale come si
faceva in Dragonruby con `sprite.x - args.state.telecamera.x`. Godot ridisegna da
solo tutta la scena nel punto giusto rispetto alla telecamera.

## Telecamera indipendente dal giocatore

Se la telecamera non deve essere figlia diretta del giocatore (ad esempio per
poterla muovere con una logica diversa), si può comunque tenerla sincronizzata da
script.

{% highlight gdscript %}
extends Camera2D

@onready var giocatore = get_node("../Giocatore")

func _process(delta):
    global_position = giocatore.global_position
{% endhighlight %}

## Un effetto più morbido: smoothing

A differenza dell'esempio Dragonruby, dove la telecamera segue il giocatore in
modo rigido, `Camera2D` offre un'opzione integrata per un movimento più morbido:
la proprietà **Position Smoothing** (`position_smoothing_enabled` da script), che
fa "rincorrere" il bersaglio invece di teletrasportarsi istantaneamente sopra di
esso.

{% highlight gdscript %}
func _ready():
    $Camera2D.position_smoothing_enabled = true
    $Camera2D.position_smoothing_speed = 5.0
{% endhighlight %}

## Limitare la telecamera ai confini del livello

Come visto in Dragonruby con `.clamp()`, anche qui si vogliono evitare aree vuote
oltre i bordi del livello. `Camera2D` offre quattro proprietà dedicate,
**Limit Left/Right/Top/Bottom**, impostabili anche da script.

{% highlight gdscript %}
func _ready():
    $Camera2D.limit_left = 0
    $Camera2D.limit_right = 4000
    $Camera2D.limit_top = 0
    $Camera2D.limit_bottom = 720
{% endhighlight %}

Con questi limiti impostati, quando il giocatore si avvicina al bordo del livello
la telecamera si ferma da sola, esattamente come otteneva Dragonruby con
`.clamp(0, larghezza_livello - 1280)`, ma senza bisogno di scriverlo esplicitamente
ogni frame.

## Zoom

Un'altra funzionalità che Dragonruby non offriva nativamente è lo **zoom**: un
`Vector2` che scala la visuale. Valori minori di 1 avvicinano l'inquadratura,
valori maggiori la allontanano.

{% highlight gdscript %}
func _ready():
    $Camera2D.zoom = Vector2(2, 2)    # inquadratura più ravvicinata
{% endhighlight %}

## Riepilogo: da Dragonruby a Godot

| Dragonruby (calcolato a mano)                             | Godot equivalente                        |
|-----------------------------------------------------------------|------------------------------------------------|
| sottrarre `telecamera.x/y` da ogni sprite prima di disegnare      | nodo `Camera2D`, gestito automaticamente        |
| nessun equivalente diretto                                        | **Position Smoothing** per un movimento morbido |
| `.clamp(0, larghezza_livello - 1280)` ricalcolato ogni tick        | **Limit Left/Right/Top/Bottom**                 |
| nessun equivalente diretto                                        | `zoom`                                          |
