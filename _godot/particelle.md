---
title: 'Godot: un semplice sistema di particelle'
date: '2026-08-18T11:55:00+01:00'
author: Fabio Mattei
layout: page
---

Un'esplosione, una scia di polvere, delle scintille quando un proiettile colpisce
un muro: in Dragonruby questi effetti si costruivano a mano con un array di
dizionari nello stato del gioco. Godot offre un nodo dedicato, già pronto all'uso:
**CPUParticles2D** (o la sua variante più veloce ma meno flessibile,
**GPUParticles2D**).

## Aggiungere un emettitore di particelle

#### Esercizio 1
Aggiungi un nodo `CPUParticles2D` alla scena. Nell'Inspector, imposta:

* **Amount**: quante particelle vengono generate in un ciclo (es. 20)
* **Lifetime**: per quanti secondi resta visibile ciascuna particella
* **One Shot**: `true` se l'effetto deve avvenire una sola volta (un'esplosione),
  `false` se deve continuare (una scia)
* **Emitting**: `true`/`false` per attivare o disattivare l'emissione

## Generare un'esplosione da script

{% highlight gdscript %}
extends Node2D

func _ready():
    $CPUParticles2D.emitting = false

func esplodi_in(posizione):
    $CPUParticles2D.position = posizione
    $CPUParticles2D.restart()
{% endhighlight %}

`.restart()` fa ripartire l'emissione da capo: utile per riutilizzare lo stesso
nodo particelle in più punti diversi nel tempo, invece di crearne uno nuovo ogni
volta come si farebbe aggiungendo elementi a un array in Dragonruby.

## Personalizzare velocità e direzione

Le proprietà più usate per definire il comportamento di ciascuna particella sono
raggruppate nella sezione **Direction / Spread / Initial Velocity**
dell'Inspector, oppure impostabili da script:

{% highlight gdscript %}
func _ready():
    $CPUParticles2D.direction = Vector2(0, -1)   # verso l'alto
    $CPUParticles2D.spread = 45.0                 # angolo di dispersione in gradi
    $CPUParticles2D.initial_velocity_min = 100.0
    $CPUParticles2D.initial_velocity_max = 200.0
    $CPUParticles2D.gravity = Vector2(0, 300)     # le particelle ricadono verso il basso
{% endhighlight %}

Questo corrisponde, in Dragonruby, ad assegnare a mano `dx` e `dy` casuali a
ciascuna particella creata: qui il motore particellare lo fa già internamente per
ciascuna delle particelle generate.

## Dissolvenza nel tempo: la Color Ramp

In Dragonruby la dissolvenza si otteneva moltiplicando manualmente la trasparenza
per la vita rimanente della particella. Il `CPUParticles2D` offre lo stesso
risultato impostando una **Color Ramp** (nella sezione **Color** dell'Inspector):
un gradiente che va, ad esempio, da arancione opaco a trasparente, applicato
automaticamente durante la vita di ogni particella.

## Un emettitore per ogni esplosione, o uno riusabile?

Per un gioco con molte esplosioni contemporanee in punti diversi (ad esempio uno
[space shooter]({{ site.baseurl }}{% link _godot/sparatutto.md %}.html)), conviene
creare una piccola scena `Esplosione.tscn` con dentro un `CPUParticles2D` in
modalità **One Shot**, che si autodistrugge alla fine dell'emissione.

{% highlight gdscript %}
# Esplosione.gd
extends CPUParticles2D

func _ready():
    emitting = true
    finished.connect(queue_free)
{% endhighlight %}

Questa scena si istanzia dinamicamente ogni volta che serve un'esplosione, esattamente
nel punto dove è avvenuta la collisione:

{% highlight gdscript %}
const ESPLOSIONE = preload("res://Esplosione.tscn")

func crea_esplosione(posizione):
    var esplosione = ESPLOSIONE.instantiate()
    esplosione.position = posizione
    get_parent().add_child(esplosione)
{% endhighlight %}

`preload()` carica la scena una sola volta all'avvio; `.instantiate()` ne crea una
nuova copia ogni volta che viene chiamato. Il segnale `finished`, emesso quando
l'emissione "one shot" termina, elimina automaticamente il nodo con `queue_free()`
— lo stesso pattern di rimozione visto nella pagina sulle
[collisioni]({{ site.baseurl }}{% link _godot/collisioni.md %}.html), qui applicato
alle particelle invece che agli sprite morti.

## Riepilogo: da Dragonruby a Godot

| Dragonruby (array di dizionari a mano)         | Godot equivalente                    |
|----------------------------------------------------|------------------------------------------|
| creare N dizionari con `dx`, `dy`, `vita`           | nodo `CPUParticles2D` con **Amount**       |
| aggiornare posizione ad ogni tick                    | gestito automaticamente dal nodo           |
| dissolvenza calcolata a mano su `vita`               | **Color Ramp**                             |
| `reject!` quando `vita <= 0`                         | **One Shot** + segnale `finished` + `queue_free()` |
