---
title: 'Godot: far apparire i nemici nel tempo'
date: '2026-08-18T12:00:00+01:00'
author: Fabio Mattei
layout: page
---

Un gioco diventa interessante quando la difficoltà aumenta man mano che si
prosegue. In Dragonruby questo si otteneva contando i tick con l'operatore modulo.
Godot offre un nodo dedicato a intervalli di tempo: **Timer**.

## Un nemico ogni N secondi

#### Esercizio 1
Aggiungi un nodo `Timer` alla scena. Nell'Inspector imposta **Wait Time** a 1.5
(secondi) e spunta **Autostart**.

{% highlight gdscript %}
extends Node2D

const NEMICO = preload("res://Nemico.tscn")

func _ready():
    $Timer.timeout.connect(_su_timer_scaduto)

func _su_timer_scaduto():
    var nemico = NEMICO.instantiate()
    nemico.position = Vector2(1280, randi_range(0, 720))
    add_child(nemico)
{% endhighlight %}

Il segnale `timeout` viene emesso automaticamente ogni volta che passano i secondi
impostati in **Wait Time** (se **Autostart** è attivo, o dopo aver chiamato
`.start()`): non serve controllare manualmente
`Kernel.tick_count % 90 == 0` come in Dragonruby, perché il Timer gestisce da solo
il proprio conteggio del tempo, indipendentemente dal framerate.

`randi_range(0, 720)` genera un numero intero casuale nell'intervallo indicato,
l'equivalente di `rand(720)` in Dragonruby.

## Aumentare la difficoltà nel tempo

Per far comparire i nemici sempre più spesso, si cambia il tempo di attesa del
Timer stesso durante la partita, invece di ricalcolare un intervallo dentro il game
loop.

{% highlight gdscript %}
extends Node2D

const NEMICO = preload("res://Nemico.tscn")
var tempo_trascorso = 0.0

func _ready():
    $Timer.timeout.connect(_su_timer_scaduto)

func _process(delta):
    tempo_trascorso += delta
    # il tempo di attesa scende da 1.5 a 0.3 secondi nel corso della partita
    $Timer.wait_time = clamp(1.5 - tempo_trascorso * 0.01, 0.3, 1.5)

func _su_timer_scaduto():
    var nemico = NEMICO.instantiate()
    nemico.position = Vector2(1280, randi_range(0, 720))
    add_child(nemico)
{% endhighlight %}

`clamp(valore, minimo, massimo)` blocca il valore dentro un intervallo, esattamente
come `.clamp()` in Dragonruby.

## Muovere ed eliminare i nemici usciti dallo schermo

Ogni nemico creato deve muoversi ed eliminarsi da solo quando esce dallo schermo:
si scrive nel proprio script, non nello script dello spawner.

{% highlight gdscript %}
# Nemico.gd
extends Area2D

var velocita = 240

func _physics_process(delta):
    position.x -= velocita * delta
    if position.x < -60:
        queue_free()
{% endhighlight %}

Qui `queue_free()` sostituisce il `reject!` visto in Dragonruby: ogni nemico si
occupa da solo della propria rimozione, invece di essere ripulito da un ciclo
esterno sull'intero array.

## Un'ondata di nemici invece di uno alla volta

Per la tecnica delle **ondate** (waves), basta creare più nemici nello stesso
callback del Timer, invece di uno solo.

{% highlight gdscript %}
extends Node2D

const NEMICO = preload("res://Nemico.tscn")
var numero_ondata = 0

func _ready():
    $Timer.wait_time = 5.0
    $Timer.timeout.connect(_su_timer_scaduto)

func _su_timer_scaduto():
    numero_ondata += 1
    for i in range(3 + numero_ondata):
        var nemico = NEMICO.instantiate()
        nemico.position = Vector2(1280, 60 + i * 80)
        add_child(nemico)
{% endhighlight %}

Ogni ondata, oltre ad avvenire ogni 5 secondi, contiene un nemico in più della
precedente — stesso principio di Dragonruby, con il vantaggio che il Timer gestisce
autonomamente lo scorrere del tempo.

## Riepilogo: da Dragonruby a Godot

| Dragonruby                                             | Godot equivalente                        |
|-------------------------------------------------------------|------------------------------------------------|
| `Kernel.tick_count % intervallo == 0`                        | nodo `Timer` + segnale `timeout`                |
| `rand(1200)`                                                  | `randi_range(0, 1200)`                          |
| `.clamp(min, max)`                                           | funzione globale `clamp(valore, min, max)`      |
| `nemici.reject! { |n| n.x < -60 }` in un ciclo esterno        | ogni nemico chiama `queue_free()` su se stesso  |
