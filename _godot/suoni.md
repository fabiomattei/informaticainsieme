---
title: 'Godot: i suoni'
date: '2026-08-18T11:25:00+01:00'
author: Fabio Mattei
layout: page
---

Un videogioco senza audio perde molta della sua efficacia: i suoni danno un feedback
immediato al giocatore, la musica crea l'atmosfera. Come Dragonruby, Godot distingue
concettualmente tra effetti sonori brevi e tracce musicali più lunghe, ma affida
entrambi a un solo tipo di nodo: **AudioStreamPlayer**.

## Effetti sonori

#### Esercizio 1
Aggiungi un nodo `AudioStreamPlayer`, trascina un file audio (`.wav` o `.ogg`)
nella proprietà **Stream** dall'Inspector.

{% highlight gdscript %}
extends Node2D

func _input(event):
    if event.is_action_pressed("ui_accept"):
        $AudioStreamPlayer.play()
{% endhighlight %}

`.play()` avvia la riproduzione dall'inizio. A differenza di Dragonruby, dove basta
aggiungere il percorso del file a `args.outputs.sounds` ogni volta che serve, in
Godot il suono va prima **collegato a un nodo** nella scena; da script si richiama
solo `.play()` su quel nodo.

## Caricare un suono direttamente da script

Se non si vuole preparare un nodo per ogni suono possibile (utile ad esempio quando
i suoni da riprodurre sono molti, come gli effetti di un intero gioco), si può
creare un `AudioStreamPlayer` al volo e caricare il file con `load()`.

{% highlight gdscript %}
func riproduci_suono(percorso):
    var player = AudioStreamPlayer.new()
    add_child(player)
    player.stream = load(percorso)
    player.play()
    player.finished.connect(player.queue_free)
{% endhighlight %}

`player.finished.connect(player.queue_free)` collega il **segnale** `finished`
(emesso quando il suono termina) al metodo `queue_free()`, che rimuove il nodo:
così ogni player temporaneo si elimina da solo dopo aver suonato, senza accumularsi
nella scena. I segnali sono il modo standard con cui i nodi Godot comunicano eventi:
li approfondiremo nella pagina sui [pulsanti cliccabili]({{ site.baseurl }}{% link _godot/pulsanti.md %}.html).

## Musica di sottofondo e loop

Per una traccia che deve continuare a suonare in loop, si imposta il flag di loop
direttamente sulla risorsa audio importata (selezionandola nella FileSystem dock, poi
nell'Inspector), oppure si ricollega il segnale `finished` per far ripartire il
suono da capo.

{% highlight gdscript %}
extends Node2D

func _ready():
    $Musica.play()
    $Musica.finished.connect(func(): $Musica.play())
{% endhighlight %}

## Controllare volume e velocità

Le proprietà principali di un `AudioStreamPlayer` sono:

| Proprietà       | Significato                                              |
|------------------|-------------------------------------------------------------|
| `volume_db`      | il volume, espresso in **decibel** (0 = normale, negativo = più piano) |
| `pitch_scale`    | la velocità/altezza di riproduzione, 1.0 è la normalità     |
| `playing`        | `true` mentre il suono sta suonando                         |
| `stream_paused`  | `true` per mettere in pausa senza fermare la riproduzione   |

{% highlight gdscript %}
func _ready():
    $Musica.volume_db = -10    # più piano del normale
    $Musica.pitch_scale = 1.2  # leggermente più veloce e più acuto
{% endhighlight %}

A differenza di Dragonruby, dove `gain` va da 0.0 a 1.0 in modo lineare, Godot usa i
decibel: una scala logaritmica, più fedele a come l'orecchio umano percepisce il
volume.

## Interrompere un suono

{% highlight gdscript %}
func _ready():
    if Input.is_action_just_pressed("ui_cancel"):
        $Musica.stop()
{% endhighlight %}

## Volume generale del gioco

Godot organizza l'audio in **bus**, gestibili dal pannello **Audio** in basso
nell'editor. Il volume generale si controlla impostando il volume del bus
"Master", accessibile anche da script:

{% highlight gdscript %}
func _ready():
    var indice_bus = AudioServer.get_bus_index("Master")
    AudioServer.set_bus_volume_db(indice_bus, -6)
{% endhighlight %}

## Riepilogo: da Dragonruby a Godot

| Dragonruby                                | Godot equivalente                          |
|---------------------------------------------|-----------------------------------------------|
| `args.outputs.sounds << "file.wav"`         | nodo `AudioStreamPlayer` + `.play()`           |
| `args.audio[:chiave] = {input:, looping:}`  | `AudioStreamPlayer` con loop sulla risorsa o segnale `finished` |
| `gain` (0.0 - 1.0, lineare)                 | `volume_db` (decibel, logaritmico)             |
| `pitch`                                      | `pitch_scale`                                  |
| `args.audio.volume`                         | `AudioServer.set_bus_volume_db("Master", ...)` |
