---
title: 'Godot: esempio, la pallina che rimbalza'
date: '2026-08-18T12:20:00+01:00'
author: Fabio Mattei
layout: page
---

Prima di andare avanti con i singoli argomenti, vediamo un piccolo esempio completo
che mette insieme quello che abbiamo visto nell'[interfaccia e primo script]({{ site.baseurl }}{% link _godot/introduzione.md %}.html):
il game loop, lo stato e il disegno con [forme e primitive grafiche]({{ site.baseurl }}{% link _godot/forme.md %}.html).
L'obiettivo è disegnare una pallina che si muove sullo schermo e rimbalza quando
arriva a un bordo — lo stesso classico logo di un lettore DVD già visto per
Dragonruby.

#### Preparazione della scena
Crea una nuova scena con un nodo `Node2D` come radice e collegaci uno script.

## Posizione e velocità

Oltre alla posizione, la pallina ha bisogno di una **velocità**: di quanti pixel si
sposta ad ogni frame lungo l'asse x e lungo l'asse y. In Godot le due coordinate si
raggruppano naturalmente in un solo `Vector2`, invece delle chiavi separate `dx` e
`dy` usate in Dragonruby.

{% highlight gdscript %}
extends Node2D

const LARGHEZZA_SCHERMO = 1280
const ALTEZZA_SCHERMO = 720
const RAGGIO = 20

var posizione = Vector2(600, 300)
var velocita = Vector2(360, 240)    # pixel al secondo, non pixel a frame
{% endhighlight %}

## Muovere la pallina

Ad ogni passo fisico aggiungiamo la velocità, moltiplicata per `delta`, alla
posizione — esattamente lo stesso principio visto per il movimento con l'
[input]({{ site.baseurl }}{% link _godot/input.md %}.html), solo che qui la pallina
si muove sempre da sola.

{% highlight gdscript %}
func _physics_process(delta):
    posizione += velocita * delta
    queue_redraw()

func _draw():
    draw_circle(posizione, RAGGIO, Color(1, 0.3, 0.3))
{% endhighlight %}

`queue_redraw()` è necessario ogni volta che la posizione cambia: senza di esso,
`_draw()` non verrebbe richiamata automaticamente ad ogni frame, come visto nella
pagina sulle [forme e primitive grafiche]({{ site.baseurl }}{% link _godot/forme.md %}.html).

## Rimbalzare sui bordi dello schermo

L'area di gioco è larga 1280 e alta 720 pixel. La pallina deve rimbalzare quando il
suo bordo esce da uno dei quattro lati dello schermo.

Rimbalzare significa semplicemente **invertire il segno della velocità**: se la
pallina si stava muovendo verso destra (velocita.x positiva), dopo il rimbalzo si
muoverà verso sinistra, e viceversa.

{% highlight gdscript %}
func _physics_process(delta):
    posizione += velocita * delta

    if posizione.x - RAGGIO < 0 or posizione.x + RAGGIO > LARGHEZZA_SCHERMO:
        velocita.x *= -1

    if posizione.y - RAGGIO < 0 or posizione.y + RAGGIO > ALTEZZA_SCHERMO:
        velocita.y *= -1

    queue_redraw()

func _draw():
    draw_circle(posizione, RAGGIO, Color(1, 0.3, 0.3))
{% endhighlight %}

A differenza di Dragonruby, che non disegna cerchi nativamente e avrebbe richiesto
uno sprite o un quadrato al posto della pallina rotonda, Godot offre `draw_circle`
direttamente: la forma risulta già corretta senza bisogno di un'immagine.

## Un tocco in più: cambiare colore ad ogni rimbalzo

Per rendere l'esempio un po' più vivace possiamo far cambiare colore alla pallina
ogni volta che rimbalza.

{% highlight gdscript %}
extends Node2D

const LARGHEZZA_SCHERMO = 1280
const ALTEZZA_SCHERMO = 720
const RAGGIO = 20

var posizione = Vector2(600, 300)
var velocita = Vector2(360, 240)
var colore = Color(1, 0.3, 0.3)

func _physics_process(delta):
    posizione += velocita * delta

    var rimbalzata = false

    if posizione.x - RAGGIO < 0 or posizione.x + RAGGIO > LARGHEZZA_SCHERMO:
        velocita.x *= -1
        rimbalzata = true

    if posizione.y - RAGGIO < 0 or posizione.y + RAGGIO > ALTEZZA_SCHERMO:
        velocita.y *= -1
        rimbalzata = true

    if rimbalzata:
        colore = Color(randf(), randf(), randf())

    queue_redraw()

func _draw():
    draw_circle(posizione, RAGGIO, colore)
{% endhighlight %}

`randf()` restituisce un numero decimale casuale tra 0.0 e 1.0: è l'equivalente
diretto di `rand` in Dragonruby, e qui è comodo perché i colori Godot usano proprio
la scala 0.0-1.0.

Questo piccolo esempio, anche se molto semplice, contiene già tutti gli ingredienti
di base di un videogioco: uno stato che si aggiorna nel tempo, delle regole che
reagiscono a delle condizioni (in questo caso i bordi dello schermo), e un disegno
che cambia di conseguenza. Sostituendo i bordi dello schermo con un altro
rettangolo, come vedremo nella pagina su [Pong]({{ site.baseurl }}{% link _godot/pong.md %}.html),
lo stesso principio serve a far rimbalzare una palla contro una racchetta.
