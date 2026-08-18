---
title: 'Godot: le Label'
date: '2026-08-18T11:05:00+01:00'
author: Fabio Mattei
layout: page
---

In Dragonruby una label è un dizionario aggiunto a `args.outputs.labels` ad ogni
tick. In Godot il testo a schermo è un **nodo vero e proprio**: `Label`.

## Aggiungere una Label

#### Esercizio 1
Nella Scene dock, seleziona il nodo radice e aggiungi un nodo figlio (**+** in alto
a sinistra, oppure tasto destro > **Add Child Node**) di tipo `Label`. Nell'Inspector
a destra, imposta la proprietà **Text** su "Ciao 4C!" e la posizione (**Position**)
su x: 120, y: 120.

Esegui la scena con F6: il testo compare subito, senza bisogno di scrivere nemmeno
una riga di codice — è già una differenza fondamentale rispetto a Dragonruby, dove
ogni elemento va ridisegnato manualmente ad ogni tick.

## Le proprietà principali

Le proprietà più usate di una Label, modificabili sia dall'Inspector sia da script,
sono:

* `text`: la stringa da mostrare
* `position`: le coordinate (x, y) sul piano, ereditate da `Control`
* `horizontal_alignment` / `vertical_alignment`: l'allineamento del testo rispetto
  al proprio riquadro (equivalente di `anchor_x`/`anchor_y` in Dragonruby)
* `modulate`: un colore (con canale alpha) che tinge il testo, incluso il colore
  bianco per la trasparenza totale
* `label_settings` (o le proprietà **Theme Overrides > Font**): per impostare il
  font e la dimensione del testo

## Modificare il testo da script

Per mostrare informazioni che cambiano nel tempo, come il punteggio, si accede alla
Label dallo script con `$NomeDelNodo` (o salvando un riferimento con `@onready`) e
si aggiorna la sua proprietà `text`.

#### Esercizio 2
Aggiungi una Label chiamata "Punteggio" e collega questo script al nodo radice.

{% highlight gdscript %}
extends Node2D

var punteggio = 0

func _ready():
    aggiorna_testo()

func _process(delta):
    punteggio += 1
    aggiorna_testo()

func aggiorna_testo():
    $Punteggio.text = "Punteggio: %d" % punteggio
{% endhighlight %}

`$Punteggio` è una scorciatoia per ottenere il riferimento al nodo figlio chiamato
"Punteggio": è l'equivalente Godot dell'accesso a `args.state.punteggio` in
Dragonruby, con la differenza che qui si accede a un **nodo dell'albero della
scena**, non a una semplice variabile.

## @onready: salvare il riferimento una sola volta

Scrivere `$Punteggio` ogni volta funziona, ma cercare il nodo nell'albero ha un
piccolo costo. La convenzione GDScript è salvare il riferimento **una sola volta**,
al momento in cui il nodo è pronto, con l'annotazione `@onready`.

{% highlight gdscript %}
extends Node2D

@onready var label_punteggio = $Punteggio
var punteggio = 0

func _process(delta):
    punteggio += 1
    label_punteggio.text = "Punteggio: %d" % punteggio
{% endhighlight %}

`@onready` dice a Godot di assegnare quella variabile **subito prima** che venga
chiamato `_ready()`, quando l'albero della scena è già completamente costruito e il
nodo `$Punteggio` esiste di sicuro.

## Colore e trasparenza

Il colore del testo si imposta con `modulate`, un `Color` con quattro componenti (r,
g, b, a) espresse come float da 0.0 a 1.0 — non da 0 a 255 come in Dragonruby.

{% highlight gdscript %}
func _ready():
    $Punteggio.modulate = Color(1, 0, 0, 1)    # rosso pieno, opaco
    $Punteggio.modulate.a = 0.5                 # solo il canale alpha, semitrasparente
{% endhighlight %}

## Riepilogo: da Dragonruby a Godot

| Dragonruby (dizionario in `args.outputs.labels`) | Godot (nodo `Label`)     |
|-----------------------------------------------------|---------------------------|
| `x`, `y`                                             | `position`                |
| `text`                                               | `text`                    |
| `anchor_x`, `anchor_y`                               | `horizontal_alignment`, `vertical_alignment` |
| `r`, `g`, `b`, `a` (0-255)                           | `modulate` (Color, 0.0-1.0) |
| `size_px`                                            | dimensione del font nel tema |

Nella prossima pagina vediamo l'equivalente per le immagini: gli
[Sprite2D]({{ site.baseurl }}{% link _godot/sprite2d.md %}.html).
