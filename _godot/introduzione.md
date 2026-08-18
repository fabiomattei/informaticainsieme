---
title: 'Godot: interfaccia e primo script'
date: '2026-08-18T11:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Cos'è Godot

[Godot](https://godotengine.org/) è un motore per lo sviluppo di videogiochi, gratuito
e open source. A differenza di Dragonruby, che è essenzialmente una libreria attorno a
una singola funzione `tick`, Godot è un **editor completo**: una finestra con pannelli
per organizzare scene, script, immagini, suoni e tutto il resto del progetto. Il
linguaggio di programmazione principale di Godot si chiama **GDScript**, e la sua
sintassi assomiglia molto a Python.

## L'interfaccia dell'editor

Aprendo Godot e creando un nuovo progetto, la finestra principale si divide in alcune
zone fisse:

* **Viewport** (al centro): l'area di disegno dove si vede e si compone visivamente la
  scena su cui si sta lavorando.
* **Scene dock** (in alto a sinistra): l'elenco ad albero di tutti i **nodi** che
  compongono la scena aperta.
* **FileSystem dock** (in basso a sinistra): l'elenco di tutti i file del progetto —
  script, immagini, suoni, scene salvate.
* **Inspector** (a destra): mostra e permette di modificare le proprietà del nodo
  attualmente selezionato nella Scene dock.
* **Pannello in basso**: contiene, tra le altre, la scheda **Output**, dove compaiono i
  messaggi stampati con `print()` e gli eventuali errori.

In alto a destra si trovano i pulsanti per eseguire il progetto: **F5** esegue la
"scena principale" del progetto (quella impostata come punto di partenza), **F6**
esegue solo la scena attualmente aperta nell'editor — è quello che useremo più spesso
mentre proviamo i piccoli esempi di questa sezione.

## Nodi e scene

Se Dragonruby costruisce tutto attorno a una funzione `tick` che ridisegna ogni
elemento manualmente 60 volte al secondo, Godot ha un modello completamente diverso: un
**albero di nodi**. Ogni elemento del gioco — un personaggio, un'immagine, un suono, una
scritta, persino un raggruppamento logico — è un **nodo** (`Node`), e i nodi si
organizzano in una gerarchia ad albero dentro una **scena**.

Ogni tipo di nodo ha uno scopo specifico: `Node2D` rappresenta qualcosa con una
posizione nel piano 2D, `Sprite2D` mostra un'immagine, `Label` mostra del testo,
`Area2D` rileva le collisioni. Una scena può a sua volta essere inserita come nodo
dentro un'altra scena, permettendo di costruire un gioco componendo pezzi più piccoli
già pronti.

## Creare la prima scena

#### Esercizio 1
Crea una nuova scena (**Scene > New Scene**), scegli **Node2D** come nodo radice, e
salvala con un nome a piacere (ad esempio `principale.tscn`). Seleziona il nodo
radice nella Scene dock, poi clicca sull'icona a forma di pergamena nel pannello in
alto per creare ed allegare un nuovo script GDScript.

Godot genera automaticamente uno scheletro di script:

{% highlight gdscript %}
extends Node2D

func _ready():
    pass

func _process(delta):
    pass
{% endhighlight %}

`pass` è un'istruzione che non fa nulla: serve solo perché GDScript, come Python,
non accetta un blocco vuoto. Va sostituita dal codice vero e proprio.

## Il sistema di coordinate

A differenza di Dragonruby, dove l'origine è in basso a sinistra e l'asse Y cresce
verso l'alto, in Godot **l'origine è in alto a sinistra** e l'asse Y cresce **verso il
basso**: un valore di Y più grande sposta un oggetto più in basso sullo schermo, non
più in alto. È il sistema usato dalla maggior parte dei motori grafici 2D. La
dimensione della finestra di gioco si configura in **Project > Project Settings >
Display > Window** (ad esempio 1280×720, come nell'area di gioco di Dragonruby vista
in precedenza).

## _ready e _process: il game loop

Dragonruby chiama la funzione `tick` 60 volte al secondo per ogni frame. Godot divide
lo stesso concetto in due funzioni distinte, entrambe **callback** che Godot chiama da
solo quando lo script è collegato a un nodo attivo nella scena:

* **`_ready()`**: viene chiamata **una sola volta**, quando il nodo entra nella scena.
  È l'equivalente dell'inizializzazione fatta con `||=` in Dragonruby.
* **`_process(delta)`**: viene chiamata **una volta per ogni frame disegnato**,
  esattamente come `tick`. Il parametro `delta` è il tempo, in secondi, trascorso
  dall'ultimo frame: su uno schermo a 60 fotogrammi al secondo vale circa `0.0166`.

#### Esercizio 2
Copia questo script in un nodo `Node2D` e fallo eseguire con F6.

{% highlight gdscript %}
extends Node2D

func _ready():
    print("Il gioco è partito!")

func _process(delta):
    print("Tempo dall'ultimo frame: %s secondi" % delta)
{% endhighlight %}

Osserva il pannello Output: il messaggio di `_ready()` compare una volta sola,
mentre quello di `_process()` viene ripetuto continuamente.

### Perché delta, e non un conteggio fisso di frame

Dragonruby garantisce (quasi) sempre 60 tick al secondo, quindi un movimento di "6
pixel a tick" è prevedibile. Il framerate reale di un gioco può però variare (un
computer più lento potrebbe disegnare solo 30 fotogrammi al secondo): per questo, per
ottenere un movimento fluido e indipendente dal framerate, la velocità va sempre
**moltiplicata per `delta`**, invece di sommare un valore fisso ad ogni chiamata.

