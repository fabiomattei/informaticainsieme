---
title: 'Godot: forme e primitive grafiche'
date: '2026-08-18T11:20:00+01:00'
author: Fabio Mattei
layout: page
---

Oltre alle Label e agli Sprite2D, Godot permette di disegnare direttamente delle
forme geometriche di base, senza dover caricare nessuna immagine. Sono utili per
prototipare velocemente un gioco, disegnare barre di vita o hitbox durante il
debug — esattamente come `args.outputs.solids`, `borders` e `lines` in Dragonruby.

Godot offre due strade: nodi già pronti (`ColorRect`, `Line2D`) oppure disegno
diretto con la funzione `_draw()`.

## La via più semplice: ColorRect

**ColorRect** è un nodo che disegna un rettangolo pieno di un colore, con posizione e
dimensione impostabili dall'Inspector o da script.

{% highlight gdscript %}
extends ColorRect

func _ready():
    color = Color(0, 1, 0, 1)     # verde pieno, opaco
    size = Vector2(100, 100)
    position = Vector2(0, 0)
{% endhighlight %}

## Solidi, bordi e linee con _draw()

Per un controllo più fine, o per disegnare più forme dallo stesso nodo, si usa la
funzione speciale `_draw()`: Godot la richiama automaticamente ogni volta che il
nodo deve essere ridisegnato.

### Rettangoli pieni (equivalente di solids)

{% highlight gdscript %}
extends Node2D

func _draw():
    draw_rect(Rect2(0, 0, 100, 100), Color(0, 1, 0, 1))
{% endhighlight %}

`Rect2(x, y, w, h)` definisce posizione e dimensione, esattamente come le chiavi
`x, y, w, h` di un dizionario in Dragonruby. Il colore va da 0.0 a 1.0 invece che da
0 a 255.

### Rettangoli vuoti (equivalente di borders)

Per disegnare solo il contorno, senza riempimento, si passa `false` come quarto
parametro (oppure si passa uno spessore invece di `true`/riempimento pieno).

{% highlight gdscript %}
extends Node2D

func _draw():
    draw_rect(Rect2(100, 100, 160, 90), Color(1, 1, 1, 1), false, 2.0)
{% endhighlight %}

Il parametro `false` disattiva il riempimento, `2.0` è lo spessore del contorno in
pixel.

### Linee

{% highlight gdscript %}
extends Node2D

func _draw():
    draw_line(Vector2(100, 100), Vector2(150, 150), Color(0, 0, 0, 1), 2.0)
{% endhighlight %}

`draw_line(punto_iniziale, punto_finale, colore, spessore)` disegna un segmento,
proprio come le chiavi `x, y, x2, y2` di Dragonruby, ma con i due punti già
raggruppati in `Vector2`.

### Cerchi (novità rispetto a Dragonruby)

Dragonruby non disegna cerchi nativamente. Godot invece offre `draw_circle`
direttamente:

{% highlight gdscript %}
extends Node2D

func _draw():
    draw_circle(Vector2(200, 200), 40, Color(1, 0, 0, 1))
{% endhighlight %}

## Forzare il ridisegno

`_draw()` viene chiamata da Godot solo quando **necessario** (ad esempio la prima
volta, o dopo un ridimensionamento): se una forma deve cambiare in base allo stato
del gioco (ad esempio una barra di vita che si accorcia), bisogna chiedere
esplicitamente a Godot di richiamare `_draw()` con `queue_redraw()`.

{% highlight gdscript %}
extends Node2D

var vita = 100

func _process(delta):
    queue_redraw()    # richiede un nuovo _draw() al prossimo frame

func _draw():
    draw_rect(Rect2(20, 700, vita * 2, 20), Color(1, 0, 0, 1))
{% endhighlight %}

Questa è una differenza importante rispetto a Dragonruby: lì ogni frame ridisegna
sempre tutto da zero dentro `tick`; in Godot, disegnare con `_draw()` è
un'operazione **su richiesta**, pensata per essere più efficiente quando la grafica
non cambia ad ogni singolo frame.

## Riepilogo: da Dragonruby a Godot

| Dragonruby                          | Godot equivalente                              |
|----------------------------------------|---------------------------------------------------|
| `args.outputs.solids`                  | `draw_rect(rect, colore)` oppure nodo `ColorRect` |
| `args.outputs.borders`                 | `draw_rect(rect, colore, false, spessore)`         |
| `args.outputs.lines`                   | `draw_line(inizio, fine, colore, spessore)`        |
| colore 0-255                           | `Color` con componenti 0.0-1.0                      |
| ridisegno automatico ad ogni tick      | ridisegno su richiesta con `queue_redraw()`         |
| —                                       | `draw_circle()` (nessun equivalente in Dragonruby)  |
