---
title: 'Godot: esempio, il tris'
date: '2026-08-18T12:50:00+01:00'
author: Fabio Mattei
layout: page
---

Tutti gli esempi visti finora sono giochi in tempo reale, dove qualcosa si muove
ad ogni frame anche se il giocatore non fa nulla. Il **tris** (conosciuto anche
come **tic-tac-toe**) è un gioco completamente diverso: **a turni**, dove non
succede nulla finché uno dei due giocatori non clicca su una casella.

#### Preparazione della scena
Crea una scena con un `Node2D` come radice e collegaci uno script.

## Il tabellone

Il tabellone è una griglia 3x3, la stessa idea di griglia vista in
[Snake]({{ site.baseurl }}{% link _godot/snake.md %}.html), ma qui rappresentiamo
le nove celle con un semplice array di 9 elementi invece che con le coordinate di
un array bidimensionale: la cella alla riga `r` e colonna `c` si trova all'indice
`r * 3 + c`.

{% highlight gdscript %}
extends Node2D

const DIMENSIONE_CELLA = 200
const ORIGINE_X = 340
const ORIGINE_Y = 60

var celle = [null, null, null, null, null, null, null, null, null]

func _draw():
    for i in range(1, 3):
        draw_line(
            Vector2(ORIGINE_X + i * DIMENSIONE_CELLA, ORIGINE_Y),
            Vector2(ORIGINE_X + i * DIMENSIONE_CELLA, ORIGINE_Y + 600),
            Color(1, 1, 1), 2.0
        )
        draw_line(
            Vector2(ORIGINE_X, ORIGINE_Y + i * DIMENSIONE_CELLA),
            Vector2(ORIGINE_X + 600, ORIGINE_Y + i * DIMENSIONE_CELLA),
            Color(1, 1, 1), 2.0
        )
{% endhighlight %}

L'array `celle` inizia con nove valori `null`: nessuna cella è stata ancora
giocata. Le linee disegnano soltanto le due righe e le due colonne interne, quelle
che dividono le nove celle, come già visto per le
[forme e primitive grafiche]({{ site.baseurl }}{% link _godot/forme.md %}.html).

## Capire su quale cella si è cliccato

Per ogni cella controlliamo, con `Rect2.has_point()` già anticipato nella pagina
sull'[input]({{ site.baseurl }}{% link _godot/input.md %}.html), se il click del
mouse è avvenuto al suo interno.

{% highlight gdscript %}
func _unhandled_input(event):
    if event is InputEventMouseButton and event.pressed and event.button_index == MOUSE_BUTTON_LEFT:
        for riga in range(3):
            for colonna in range(3):
                var area = Rect2(
                    ORIGINE_X + colonna * DIMENSIONE_CELLA,
                    ORIGINE_Y + riga * DIMENSIONE_CELLA,
                    DIMENSIONE_CELLA, DIMENSIONE_CELLA
                )
                if area.has_point(event.position):
                    var indice = riga * 3 + colonna
                    # qui andremo a registrare la mossa
{% endhighlight %}

`_unhandled_input` è una variante di `_input` (vista nella pagina
sull'[input]({{ site.baseurl }}{% link _godot/input.md %}.html)) che riceve
l'evento solo se nessun altro nodo dell'interfaccia (come un `Button`) lo ha già
gestito: è la scelta più adatta per un click "sul fondo" della scena.

## Registrare una mossa e cambiare turno

Una mossa è valida solo se la cella cliccata è ancora libera (`null`). Dopo una
mossa valida tocca all'altro giocatore.

{% highlight gdscript %}
var turno = "x"

func gioca_mossa(indice):
    if celle[indice] == null:
        celle[indice] = turno
        turno = "o" if turno == "x" else "x"
        queue_redraw()
{% endhighlight %}

## Disegnare le X e le O

Per ogni cella non vuota disegniamo il simbolo, centrato al centro della cella, con
`draw_string()`.

{% highlight gdscript %}
func _draw():
    for riga in range(3):
        for colonna in range(3):
            var simbolo = celle[riga * 3 + colonna]
            if simbolo == null:
                continue

            var centro = Vector2(
                ORIGINE_X + colonna * DIMENSIONE_CELLA + DIMENSIONE_CELLA / 2 - 20,
                ORIGINE_Y + riga * DIMENSIONE_CELLA + DIMENSIONE_CELLA / 2 + 20
            )
            draw_string(ThemeDB.fallback_font, centro, simbolo.to_upper(), HORIZONTAL_ALIGNMENT_CENTER, -1, 64)
{% endhighlight %}

## Controllare se qualcuno ha vinto

Ci sono otto modi per vincere: tre righe, tre colonne e due diagonali. Ognuno si
rappresenta con i tre indici dell'array che devono contenere lo stesso simbolo.

{% highlight gdscript %}
const COMBINAZIONI_VINCENTI = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8],
    [0, 3, 6], [1, 4, 7], [2, 5, 8],
    [0, 4, 8], [2, 4, 6]
]

