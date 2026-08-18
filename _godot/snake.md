---
title: 'Godot: esempio, Snake'
date: '2026-08-18T12:40:00+01:00'
author: Fabio Mattei
layout: page
---

Gli esempi visti finora rappresentano ogni oggetto con la sua posizione in pixel.
**Snake** introduce un'idea diversa: il gioco si svolge su una **griglia**, cioè lo
schermo è diviso in caselle quadrate della stessa dimensione, e ogni oggetto occupa
esattamente una casella. Il serpente stesso è rappresentato come un **array di
caselle**: la prima entry è la testa, tutte le altre sono la coda.

#### Preparazione della scena
Crea una scena con un `Node2D` come radice e un nodo figlio `Label` chiamato
"Punteggio". Collega uno script al nodo radice.

## La griglia

Scegliamo una dimensione per ogni casella, ad esempio 40 pixel, e calcoliamo quante
caselle ci stanno nello schermo largo 1280 e alto 720 pixel.

{% highlight gdscript %}
extends Node2D

const CASELLA = 40
const COLONNE = 1280 / CASELLA    # 32
const RIGHE = 720 / CASELLA        # 18
{% endhighlight %}

## Il corpo del serpente

Il corpo è un array di `Vector2`, ciascuno con le coordinate espresse **in
caselle**, non in pixel: il segmento con coordinate (2, 5) occupa la terza casella
da sinistra e la sesta dall'alto, indipendentemente da quanto grande sia una
casella.

{% highlight gdscript %}
var corpo = [Vector2(16, 9), Vector2(15, 9), Vector2(14, 9)]
var direzione = Vector2(1, 0)

func _draw():
    for segmento in corpo:
        draw_rect(Rect2(segmento * CASELLA, Vector2(CASELLA - 2, CASELLA - 2)), Color(0, 0.8, 0))
{% endhighlight %}

Solo al momento di disegnare moltiplichiamo le coordinate per la dimensione della
casella, per ottenere le coordinate in pixel richieste da `draw_rect`.

## Cambiare direzione

La direzione è anch'essa un `Vector2`, ma con valori limitati a -1, 0 oppure 1:
indica di quante caselle si sposta la testa del serpente ad ogni passo.
Impediamo di invertire direttamente la direzione, altrimenti la testa andrebbe a
sbattere immediatamente contro il primo segmento del corpo.

{% highlight gdscript %}
func _physics_process(delta):
    if Input.is_action_just_pressed("ui_up") and direzione.y == 0:
        direzione = Vector2(0, -1)
    elif Input.is_action_just_pressed("ui_down") and direzione.y == 0:
        direzione = Vector2(0, 1)
    elif Input.is_action_just_pressed("ui_left") and direzione.x == 0:
        direzione = Vector2(-1, 0)
    elif Input.is_action_just_pressed("ui_right") and direzione.x == 0:
        direzione = Vector2(1, 0)
{% endhighlight %}

Da notare: qui l'asse Y **cresce verso il basso** (come spiegato nella pagina
sull'[interfaccia]({{ site.baseurl }}{% link _godot/introduzione.md %}.html)), il
contrario di Dragonruby: premere la freccia su corrisponde quindi a `Vector2(0,
-1)`, non `(0, 1)`.

## Muoversi sulla griglia

Se il serpente si spostasse di una casella ad ogni passo fisico (60 volte al
secondo), sarebbe troppo veloce per essere giocabile. Lo facciamo avanzare solo
una volta ogni 8 passi fisici, usando `Engine.get_physics_frames()`: un contatore
globale sempre crescente, l'equivalente diretto di `Kernel.tick_count` in
Dragonruby.

Muoversi significa aggiungere una nuova testa nella direzione corrente in cima
all'array, e togliere l'ultimo segmento della coda, a meno che il serpente non
debba crescere.

{% highlight gdscript %}
var crescere = false

func _physics_process(delta):
    if Engine.get_physics_frames() % 8 == 0:
        var testa = corpo[0]
        var nuova_testa = testa + direzione
        corpo.insert(0, nuova_testa)

        if crescere:
            crescere = false
        else:
            corpo.pop_back()
{% endhighlight %}

## Il cibo

Il cibo è una singola casella, posizionata a caso, che va evitata di posizionare
sopra il corpo del serpente stesso.

{% highlight gdscript %}
var cibo = Vector2(5, 5)

func posizione_libera_a_caso():
    while true:
        var posizione = Vector2(randi_range(0, COLONNE - 1), randi_range(0, RIGHE - 1))
        var occupata = false
        for segmento in corpo:
            if segmento == posizione:
                occupata = true
                break
        if not occupata:
            return posizione
{% endhighlight %}

