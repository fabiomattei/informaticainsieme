---
title: 'Godot: pulsanti cliccabili'
date: '2026-08-18T11:40:00+01:00'
author: Fabio Mattei
layout: page
---

Nel menu del nostro [stato del gioco]({{ site.baseurl }}{% link _godot/scene.md %}.html)
può essere comodo offrire un vero pulsante da cliccare, invece di chiedere di
premere un tasto della tastiera. In Dragonruby un pulsante andava costruito a mano
componendo un rettangolo, una label e il rilevamento del click del mouse. In Godot
esiste già un nodo pronto: **Button**.

## Aggiungere un pulsante

#### Esercizio 1
Aggiungi un nodo `Button` alla scena. Imposta la proprietà **Text** su "Gioca" e
posizionalo dove preferisci.

## Il segnale pressed

Ogni `Button` emette il segnale `pressed` quando viene cliccato. Ci si collega da
script proprio come visto per il `LineEdit` nella pagina precedente.

#### Esercizio 2
Copia questo script nel nodo padre, che contiene un `Button` chiamato
"PulsanteGioca".

{% highlight gdscript %}
extends Node2D

func _ready():
    $PulsanteGioca.pressed.connect(_su_gioca_premuto)

func _su_gioca_premuto():
    print("Si comincia!")
    get_tree().change_scene_to_file("res://gioco.tscn")
{% endhighlight %}

A differenza di Dragonruby, dove ogni tick bisognava controllare manualmente
`args.inputs.mouse.key_down.left && args.inputs.mouse.inside_rect?(...)`, qui Godot
si occupa da solo di capire quando il pulsante è stato premuto: il segnale scatta
una volta sola, nel momento giusto, senza bisogno di nessun controllo nel game loop.

## Collegare il segnale anche dall'editor

In alternativa a `.connect()` scritto da script, si può collegare un segnale
direttamente dall'editor: seleziona il nodo `Button`, apri la scheda **Node**
(accanto all'Inspector), doppio click sul segnale `pressed`, e scegli la funzione da
chiamare. Godot genera automaticamente lo scheletro della funzione nello script.
Le due strade sono equivalenti: quella da editor è comoda per pochi collegamenti
fissi, quella da script è più adatta quando i pulsanti si creano dinamicamente.

## Uno stato per pulsante: disabled

Un pulsante può essere disattivato impostando `disabled = true`: in questo stato
non genera più il segnale `pressed` e Godot lo disegna automaticamente con un
aspetto "spento", senza bisogno di gestire a mano un colore diverso come si farebbe
in Dragonruby.

{% highlight gdscript %}
func _ready():
    $PulsanteGioca.disabled = true
{% endhighlight %}

## L'effetto al passaggio del mouse è già incluso

In Dragonruby, dare un feedback visivo al passaggio del mouse richiedeva di
controllare manualmente `inside_rect?` ad ogni tick e cambiare colore di
conseguenza. Un `Button` di Godot lo fa già da solo: cambia automaticamente aspetto
quando il mouse è sopra, quando viene premuto, e quando è disabilitato, seguendo il
tema grafico del progetto. Per personalizzare questi aspetti si lavora sulle
proprietà **Theme Overrides** nell'Inspector, oppure creando un `Theme` dedicato.

## Un menu con più pulsanti

#### Esercizio 3
Aggiungi due pulsanti, "Gioca" e "Esci", entrambi figli dello stesso nodo padre.

{% highlight gdscript %}
extends Node2D

func _ready():
    $PulsanteGioca.pressed.connect(_su_gioca_premuto)
    $PulsanteEsci.pressed.connect(_su_esci_premuto)

func _su_gioca_premuto():
    get_tree().change_scene_to_file("res://gioco.tscn")

func _su_esci_premuto():
    get_tree().quit()
{% endhighlight %}

`get_tree().quit()` chiude il gioco: è l'equivalente Godot di terminare il
programma, un'operazione che in Dragonruby non ha un vero corrispettivo diretto
perché tipicamente si gioca dentro l'editor stesso durante lo sviluppo.

## Riepilogo: da Dragonruby a Godot

| Dragonruby (pulsante fatto a mano)                        | Godot equivalente          |
|---------------------------------------------------------------|-------------------------------|
| rettangolo + label + `inside_rect?` + `key_down.left`          | nodo `Button`                 |
| controllo manuale ad ogni tick                                 | segnale `pressed`             |
| cambiare colore manualmente al passaggio del mouse              | gestito automaticamente dal tema |
