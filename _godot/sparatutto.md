---
title: 'Godot: esempio, uno space shooter'
date: '2026-08-18T12:35:00+01:00'
author: Fabio Mattei
layout: page
---

Come esempio conclusivo di questa sezione mettiamo insieme quasi tutte le tecniche
viste nelle pagine precedenti per costruire un piccolo **space shooter**:
un'astronave che si muove in basso sullo schermo, spara verso l'alto, e deve
distruggere i nemici che scendono dall'alto prima che la raggiungano.

#### Preparazione della scena
Crea una scena con un `Node2D` come radice e un nodo figlio `Label` chiamato
"Punteggio". Collega uno script al nodo radice.

## L'astronave del giocatore

Il giocatore si muove solo a sinistra e a destra, restando sempre alla stessa
altezza, come già visto per l'[input]({{ site.baseurl }}{% link _godot/input.md %}.html).

{% highlight gdscript %}
extends Node2D

const LARGHEZZA_SCHERMO = 1280
const ALTEZZA_SCHERMO = 720

var giocatore = Rect2(610, 60, 60, 60)

func _physics_process(delta):
    var direzione = Input.get_axis("ui_left", "ui_right")
    giocatore.position.x += direzione * 360 * delta

    queue_redraw()

func _draw():
    draw_rect(giocatore, Color(0.3, 0.6, 1))
{% endhighlight %}

## Sparare

Ogni volta che viene premuta la barra spaziatrice creiamo un nuovo proiettile,
esattamente sopra l'astronave, che si sposterà verso l'alto ad ogni passo fisico.
Usiamo `is_action_just_pressed` e non `is_action_pressed`, altrimenti tenere
premuto lo spazio produrrebbe decine di proiettili al secondo.

{% highlight gdscript %}
var proiettili = []

func _physics_process(delta):
    if Input.is_action_just_pressed("ui_accept"):
        proiettili.append(Rect2(
            giocatore.position.x + giocatore.size.x / 2 - 4,
            giocatore.position.y + giocatore.size.y,
            8, 20
        ))

    for p in proiettili:
        p.position.y += 600 * delta

    proiettili = proiettili.filter(func(p): return p.position.y < ALTEZZA_SCHERMO)
{% endhighlight %}

Come già visto nella pagina sui [nemici che appaiono nel tempo]({{ site.baseurl }}{% link _godot/spawn.md %}.html),
è fondamentale togliere dall'array i proiettili usciti dallo schermo con
`.filter()`, altrimenti l'array crescerebbe all'infinito.

## I nemici

I nemici compaiono ad intervalli regolari in alto, ad una posizione orizzontale
casuale, e scendono verso il basso.

#### Preparazione della scena (aggiunta)
Aggiungi un nodo `Timer` alla scena, con **Wait Time** a 1.0 secondo e
**Autostart** attivo.

{% highlight gdscript %}
var nemici = []

func _ready():
    $Timer.timeout.connect(_su_timer_scaduto)

func _su_timer_scaduto():
    nemici.append(Rect2(randi_range(0, 1230), ALTEZZA_SCHERMO, 50, 50))

func _physics_process(delta):
    for n in nemici:
        n.position.y -= 120 * delta
{% endhighlight %}

## Le collisioni tra proiettili e nemici

Per ogni proiettile controlliamo se ha colpito un nemico con `.intersects()`, come
visto già in [Pong]({{ site.baseurl }}{% link _godot/pong.md %}.html). Marchiamo
entrambi come "morti" e li rimuoviamo tutti insieme alla fine, con lo stesso
pattern "marca e poi rimuovi" già incontrato nella pagina sulle
[collisioni]({{ site.baseurl }}{% link _godot/collisioni.md %}.html).

{% highlight gdscript %}
var punteggio = 0

