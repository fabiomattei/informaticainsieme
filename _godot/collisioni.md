---
title: 'Godot: collisioni tra Area2D'
date: '2026-08-18T11:45:00+01:00'
author: Fabio Mattei
layout: page
---

Per monitorare l'interazione tra i vari elementi sullo schermo dobbiamo monitorare
le loro **collisioni**: se un proiettile tocca un nemico, il nemico deve reagire.
In Dragonruby questo si calcolava manualmente ogni tick con `intersect_rect?`. Godot
affida questo compito al proprio **motore fisico**, attraverso i nodi `Area2D` e
`CollisionShape2D`.

## Costruire un oggetto che rileva le collisioni

Un oggetto capace di accorgersi quando qualcosa lo tocca è composto da un `Area2D`
con un `CollisionShape2D` come figlio, che ne definisce la forma della hitbox.

#### Esercizio 1
Crea una scena con un `Area2D` come radice, aggiungi un figlio `CollisionShape2D` e,
nell'Inspector, assegnagli una nuova risorsa **RectangleShape2D** (o
**CircleShape2D**), impostandone la dimensione.

## Il segnale body_entered / area_entered

Quando un'altra `Area2D` (o un corpo fisico) entra nell'area, viene emesso un
segnale: `area_entered` se si tocca un'altra `Area2D`, `body_entered` se si tocca
un corpo come `CharacterBody2D` o `StaticBody2D`.

{% highlight gdscript %}
extends Area2D

func _ready():
    area_entered.connect(_su_collisione)

func _su_collisione(altra_area):
    print("Ho toccato: %s" % altra_area.name)
    queue_free()    # rimuove questo nodo dalla scena
{% endhighlight %}

A differenza di Dragonruby, dove bisognava controllare `intersect_rect?` per ogni
possibile coppia di sprite ad ogni singolo tick, qui è il motore fisico di Godot a
calcolare le collisioni internamente e ad avvisarci **solo quando succede
qualcosa**, tramite il segnale.

## queue_free(): rimuovere un nodo

`queue_free()` è l'equivalente Godot del pattern "marca come morto e poi rimuovi"
visto in Dragonruby con `reject!`: pianifica la rimozione del nodo alla fine del
frame corrente, evitando i problemi che si avrebbero a rimuoverlo nel mezzo del
calcolo delle fisiche.

## Un esempio completo: proiettile contro nemico

#### Esercizio 2
Crea una scena `Proiettile.tscn` con `Area2D` come radice.

{% highlight gdscript %}
# Proiettile.gd
extends Area2D

func _ready():
    area_entered.connect(_su_collisione)

func _su_collisione(altra_area):
    if altra_area.is_in_group("nemici"):
        altra_area.queue_free()
        queue_free()
{% endhighlight %}

`is_in_group("nemici")` controlla se il nodo colpito appartiene al **gruppo**
"nemici": i gruppi (assegnabili dalla scheda **Node > Groups** nell'editor, oppure
con `add_to_group()` da script) sono il modo standard con cui Godot classifica
oggetti simili, sostituendo il controllo manuale che in Dragonruby si faceva
scorrendo array separati (`args.state.nemici`, `args.state.proiettili`).

## Livelli e maschere di collisione

Per evitare che ogni oggetto rilevi collisioni con tutto (ad esempio, i proiettili
del giocatore non dovrebbero colpire il giocatore stesso), Godot usa i **collision
layer** e le **collision mask**, configurabili nell'Inspector di ogni `Area2D`:
il layer dice "a quale categoria appartengo", la mask dice "quali categorie voglio
rilevare". Impostando layer e mask con cura si evita di dover scrivere, come si
farebbe in Dragonruby, controlli espliciti del tipo "salta se questo è il mio
stesso proiettile".

## Riepilogo: da Dragonruby a Godot

| Dragonruby                                          | Godot equivalente                       |
|--------------------------------------------------------|----------------------------------------------|
| `sprite.intersect_rect?(altro)`                         | segnale `area_entered` / `body_entered`       |
| controllo manuale ogni tick, per ogni coppia di sprite  | calcolato dal motore fisico                    |
| `reject!` per rimuovere elementi morti                  | `queue_free()`                                |
| array separati per tipo (`nemici`, `proiettili`)        | **gruppi** (`is_in_group`, `add_to_group`)    |
| nessun filtro nativo                                     | collision layer / mask                        |

Nella prossima pagina vediamo la [geometria]({{ site.baseurl }}{% link _godot/geometria.md %}.html):
distanze e angoli, utili anche al di fuori delle collisioni.