Quando la testa raggiunge il cibo, il serpente deve crescere e il cibo deve
riapparire in un'altra posizione casuale.

{% highlight gdscript %}
var punteggio = 0

func _physics_process(delta):
    var nuova_testa = corpo[0] + direzione
    if nuova_testa == cibo:
        crescere = true
        punteggio += 1
        cibo = posizione_libera_a_caso()
{% endhighlight %}

## Game over

Il serpente muore se la testa esce dalla griglia, oppure se collide con uno degli
altri segmenti del proprio corpo.

{% highlight gdscript %}
var finita = false

func _physics_process(delta):
    var testa = corpo[0]
    var fuori_dalla_griglia = testa.x < 0 or testa.x >= COLONNE or testa.y < 0 or testa.y >= RIGHE

    var contro_se_stesso = false
    for i in range(1, corpo.size()):
        if corpo[i] == testa:
            contro_se_stesso = true
            break

    finita = fuori_dalla_griglia or contro_se_stesso
{% endhighlight %}

## Il gioco completo

{% highlight gdscript %}
extends Node2D

const CASELLA = 40
const COLONNE = 1280 / CASELLA
const RIGHE = 720 / CASELLA

var corpo = [Vector2(16, 9), Vector2(15, 9), Vector2(14, 9)]
var direzione = Vector2(1, 0)
var cibo = Vector2(5, 5)
var crescere = false
var punteggio = 0
var finita = false

func posizione_libera_a_caso():
    while true:
        var posizione = Vector2(randi_range(0, COLONNE - 1), randi_range(0, RIGHE - 1))
        var occupata = false
        for segmento in corpo:
            if segmento == posizione:
                occupata = true
                break
        if not occupata:
            return posizione

func _ready():
    cibo = posizione_libera_a_caso()

func _physics_process(delta):
    if finita:
        queue_redraw()
        return

    # cambio di direzione
    if Input.is_action_just_pressed("ui_up") and direzione.y == 0:
        direzione = Vector2(0, -1)
    elif Input.is_action_just_pressed("ui_down") and direzione.y == 0:
        direzione = Vector2(0, 1)
    elif Input.is_action_just_pressed("ui_left") and direzione.x == 0:
        direzione = Vector2(-1, 0)
    elif Input.is_action_just_pressed("ui_right") and direzione.x == 0:
        direzione = Vector2(1, 0)

    # movimento, una casella ogni 8 passi fisici
    if Engine.get_physics_frames() % 8 == 0:
        var testa = corpo[0]
        var nuova_testa = testa + direzione
        corpo.insert(0, nuova_testa)

        if crescere:
            crescere = false
        else:
            corpo.pop_back()

        # cibo raggiunto
        if nuova_testa == cibo:
            crescere = true
            punteggio += 1
            cibo = posizione_libera_a_caso()

        # game over
        var fuori_dalla_griglia = nuova_testa.x < 0 or nuova_testa.x >= COLONNE or nuova_testa.y < 0 or nuova_testa.y >= RIGHE
        var contro_se_stesso = false
        for i in range(1, corpo.size()):
            if corpo[i] == nuova_testa:
                contro_se_stesso = true
                break
        finita = fuori_dalla_griglia or contro_se_stesso

    $Punteggio.text = "Punteggio: %d" % punteggio
    queue_redraw()

func _draw():
    for segmento in corpo:
        draw_rect(Rect2(segmento * CASELLA, Vector2(CASELLA - 2, CASELLA - 2)), Color(0, 0.8, 0))
    draw_rect(Rect2(cibo * CASELLA, Vector2(CASELLA - 2, CASELLA - 2)), Color(0.85, 0.15, 0.15))

    if finita:
        draw_string(ThemeDB.fallback_font, Vector2(580, 360), "Game Over", HORIZONTAL_ALIGNMENT_CENTER, -1, 32)
{% endhighlight %}

## Come continuare

* far aumentare la velocità del serpente (diminuendo il numero 8 usato nel modulo)
  ogni volta che mangia un frutto
* salvare il [punteggio più alto]({{ site.baseurl }}{% link _godot/salvataggio.md %}.html)
  ottenuto tra una partita e l'altra
* trasformare il messaggio di game over in una vera schermata da cui ricominciare,
  come visto nello [stato del gioco e cambio scena]({{ site.baseurl }}{% link _godot/scene.md %}.html)
* disegnare il serpente con degli sprite invece di semplici rettangoli, come visto
  nella pagina sugli [Sprite2D]({{ site.baseurl }}{% link _godot/sprite2d.md %}.html)