func _physics_process(delta):
    var indici_proiettili_morti = []
    var indici_nemici_morti = []

    for i in range(proiettili.size()):
        for j in range(nemici.size()):
            if proiettili[i].intersects(nemici[j]):
                indici_proiettili_morti.append(i)
                indici_nemici_morti.append(j)
                punteggio += 10

    var nuovi_proiettili = []
    for i in range(proiettili.size()):
        if not (i in indici_proiettili_morti):
            nuovi_proiettili.append(proiettili[i])
    proiettili = nuovi_proiettili

    var nuovi_nemici = []
    for j in range(nemici.size()):
        if not (j in indici_nemici_morti):
            nuovi_nemici.append(nemici[j])
    nemici = nuovi_nemici
{% endhighlight %}

Rimuovere elementi da un array **mentre lo si scorre** è un errore comune: per
questo si raccolgono prima gli indici da eliminare, e si applica il filtro solo
alla fine, dopo aver terminato il doppio ciclo di controllo.

## Il gioco completo

{% highlight gdscript %}
extends Node2D

const LARGHEZZA_SCHERMO = 1280
const ALTEZZA_SCHERMO = 720

var giocatore = Rect2(610, 60, 60, 60)
var proiettili = []
var nemici = []
var punteggio = 0

func _ready():
    $Timer.timeout.connect(_su_timer_scaduto)

func _su_timer_scaduto():
    nemici.append(Rect2(randi_range(0, 1230), ALTEZZA_SCHERMO, 50, 50))

func _physics_process(delta):
    # movimento del giocatore
    var direzione = Input.get_axis("ui_left", "ui_right")
    giocatore.position.x += direzione * 360 * delta

    # sparare
    if Input.is_action_just_pressed("ui_accept"):
        proiettili.append(Rect2(
            giocatore.position.x + giocatore.size.x / 2 - 4,
            giocatore.position.y + giocatore.size.y,
            8, 20
        ))

    for p in proiettili:
        p.position.y += 600 * delta
    proiettili = proiettili.filter(func(p): return p.position.y < ALTEZZA_SCHERMO)

    # movimento dei nemici
    for n in nemici:
        n.position.y -= 120 * delta

    # collisioni tra proiettili e nemici
    var indici_proiettili_morti = []
    var indici_nemici_morti = []
    for i in range(proiettili.size()):
        for j in range(nemici.size()):
            if proiettili[i].intersects(nemici[j]):
                indici_proiettili_morti.append(i)
                indici_nemici_morti.append(j)
                punteggio += 10

    var nuovi_proiettili = []
    for i in range(proiettili.size()):
        if not (i in indici_proiettili_morti):
            nuovi_proiettili.append(proiettili[i])
    proiettili = nuovi_proiettili

    var nuovi_nemici = []
    for j in range(nemici.size()):
        if not (j in indici_nemici_morti):
            nuovi_nemici.append(nemici[j])
    nemici = nuovi_nemici

    $Punteggio.text = "Punteggio: %d" % punteggio
    queue_redraw()

func _draw():
    draw_rect(giocatore, Color(0.3, 0.6, 1))
    for p in proiettili:
        draw_rect(p, Color(1, 1, 0))
    for n in nemici:
        draw_rect(n, Color(1, 0.3, 0.3))
{% endhighlight %}

## Come continuare

Questo esempio riassume gran parte della sezione, ma si può ancora migliorare
molto, riprendendo le pagine già viste:

* aggiungere delle [particelle]({{ site.baseurl }}{% link _godot/particelle.md %}.html)
  quando un nemico viene distrutto
* far terminare la partita, con una vera schermata di
  [game over]({{ site.baseurl }}{% link _godot/scene.md %}.html), quando un nemico
  raggiunge l'altezza del giocatore
* salvare il [punteggio più alto]({{ site.baseurl }}{% link _godot/salvataggio.md %}.html)
  ottenuto tra una partita e l'altra
* far diminuire il tempo di attesa del `Timer` nel tempo, come visto nella pagina
  sullo [spawn dei nemici]({{ site.baseurl }}{% link _godot/spawn.md %}.html), per
  aumentare la difficoltà
* aggiungere un pulsante "Gioca" cliccabile nel menu iniziale, come visto nella
  pagina sui [pulsanti cliccabili]({{ site.baseurl }}{% link _godot/pulsanti.md %}.html)
