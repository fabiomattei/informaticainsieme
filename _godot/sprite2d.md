---
title: 'Godot: gli Sprite2D'
date: '2026-08-18T11:10:00+01:00'
author: Fabio Mattei
layout: page
---

Gli sprite sono gli elementi grafici che possiamo utilizzare nel nostro videogioco.
In Dragonruby uno sprite è un dizionario ridisegnato ogni tick; in Godot è un nodo,
**Sprite2D**, con delle proprietà che si impostano una volta e restano valide finché
non vengono cambiate.

## Aggiungere uno Sprite2D

#### Esercizio 1
Importa un'immagine nel progetto trascinandola nella FileSystem dock. Aggiungi un
nodo figlio di tipo `Sprite2D`, e nell'Inspector trascina l'immagine nella proprietà
**Texture**. Imposta `position` a x: 120, y: 280.

## Le proprietà principali

{% highlight gdscript %}
func _ready():
    position = Vector2(0, 0)
    rotation_degrees = 90
    modulate = Color(1, 1, 1, 1)     # r, g, b, a da 0.0 a 1.0
    flip_h = false
    flip_v = false
    scale = Vector2(1, 1)
{% endhighlight %}

* `position`: le coordinate (x, y), un `Vector2`
* `rotation_degrees`: l'angolo di rotazione in gradi (o `rotation` in radianti)
* `modulate`: colore e trasparenza (equivalente di r, g, b, a in Dragonruby, ma con
  valori da 0.0 a 1.0 invece che da 0 a 255)
* `flip_h` / `flip_v`: capovolgono l'immagine orizzontalmente o verticalmente
  (equivalenti a `flip_horizontally`/`flip_vertically`)
* `scale`: un `Vector2` che ingrandisce o rimpicciolisce lo sprite

## Anchoring: l'offset

Ogni Sprite2D ha di default il proprio punto di ancoraggio al **centro**
dell'immagine — non in basso a sinistra come in Dragonruby. Per cambiarlo si usa la
proprietà `offset`, oppure si spunta `centered = false` per riportarlo all'angolo in
alto a sinistra dell'immagine.

{% highlight gdscript %}
func _ready():
    centered = false    # punto di ancoraggio in alto a sinistra, invece che al centro
{% endhighlight %}

## Sprite sheet: AtlasTexture e region

Per ritagliare una singola immagine (un fotogramma) da un foglio più grande —
quello che in Dragonruby si otteneva con `tile_x`, `tile_y`, `tile_w`, `tile_h` — si
usa la proprietà `region_enabled` insieme a `region_rect`.

{% highlight gdscript %}
func _ready():
    region_enabled = true
    region_rect = Rect2(0, 0, 32, 32)    # x, y, larghezza, altezza del ritaglio
{% endhighlight %}

## Animare uno sprite: AnimatedSprite2D

Per l'animazione, invece di cambiare manualmente la `texture` di uno Sprite2D ad
ogni frame (tecnica comunque possibile, identica a quella vista per Dragonruby),
Godot offre un nodo dedicato: **AnimatedSprite2D**, che gestisce da solo la
sequenza di fotogrammi a partire da una risorsa **SpriteFrames**.

#### Esercizio 2
Aggiungi un nodo `AnimatedSprite2D`, crea una nuova risorsa **SpriteFrames**
nell'Inspector, aggiungi un'animazione (ad esempio chiamata "corsa") e trascina
dentro i fotogrammi in ordine.

{% highlight gdscript %}
extends Node2D

func _ready():
    $AnimatedSprite2D.play("corsa")
{% endhighlight %}

`.play("corsa")` avvia il loop automatico dell'animazione, con la velocità (fotogrammi
al secondo) impostata nella risorsa SpriteFrames: non serve calcolare a mano, come
si faceva in Dragonruby con `Kernel.tick_count / 6`, quale fotogramma mostrare.

## Cambiare sprite da script

Se invece serve cambiare l'immagine di un semplice `Sprite2D` (ad esempio in base
allo stato del personaggio), si assegna una nuova risorsa `Texture2D` alla proprietà
`texture`.

{% highlight gdscript %}
extends Sprite2D

@export var texture_normale: Texture2D
@export var texture_colpito: Texture2D

func mostra_colpito():
    texture = texture_colpito
{% endhighlight %}

`@export` rende la variabile visibile e modificabile dall'Inspector, senza dover
scrivere il percorso del file nel codice: è utile per collegare risorse (immagini,
suoni, altre scene) direttamente dall'editor.

## Riepilogo: da Dragonruby a Godot

| Dragonruby (dizionario in `args.outputs.sprites`) | Godot (nodo `Sprite2D`)         |
|-------------------------------------------------------|-----------------------------------|
| `x`, `y`                                               | `position`                        |
| `w`, `h`                                               | dimensione della texture × `scale`|
| `path`                                                 | `texture`                          |
| `angle`                                                | `rotation_degrees`                 |
| `a`, `r`, `g`, `b` (0-255)                             | `modulate` (Color, 0.0-1.0)        |
| `flip_horizontally`, `flip_vertically`                 | `flip_h`, `flip_v`                 |
| `tile_x/y/w/h`                                         | `region_rect` (con `region_enabled = true`) |
| animazione manuale con array di path                   | nodo `AnimatedSprite2D` + `SpriteFrames` |
