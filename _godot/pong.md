---
title: 'Godot: esempio, Pong'
date: '2026-08-18T12:25:00+01:00'
author: Fabio Mattei
layout: page
---

Come anticipato nella pagina sulla [pallina che rimbalza]({{ site.baseurl }}{% link _godot/pallina.md %}.html),
lo stesso principio usato per farla rimbalzare sui bordi dello schermo basta, con
una piccola aggiunta, a costruire uno dei videogiochi più famosi della storia:
**Pong**. Ci servono soltanto due racchette che il giocatore controlla con la
tastiera, e la pallina che deve rimbalzare anche contro di loro.

#### Preparazione della scena
Crea una scena con un `Node2D` come radice e un nodo figlio `Label` chiamato
"Punteggio". Collega uno script al nodo radice.

## Le racchette

Rappresentiamo ogni racchetta con un `Rect2`, che raggruppa posizione e dimensione
in un solo valore — l'equivalente del dizionario `{x:, y:, w:, h:}` usato in
Dragonruby. Usiamo i tasti **W** e **S** per la racchetta di sinistra, e le
freccette **su** e **giù** per la racchetta di destra, come già visto per l'
[input]({{ site.baseurl }}{% link _godot/input.md %}.html).

{% highlight gdscript %}
extends Node2D

const LARGHEZZA_SCHERMO = 1280
const ALTEZZA_SCHERMO = 720

var racchetta_sinistra = Rect2(40, 300, 20, 120)
var racchetta_destra = Rect2(1220, 300, 20, 120)

func _physics_process(delta):
    if Input.is_key_pressed(KEY_W):
        racchetta_sinistra.position.y -= 480 * delta
    elif Input.is_key_pressed(KEY_S):
        racchetta_sinistra.position.y += 480 * delta

    if Input.is_action_pressed("ui_up"):
        racchetta_destra.position.y -= 480 * delta
    elif Input.is_action_pressed("ui_down"):
        racchetta_destra.position.y += 480 * delta

    queue_redraw()

func _draw():
    draw_rect(racchetta_sinistra, Color(1, 1, 1))
    draw_rect(racchetta_destra, Color(1, 1, 1))
{% endhighlight %}

## La pallina, come nell'esempio precedente

Riprendiamo la pallina che si muove e rimbalza sopra e sotto, esattamente come
nella pagina precedente: cambia solo che, invece di rimbalzare anche a sinistra e a
destra, quando la pallina supera il bordo sinistro o destro significa che una
racchetta non è arrivata in tempo, quindi la pallina è persa.

{% highlight gdscript %}
var pallina = Rect2(630, 350, 20, 20)
var velocita_pallina = Vector2(360, 240)

func _physics_process(delta):
    pallina.position += velocita_pallina * delta

    if pallina.position.y < 0 or pallina.position.y + pallina.size.y > ALTEZZA_SCHERMO:
        velocita_pallina.y *= -1
{% endhighlight %}

## Rimbalzare contro le racchette

`Rect2` offre il metodo `.intersects()`, l'equivalente diretto di `intersect_rect?`
in Dragonruby, per capire se la pallina ha toccato una racchetta. In quel caso
invertiamo la componente orizzontale della velocità.

{% highlight gdscript %}
func _physics_process(delta):
    pallina.position += velocita_pallina * delta

    if pallina.position.y < 0 or pallina.position.y + pallina.size.y > ALTEZZA_SCHERMO:
        velocita_pallina.y *= -1

    if pallina.intersects(racchetta_sinistra) or pallina.intersects(racchetta_destra):
        velocita_pallina.x *= -1
{% endhighlight %}

## Il punteggio

Quando la pallina esce completamente dallo schermo a sinistra o a destra, un
giocatore ha segnato un punto contro l'altro, e la pallina va rimessa al centro.

{% highlight gdscript %}
var punti_sinistra = 0
var punti_destra = 0

func _physics_process(delta):
    if pallina.position.x < 0:
        punti_destra += 1
        pallina.position = Vector2(630, 350)
        velocita_pallina.x = 360
    elif pallina.position.x > LARGHEZZA_SCHERMO:
        punti_sinistra += 1
        pallina.position = Vector2(630, 350)
        velocita_pallina.x = -360

    $Punteggio.text = "%d   -   %d" % [punti_sinistra, punti_destra]
{% endhighlight %}

## Il gioco completo

Mettendo insieme tutti i pezzi visti sopra otteniamo il gioco completo:

{% highlight gdscript %}
extends Node2D

const LARGHEZZA_SCHERMO = 1280
const ALTEZZA_SCHERMO = 720

var racchetta_sinistra = Rect2(40, 300, 20, 120)
var racchetta_destra = Rect2(1220, 300, 20, 120)
var pallina = Rect2(630, 350, 20, 20)
var velocita_pallina = Vector2(360, 240)
var punti_sinistra = 0
var punti_destra = 0

func _physics_process(delta):
    # movimento delle racchette
    if Input.is_key_pressed(KEY_W):
        racchetta_sinistra.position.y -= 480 * delta
    elif Input.is_key_pressed(KEY_S):
        racchetta_sinistra.position.y += 480 * delta

    if Input.is_action_pressed("ui_up"):
        racchetta_destra.position.y -= 480 * delta
    elif Input.is_action_pressed("ui_down"):
        racchetta_destra.position.y += 480 * delta

    # movimento della pallina
    pallina.position += velocita_pallina * delta

    # rimbalzo sopra e sotto
    if pallina.position.y < 0 or pallina.position.y + pallina.size.y > ALTEZZA_SCHERMO:
        velocita_pallina.y *= -1

    # rimbalzo contro le racchette
    if pallina.intersects(racchetta_sinistra) or pallina.intersects(racchetta_destra):
        velocita_pallina.x *= -1

    # punteggio
    if pallina.position.x < 0:
        punti_destra += 1
        pallina.position = Vector2(630, 350)
        velocita_pallina.x = 360
    elif pallina.position.x > LARGHEZZA_SCHERMO:
        punti_sinistra += 1
        pallina.position = Vector2(630, 350)
        velocita_pallina.x = -360

    $Punteggio.text = "%d   -   %d" % [punti_sinistra, punti_destra]
    queue_redraw()

func _draw():
    draw_rect(racchetta_sinistra, Color(1, 1, 1))
    draw_rect(racchetta_destra, Color(1, 1, 1))
    draw_rect(pallina, Color(1, 1, 1))
{% endhighlight %}

## Come continuare

Questo Pong è volutamente minimale. Alcuni miglioramenti che si possono provare da
soli, riprendendo le pagine già viste in questa sezione:

* far aumentare leggermente la velocità della pallina ad ogni rimbalzo
* aggiungere un suono quando la pallina rimbalza, come visto per i
  [suoni]({{ site.baseurl }}{% link _godot/suoni.md %}.html)
* mostrare una vera schermata di [game over]({{ site.baseurl }}{% link _godot/scene.md %}.html)
  quando un giocatore arriva a 5 punti
* impedire alle racchette di uscire dallo schermo, con `clamp()` come visto nella
  pagina sulla [telecamera]({{ site.baseurl }}{% link _godot/telecamera.md %}.html)