{% highlight gdscript %}
extends Node2D

var velocita = 200    # pixel al secondo, non pixel a frame

func _process(delta):
    position.x += velocita * delta
{% endhighlight %}

Con `velocita * delta`, in un secondo l'oggetto si sposta sempre di `velocita`
pixel, sia che il gioco giri a 30 che a 300 fotogrammi al secondo.

### _physics_process: l'alternativa a passo fisso

Esiste anche `_physics_process(delta)`, chiamata a intervalli **regolari** (di
default 60 volte al secondo, indipendentemente dal framerate di disegno). Si usa al
posto di `_process` per tutto ciò che riguarda il movimento e le collisioni fisiche,
proprio perché un passo costante rende i calcoli più prevedibili — è il corrispettivo
più fedele del `tick` di Dragonruby, e sarà la funzione che useremo più spesso negli
esempi di gioco di questa sezione.

## Muovere un nodo con l'input

Ogni nodo che eredita da `Node2D` (quindi anche `Sprite2D`, che vedremo nella prossima
pagina) ha una proprietà `position`, un `Vector2` con i campi `x` e `y`. Per
anticipare quanto vedremo nella pagina sull'[input]({{ site.baseurl }}{% link _godot/input.md %}.html),
ecco un piccolo esempio che sposta il nodo con le frecce della tastiera.

{% highlight gdscript %}
extends Node2D

func _physics_process(delta):
    if Input.is_action_pressed("ui_left"):
        position.x -= 200 * delta
    if Input.is_action_pressed("ui_right"):
        position.x += 200 * delta
    if Input.is_action_pressed("ui_up"):
        position.y -= 200 * delta
    if Input.is_action_pressed("ui_down"):
        position.y += 200 * delta
{% endhighlight %}

`"ui_left"`, `"ui_right"`, `"ui_up"` e `"ui_down"` sono **azioni di input**
predefinite da Godot, già collegate alle frecce della tastiera: concettualmente sono
l'equivalente di `args.inputs.left` in Dragonruby, ma passano attraverso un livello di
indirezione in più (il nome dell'azione) che rende facile, in seguito, ridefinire
quale tasto fisico corrisponde a quale comando.

## Riepilogo: da Dragonruby a Godot

| Dragonruby                          | Godot equivalente                              |
|----------------------------------------|---------------------------------------------------|
| funzione `tick`                        | `_process(delta)` o `_physics_process(delta)`      |
| `args.state.x ||= valore`              | `var x = valore` (attributo di istanza)            |
| `args.outputs.sprites << {...}`        | un nodo `Sprite2D` nella scena                     |
| origine in basso a sinistra, Y verso l'alto | origine in alto a sinistra, Y verso il basso  |
| un solo file di gioco                  | tante scene (`.tscn`) e script (`.gd`) collegati   |

Nelle prossime pagine vedremo, uno alla volta, i nodi principali che sostituiscono
gli elementi già visti per Dragonruby: le [Label]({{ site.baseurl }}{% link _godot/label.md %}.html)
per il testo e gli [Sprite2D]({{ site.baseurl }}{% link _godot/sprite2d.md %}.html)
per le immagini.
