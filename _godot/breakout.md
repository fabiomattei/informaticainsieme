---
title: 'Godot: esempio, Breakout'
date: '2026-08-18T12:30:00+01:00'
author: Fabio Mattei
layout: page
---

Partendo dal [Pong]({{ site.baseurl }}{% link _godot/pong.md %}.html) appena visto
basta sostituire la seconda racchetta con una griglia di mattoncini da distruggere
per ottenere un altro classico: **Breakout** (conosciuto anche come **Arkanoid**).
La racchetta e il rimbalzo della pallina restano identici, cambia solo contro cosa
la pallina rimbalza.

#### Preparazione della scena
Crea una scena con un `Node2D` come radice e un nodo figlio `Label` chiamato
"Punteggio". Collega uno script al nodo radice.

## La racchetta

Una sola racchetta, in basso, che il giocatore muove a sinistra e a destra.

{% highlight gdscript %}
extends Node2D

const LARGHEZZA_SCHERMO = 1280
const ALTEZZA_SCHERMO = 720

var racchetta = Rect2(560, 40, 160, 20)

func _physics_process(delta):
    var direzione = Input.get_axis("ui_left", "ui_right")
    racchetta.position.x += direzione * 480 * delta

    queue_redraw()

func _draw():
    draw_rect(racchetta, Color(1, 1, 1))
{% endhighlight %}

## La pallina

Esattamente come nella [pallina che rimbalza]({{ site.baseurl }}{% link _godot/pallina.md %}.html):
rimbalza sopra, a sinistra e a destra, e rimbalza anche contro la racchetta, come
nel Pong.

{% highlight gdscript %}
var pallina = Rect2(630, 300, 20, 20)
var velocita_pallina = Vector2(240, 300)

func _physics_process(delta):
    pallina.position += velocita_pallina * delta

    if pallina.position.x < 0 or pallina.position.x + pallina.size.x > LARGHEZZA_SCHERMO:
        velocita_pallina.x *= -1

    if pallina.position.y + pallina.size.y > ALTEZZA_SCHERMO:
        velocita_pallina.y *= -1

    if pallina.intersects(racchetta):
        velocita_pallina.y *= -1
{% endhighlight %}

## La griglia di mattoncini

All'inizio della partita generiamo una griglia di mattoncini, un po' come abbiamo
generato i nemici nella pagina sullo [spawn dei nemici]({{ site.baseurl }}{% link _godot/spawn.md %}.html),
ma tutti insieme in un colpo solo, prima ancora che inizi il gioco. Ogni mattoncino
è rappresentato da un piccolo dizionario con un `Rect2` e un colore.

{% highlight gdscript %}
var mattoncini = []

func crea_mattoncini():
    var lista = []
    for riga in range(5):
        for colonna in range(8):
            lista.append({
                "rettangolo": Rect2(40 + colonna * 150, 500 + riga * 35, 140, 25),
                "colore": Color(1.0 - riga * 0.12, 0.5 + riga * 0.08, 0.35)
            })
    return lista

func _ready():
    mattoncini = crea_mattoncini()
{% endhighlight %}

Ogni riga della griglia riceve un colore leggermente diverso, calcolato a partire
dal numero della riga, così la griglia risulta più leggibile a colpo d'occhio.

## Distruggere un mattoncino

Quando la pallina tocca un mattoncino, quel mattoncino va rimosso e la pallina deve
rimbalzare. GDScript non ha un metodo `.find()` con blocco come Ruby: scorriamo
l'array con un ciclo `for` e usciamo con `break` non appena troviamo una collisione.

{% highlight gdscript %}
var punteggio = 0

func _physics_process(delta):
    var indice_colpito = -1
    for i in range(mattoncini.size()):
        if pallina.intersects(mattoncini[i]["rettangolo"]):
            indice_colpito = i
            break

    if indice_colpito != -1:
        mattoncini.remove_at(indice_colpito)
        velocita_pallina.y *= -1
        punteggio += 10
{% endhighlight %}

