---
title: 'Godot: raccogliere il testo digitato'
date: '2026-08-18T11:35:00+01:00'
author: Fabio Mattei
layout: page
---

Nella pagina sull'[input]({{ site.baseurl }}{% link _godot/input.md %}.html) abbiamo
visto come leggere la pressione di un singolo tasto. A volte però serve raccogliere
del testo vero e proprio digitato dal giocatore, ad esempio per chiedere il suo nome
prima di iniziare la partita. A differenza di Dragonruby, dove si intercetta
carattere per carattere con `args.inputs.keyboard.key_down.char`, Godot offre un
nodo già pronto per questo: **LineEdit**.

## Un campo di testo pronto all'uso

#### Esercizio 1
Aggiungi un nodo `LineEdit` alla scena. Imposta, se vuoi, la proprietà
**Placeholder Text** su "Scrivi il tuo nome...".

{% highlight gdscript %}
extends Node2D

func _ready():
    pass

func mostra_nome():
    var nome = $LineEdit.text
    print("Ciao, %s!" % nome)
{% endhighlight %}

`$LineEdit.text` contiene in ogni momento quello che l'utente ha digitato: Godot si
occupa da solo di intercettare i tasti, gestire il backspace, spostare il cursore e
mostrare il testo — tutto quello che in Dragonruby andava scritto a mano.

## Reagire quando l'utente preme Invio

Il nodo `LineEdit` emette un **segnale**, `text_submitted`, quando l'utente preme
Invio dentro il campo. Ci si collega da script con `.connect()`.

#### Esercizio 2
Copia questo script nel nodo padre della scena, che contiene un `LineEdit` chiamato
"CampoNome".

{% highlight gdscript %}
extends Node2D

func _ready():
    $CampoNome.text_submitted.connect(_su_nome_confermato)

func _su_nome_confermato(nuovo_testo):
    print("Nome confermato: %s" % nuovo_testo)
{% endhighlight %}

`.connect()` collega il segnale a una funzione: ogni volta che l'utente preme
Invio, Godot chiama automaticamente `_su_nome_confermato`, passandole il testo
digitato. I segnali sono lo stesso meccanismo che vedremo per i
[pulsanti cliccabili]({{ site.baseurl }}{% link _godot/pulsanti.md %}.html).

## Limitare la lunghezza del testo

`LineEdit` ha una proprietà `max_length` che impedisce di digitare oltre un certo
numero di caratteri, senza bisogno di controllarlo manualmente ad ogni carattere
come si farebbe in Dragonruby.

{% highlight gdscript %}
func _ready():
    $CampoNome.max_length = 10
{% endhighlight %}

## Una schermata di inserimento nome

Mettendo insieme questa tecnica con lo [stato del gioco e il cambio scena]({{ site.baseurl }}{% link _godot/scene.md %}.html)
possiamo costruire una vera schermata iniziale.

{% highlight gdscript %}
extends Node2D

func _ready():
    $CampoNome.text_submitted.connect(_su_nome_confermato)

func _su_nome_confermato(nome):
    if nome.length() > 0:
        GestoreGioco.nome_giocatore = nome
        get_tree().change_scene_to_file("res://gioco.tscn")
{% endhighlight %}

`GestoreGioco` qui è un ipotetico Autoload, visto nella pagina sulla
[visibilità delle variabili]({{ site.baseurl }}{% link _godot/visibilita-variabili.md %}.html),
usato per portare il nome digitato da una scena all'altra: ogni scena, a differenza
di `args.state` in Dragonruby, parte con il proprio stato "pulito", quindi i dati
da conservare tra una scena e l'altra vanno messi in un Autoload.

## Testo su più righe: TextEdit

Se serve raccogliere più di una riga di testo, il nodo equivalente è `TextEdit`,
che si usa in modo molto simile ma espone `.text` come stringa multi-riga invece
che un singolo segnale alla pressione di Invio (che in un'area di testo serve
normalmente per andare a capo, non per confermare).
