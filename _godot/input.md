---
title: 'Godot: input'
date: '2026-08-18T11:30:00+01:00'
author: Fabio Mattei
layout: page
---

Per permettere all'utente di interagire con il nostro gioco dobbiamo raccogliere il
suo input. Godot mette a disposizione il singleton globale **Input**, accessibile da
qualsiasi script senza bisogno di un riferimento (a differenza di Dragonruby, dove
l'input passa sempre attraverso il parametro `args`).

## Le Input Map: azioni invece di tasti

La differenza concettuale più importante rispetto a Dragonruby è che Godot
introduce un livello di indirezione tra il tasto fisico e il significato nel gioco,
chiamato **azione** (action). Invece di controllare direttamente "è premuta la
freccia sinistra?", si controlla "è attiva l'azione ui_left?", e la corrispondenza
tra tasti e azioni si configura in **Project > Project Settings > Input Map**.

Godot fornisce già alcune azioni predefinite (`ui_left`, `ui_right`, `ui_up`,
`ui_down`, `ui_accept`, `ui_cancel`), collegate di default alle frecce, a Invio e a
Esc. Per un gioco vero conviene creare azioni personalizzate (ad esempio "spara",
"salta") e assegnare loro uno o più tasti dal pannello Input Map: in questo modo,
se in futuro si vuole cambiare tasto, basta modificare l'Input Map senza toccare il
codice.

## Tastiera

Come in Dragonruby, abbiamo diverse modalità di lettura.

{% highlight gdscript %}
func _physics_process(delta):
    # true finché il tasto/azione resta premuto (come key_held)
    if Input.is_action_pressed("ui_left"):
        position.x -= 200 * delta

    # true solo nel frame in cui è stato premuto (come key_down)
    if Input.is_action_just_pressed("ui_accept"):
        print("Premuto!")

    # true solo nel frame in cui è stato rilasciato (come key_up)
    if Input.is_action_just_released("ui_accept"):
        print("Rilasciato!")
{% endhighlight %}

| Metodo                              | Equivalente Dragonruby              |
|----------------------------------------|-----------------------------------------|
| `Input.is_action_pressed(azione)`      | `args.inputs.keyboard.key_held.tasto`   |
| `Input.is_action_just_pressed(azione)` | `args.inputs.keyboard.key_down.tasto`   |
| `Input.is_action_just_released(azione)`| `args.inputs.keyboard.key_up.tasto`     |

Per leggere un tasto specifico senza passare da un'azione (utile per prototipare
rapidamente), si usa `Input.is_key_pressed()` con le costanti `KEY_*`.

{% highlight gdscript %}
func _physics_process(delta):
    if Input.is_key_pressed(KEY_A):
        print("Il tasto A è premuto")
{% endhighlight %}

## Assi: scorciatoia per movimenti su due direzioni

Per un movimento sinistra/destra o su/giù, invece di controllare separatamente due
azioni opposte, `Input.get_axis()` restituisce direttamente un valore da -1 a 1.

{% highlight gdscript %}
func _physics_process(delta):
    var direzione = Input.get_axis("ui_left", "ui_right")
    position.x += direzione * 200 * delta
{% endhighlight %}

Esiste anche `Input.get_vector()`, che restituisce un `Vector2` completo per un
movimento su entrambi gli assi in un colpo solo, già normalizzato per evitare che
muoversi in diagonale risulti più veloce.

{% highlight gdscript %}
func _physics_process(delta):
    var movimento = Input.get_vector("ui_left", "ui_right", "ui_up", "ui_down")
    position += movimento * 200 * delta
{% endhighlight %}

## Mouse

Il mouse dà diversi tipi di input, accessibili tramite il singleton `Input` e
attraverso gli eventi.

#### Posizione corrente del mouse

{% highlight gdscript %}
func _process(delta):
    var posizione = get_viewport().get_mouse_position()
    print(posizione.x, posizione.y)
{% endhighlight %}

#### Pressione dei pulsanti del mouse

{% highlight gdscript %}
func _process(delta):
    if Input.is_mouse_button_pressed(MOUSE_BUTTON_LEFT):
        print("Pulsante sinistro premuto")
{% endhighlight %}

#### Verificare se il mouse è dentro un rettangolo

A differenza di Dragonruby, che offre `inside_rect?` già pronto, in Godot si
costruisce un `Rect2` e si chiama `.has_point()`.

{% highlight gdscript %}
func _process(delta):
    var area = Rect2(100, 120, 80, 60)
    var dentro = area.has_point(get_viewport().get_mouse_position())
    print(dentro)
{% endhighlight %}

## L'evento _input(event): un approccio alternativo

Oltre a interrogare lo stato attuale dentro `_process`/`_physics_process` (il
cosiddetto **polling**, lo stesso approccio di Dragonruby), Godot offre anche un
callback specifico per gli eventi di input, `_input(event)`, chiamato solo quando
succede davvero qualcosa (un tasto premuto, il mouse mosso).

{% highlight gdscript %}
func _input(event):
    if event is InputEventKey and event.pressed and not event.echo:
        print("Tasto premuto: %s" % event.as_text())

    if event is InputEventMouseButton and event.pressed:
        print("Click alle coordinate: %s" % event.position)
{% endhighlight %}

Questo approccio ad eventi è più efficiente per azioni rare (un click, la pressione
di un tasto una tantum), mentre il polling dentro `_physics_process` resta la scelta
naturale per il movimento continuo, esattamente come `tick` in Dragonruby.

## Controller

Godot gestisce automaticamente più controller collegati, identificati da un
indice numerico (0, 1, 2, …). Le azioni configurate nell'Input Map possono essere
collegate anche ai tasti del controller, quindi lo stesso codice visto sopra con
`Input.is_action_pressed("ui_left")` funziona identico sia con tastiera che con
controller, senza scrivere codice separato come si farebbe in Dragonruby con
`args.inputs.controller_one`.