A differenza degli esempi con molti elementi che collidono contemporaneamente
(come lo [space shooter]({{ site.baseurl }}{% link _godot/sparatutto.md %}.html)),
qui basta `remove_at()` sul primo indice trovato perché ad ogni passo fisico la
pallina può toccare al massimo un mattoncino.

## Vincere e perdere

Il giocatore vince quando l'array dei mattoncini è vuoto, e perde quando la pallina
scende sotto il fondo dello schermo senza che la racchetta l'abbia intercettata.

{% highlight gdscript %}
var finita = false

func _physics_process(delta):
    if pallina.position.y < 0:
        finita = true
    if mattoncini.is_empty():
        finita = true
{% endhighlight %}

## Il gioco completo

{% highlight gdscript %}
extends Node2D

const LARGHEZZA_SCHERMO = 1280
const ALTEZZA_SCHERMO = 720

var racchetta = Rect2(560, 40, 160, 20)
var pallina = Rect2(630, 300, 20, 20)
var velocita_pallina = Vector2(240, 300)
var mattoncini = []
var punteggio = 0
var finita = false

func crea_mattoncini():
    var lista = []
    for riga in range(5):
        for colonna in range(8):
            lista.append({
                "rettangolo": Rect2(40 + colonna * 150, 500 + riga * 35, 140, 25),
                "colore": Color(1.0 - riga * 0.12, 0.5 + riga * 0.08, 0.35)
            })
    return lista

func _ready():
    mattoncini = crea_mattoncini()

func _physics_process(delta):
    if finita:
        queue_redraw()
        return

    # movimento della racchetta
    var direzione = Input.get_axis("ui_left", "ui_right")
    racchetta.position.x += direzione * 480 * delta

    # movimento della pallina
    pallina.position += velocita_pallina * delta

    if pallina.position.x < 0 or pallina.position.x + pallina.size.x > LARGHEZZA_SCHERMO:
        velocita_pallina.x *= -1
    if pallina.position.y + pallina.size.y > ALTEZZA_SCHERMO:
        velocita_pallina.y *= -1
    if pallina.intersects(racchetta):
        velocita_pallina.y *= -1

    # collisione con i mattoncini
    var indice_colpito = -1
    for i in range(mattoncini.size()):
        if pallina.intersects(mattoncini[i]["rettangolo"]):
            indice_colpito = i
            break

    if indice_colpito != -1:
        mattoncini.remove_at(indice_colpito)
        velocita_pallina.y *= -1
        punteggio += 10

    # fine della partita
    finita = mattoncini.is_empty() or pallina.position.y < 0

    $Punteggio.text = "Punteggio: %d" % punteggio
    queue_redraw()

func _draw():
    if finita:
        var testo = "Hai vinto!" if mattoncini.is_empty() else "Game Over"
        draw_string(ThemeDB.fallback_font, Vector2(560, 360), testo, HORIZONTAL_ALIGNMENT_CENTER, -1, 32)
        return

    draw_rect(racchetta, Color(1, 1, 1))
    draw_rect(pallina, Color(1, 1, 1))
    for mattoncino in mattoncini:
        draw_rect(mattoncino["rettangolo"], mattoncino["colore"])
{% endhighlight %}

`draw_string()` è il modo per scrivere del testo direttamente dentro `_draw()`,
senza usare un nodo `Label`: utile per messaggi temporanei come questo, che
compaiono solo a fine partita.

## Come continuare

* far ripartire la pallina da una posizione casuale sopra la racchetta invece di
  terminare subito la partita, tenendo un contatore di vite come nello
  [space shooter]({{ site.baseurl }}{% link _godot/sparatutto.md %}.html)
* aggiungere delle particelle quando un mattoncino si rompe, come visto nella
  pagina sulle [particelle]({{ site.baseurl }}{% link _godot/particelle.md %}.html)
* salvare il [punteggio più alto]({{ site.baseurl }}{% link _godot/salvataggio.md %}.html)
  ottenuto tra una partita e l'altra
* trasformare il messaggio finale in una vera schermata di
  [game over]({{ site.baseurl }}{% link _godot/scene.md %}.html)