func trova_vincitore():
    for combinazione in COMBINAZIONI_VINCENTI:
        var a = combinazione[0]
        var b = combinazione[1]
        var c = combinazione[2]
        if celle[a] != null and celle[a] == celle[b] and celle[b] == celle[c]:
            return celle[a]
    return null
{% endhighlight %}

## Pareggio e come ricominciare

Se nessuno ha vinto e tutte le celle sono piene, la partita finisce in pareggio.
Quando la partita è finita, un click permette di ricominciare da capo.

{% highlight gdscript %}
func partita_finita():
    if trova_vincitore() != null:
        return true
    return not (null in celle)

func ricomincia():
    celle = [null, null, null, null, null, null, null, null, null]
    turno = "x"
{% endhighlight %}

## Il gioco completo

{% highlight gdscript %}
extends Node2D

const DIMENSIONE_CELLA = 200
const ORIGINE_X = 340
const ORIGINE_Y = 60

const COMBINAZIONI_VINCENTI = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8],
    [0, 3, 6], [1, 4, 7], [2, 5, 8],
    [0, 4, 8], [2, 4, 6]
]

var celle = [null, null, null, null, null, null, null, null, null]
var turno = "x"

func trova_vincitore():
    for combinazione in COMBINAZIONI_VINCENTI:
        var a = combinazione[0]
        var b = combinazione[1]
        var c = combinazione[2]
        if celle[a] != null and celle[a] == celle[b] and celle[b] == celle[c]:
            return celle[a]
    return null

func partita_finita():
    if trova_vincitore() != null:
        return true
    return not (null in celle)

func ricomincia():
    celle = [null, null, null, null, null, null, null, null, null]
    turno = "x"

func gioca_mossa(indice):
    if celle[indice] == null:
        celle[indice] = turno
        turno = "o" if turno == "x" else "x"

func _unhandled_input(event):
    if not (event is InputEventMouseButton and event.pressed and event.button_index == MOUSE_BUTTON_LEFT):
        return

    if partita_finita():
        ricomincia()
        queue_redraw()
        return

    for riga in range(3):
        for colonna in range(3):
            var area = Rect2(
                ORIGINE_X + colonna * DIMENSIONE_CELLA,
                ORIGINE_Y + riga * DIMENSIONE_CELLA,
                DIMENSIONE_CELLA, DIMENSIONE_CELLA
            )
            if area.has_point(event.position):
                gioca_mossa(riga * 3 + colonna)

    queue_redraw()

func _draw():
    # griglia
    for i in range(1, 3):
        draw_line(
            Vector2(ORIGINE_X + i * DIMENSIONE_CELLA, ORIGINE_Y),
            Vector2(ORIGINE_X + i * DIMENSIONE_CELLA, ORIGINE_Y + 600),
            Color(1, 1, 1), 2.0
        )
        draw_line(
            Vector2(ORIGINE_X, ORIGINE_Y + i * DIMENSIONE_CELLA),
            Vector2(ORIGINE_X + 600, ORIGINE_Y + i * DIMENSIONE_CELLA),
            Color(1, 1, 1), 2.0
        )

    # simboli
    for riga in range(3):
        for colonna in range(3):
            var simbolo = celle[riga * 3 + colonna]
            if simbolo == null:
                continue
            var centro = Vector2(
                ORIGINE_X + colonna * DIMENSIONE_CELLA + DIMENSIONE_CELLA / 2 - 20,
                ORIGINE_Y + riga * DIMENSIONE_CELLA + DIMENSIONE_CELLA / 2 + 20
            )
            draw_string(ThemeDB.fallback_font, centro, simbolo.to_upper(), HORIZONTAL_ALIGNMENT_CENTER, -1, 64)

    # messaggio di stato
    var vincitore = trova_vincitore()
    var testo
    if vincitore != null:
        testo = "Ha vinto %s! Clicca per ricominciare" % vincitore.to_upper()
    elif partita_finita():
        testo = "Pareggio! Clicca per ricominciare"
    else:
        testo = "Turno di %s" % turno.to_upper()

    draw_string(ThemeDB.fallback_font, Vector2(400, 720), testo, HORIZONTAL_ALIGNMENT_CENTER, -1, 24)
{% endhighlight %}

## Come continuare

* evidenziare con un colore diverso la casella su cui si trova il mouse, come
  visto per i [pulsanti cliccabili]({{ site.baseurl }}{% link _godot/pulsanti.md %}.html)
* far scegliere il nome dei due giocatori con la tecnica vista nella pagina su
  [come raccogliere il testo digitato]({{ site.baseurl }}{% link _godot/testo.md %}.html)
* tenere il conteggio delle vittorie di ciascun giocatore, salvandolo su file come
  visto nella pagina su [come salvare e caricare i dati]({{ site.baseurl }}{% link _godot/salvataggio.md %}.html)
* trasformare il messaggio di inizio e fine partita in vere schermate, come visto
  nello [stato del gioco e cambio scena]({{ site.baseurl }}{% link _godot/scene.md %}.html)
