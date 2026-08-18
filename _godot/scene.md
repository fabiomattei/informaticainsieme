---
title: 'Godot: stato del gioco e cambio scena'
date: '2026-08-18T12:10:00+01:00'
author: Fabio Mattei
layout: page
---

Un videogioco vero non è composto da una sola schermata: c'è tipicamente un menu
iniziale, la schermata di gioco, e una schermata di game over. In Dragonruby,
dove esiste un solo file eseguito in loop, queste "scene" erano semplici funzioni
diverse selezionate con una variabile di stato (`args.state.scena`). In Godot il
concetto di scena è **nativo**: ogni schermata è davvero un file `.tscn` separato,
e Godot offre funzioni dedicate per passare dall'una all'altra.

## Cambiare scena

{% highlight gdscript %}
extends Node2D

func _su_gioca_premuto():
    get_tree().change_scene_to_file("res://gioco.tscn")
{% endhighlight %}

`get_tree().change_scene_to_file()` scarica completamente la scena corrente dalla
memoria e ne carica un'altra al suo posto. A differenza di Dragonruby, dove
cambiare `args.state.scena` lasciava tutto lo stato precedente ancora in memoria
(dentro lo stesso dizionario `args.state`), qui **ogni scena riparte da zero**: le
variabili degli script della scena precedente vengono perse insieme ai suoi nodi.

## Conservare dati tra una scena e l'altra: un Autoload

Per portare informazioni da una scena all'altra — il punteggio, il nome del
giocatore, il livello raggiunto — serve un posto che **non venga distrutto** al
cambio di scena. È esattamente il ruolo dell'**Autoload** (singleton), già visto
nella pagina sulla [visibilità delle variabili]({{ site.baseurl }}{% link _godot/visibilita-variabili.md %}.html):
uno script registrato in **Project > Project Settings > Autoload**, che resta
sempre attivo per tutta la durata del gioco, indipendentemente da quale scena sia
aperta.

#### Esercizio 1
Crea uno script `gestore_gioco.gd` e registralo come Autoload con il nome
"GestoreGioco".

{% highlight gdscript %}
# gestore_gioco.gd
extends Node

var punteggio = 0
var vite = 3

func azzera_partita():
    punteggio = 0
    vite = 3
{% endhighlight %}

Da qualsiasi altra scena, questi dati sono accessibili scrivendo semplicemente
`GestoreGioco.punteggio`, senza bisogno di passarli esplicitamente da una scena
all'altra:

{% highlight gdscript %}
# nel menu
func _su_gioca_premuto():
    GestoreGioco.azzera_partita()
    get_tree().change_scene_to_file("res://gioco.tscn")
{% endhighlight %}

{% highlight gdscript %}
# nella scena di gioco
func _ready():
    $Punteggio.text = "Punteggio: %d" % GestoreGioco.punteggio

func aggiungi_punti(n):
    GestoreGioco.punteggio += n
    $Punteggio.text = "Punteggio: %d" % GestoreGioco.punteggio
{% endhighlight %}

## Il menu

Nel menu mostriamo una Label e un Button (già visti nella pagina sui
[pulsanti cliccabili]({{ site.baseurl }}{% link _godot/pulsanti.md %}.html)) che
avvia la partita.

{% highlight gdscript %}
# Menu.gd
extends Node2D

func _ready():
    $PulsanteGioca.pressed.connect(_su_gioca_premuto)

func _su_gioca_premuto():
    GestoreGioco.azzera_partita()
    get_tree().change_scene_to_file("res://gioco.tscn")
{% endhighlight %}

## Il gioco

Nella scena di gioco mettiamo tutto quello che abbiamo visto nelle sezioni
precedenti: input, sprite, collisioni. Quando l'evento che fa terminare la partita
si verifica (ad esempio il giocatore che perde tutte le vite), cambiamo scena.

{% highlight gdscript %}
# Gioco.gd
extends Node2D

func _process(delta):
    $EtichettaStato.text = "Punteggio: %d   Vite: %d" % [GestoreGioco.punteggio, GestoreGioco.vite]

    if GestoreGioco.vite <= 0:
        get_tree().change_scene_to_file("res://game_over.tscn")
{% endhighlight %}

## Il game over

Nella schermata di game over mostriamo il punteggio finale e permettiamo di
tornare al menu.

{% highlight gdscript %}
# GameOver.gd
extends Node2D

func _ready():
    $EtichettaPunteggio.text = "Game Over - punteggio: %d" % GestoreGioco.punteggio
    $PulsanteMenu.pressed.connect(_su_menu_premuto)

func _su_menu_premuto():
    get_tree().change_scene_to_file("res://menu.tscn")
{% endhighlight %}

Questo è anche il punto giusto per verificare se il punteggio della partita appena
conclusa è un nuovo record, e in caso affermativo salvarlo su file: vedi la pagina
su [come salvare e caricare i dati]({{ site.baseurl }}{% link _godot/salvataggio.md %}.html).

## Riepilogo: da Dragonruby a Godot

| Dragonruby                                             | Godot equivalente                                 |
|-------------------------------------------------------------|--------------------------------------------------------|
| `args.state.scena` + `case/when` in un unico file            | scene `.tscn` separate + `change_scene_to_file()`       |
| `args.state` (persiste sempre, unico dizionario globale)      | ogni scena riparte da zero: serve un **Autoload** per condividere dati |
| `tick_menu`, `tick_gioco`, `tick_game_over` come funzioni      | `Menu.gd`, `Gioco.gd`, `GameOver.gd` come script separati |
