---
title: 'Godot: salvare e caricare i dati'
date: '2026-08-18T12:15:00+01:00'
author: Fabio Mattei
layout: page
---

Come visto nella pagina su [stato del gioco e cambio scena]({{ site.baseurl }}{% link _godot/scene.md %}.html),
un Autoload conserva i dati finché il gioco resta aperto, ma li perde non appena il
programma viene chiuso. Per ricordare informazioni da una partita all'altra, ad
esempio il record migliore, dobbiamo scrivere quei dati su un file. Godot usa la
classe **FileAccess** al posto di `args.gtk.write_file`/`read_file` di Dragonruby.

## Scrivere un file

{% highlight gdscript %}
func _ready():
    if Input.is_action_just_pressed("ui_accept"):
        var file = FileAccess.open("user://record.txt", FileAccess.WRITE)
        file.store_string(str(GestoreGioco.punteggio))
        file.close()
{% endhighlight %}

`"user://"` è un percorso speciale che Godot traduce automaticamente in una
cartella dedicata al progetto sul computer dell'utente (diversa dai file del
progetto stesso): è l'equivalente della cartella dedicata usata da
`args.gtk.write_file` in Dragonruby, e funziona anche una volta che il gioco viene
esportato e installato. `str()` converte il punteggio in stringa, esattamente come
`.to_s` in Ruby.

`.close()` è importante: senza chiudere il file, le modifiche potrebbero non
essere salvate su disco.

## Leggere un file

{% highlight gdscript %}
func _ready():
    var record = 0
    if FileAccess.file_exists("user://record.txt"):
        var file = FileAccess.open("user://record.txt", FileAccess.READ)
        record = int(file.get_as_text())
        file.close()

    print("Record: %d" % record)
{% endhighlight %}

`FileAccess.file_exists()` va controllato **prima** di aprire il file in lettura:
a differenza di `args.gtk.read_file`, che restituisce `nil` se il file non esiste,
`FileAccess.open()` in lettura restituisce `null` e genera un errore in Output se il
file non c'è ancora — tipicamente la primissima volta che il gioco viene avviato.

## Un record che si aggiorna da solo

{% highlight gdscript %}
extends Node2D

var record = 0

func _ready():
    if FileAccess.file_exists("user://record.txt"):
        var file = FileAccess.open("user://record.txt", FileAccess.READ)
        record = int(file.get_as_text())
        file.close()

func _process(delta):
    $Etichetta.text = "Punteggio: %d   Record: %d" % [GestoreGioco.punteggio, record]

    if GestoreGioco.punteggio > record:
        record = GestoreGioco.punteggio
        var file = FileAccess.open("user://record.txt", FileAccess.WRITE)
        file.store_string(str(record))
        file.close()
{% endhighlight %}

## Salvare dati più complessi: JSON

Un singolo numero è facile da salvare, ma un gioco più completo ha bisogno di
salvare più informazioni insieme: il record, il livello raggiunto, le impostazioni
audio. Come in Dragonruby, conviene usare il formato **JSON**.

{% highlight gdscript %}
func salva_partita():
    var dati = {"record": record, "livello": GestoreGioco.livello}
    var file = FileAccess.open("user://salvataggio.json", FileAccess.WRITE)
    file.store_string(JSON.stringify(dati))
    file.close()

func carica_partita():
    if not FileAccess.file_exists("user://salvataggio.json"):
        return
    var file = FileAccess.open("user://salvataggio.json", FileAccess.READ)
    var contenuto = file.get_as_text()
    file.close()

    var dati = JSON.parse_string(contenuto)
    record = dati["record"]
    GestoreGioco.livello = dati["livello"]
{% endhighlight %}

`JSON.stringify()` converte un `Dictionary` in testo, `JSON.parse_string()` fa
l'operazione inversa — gli equivalenti diretti di `.to_json` e
`args.gtk.parse_json_file` in Dragonruby.

## Riepilogo: da Dragonruby a Godot

| Dragonruby                                    | Godot equivalente                                     |
|----------------------------------------------------|-------------------------------------------------------------|
| `args.gtk.write_file(path, testo)`                  | `FileAccess.open(path, FileAccess.WRITE).store_string(testo)` |
| `args.gtk.read_file(path)`                          | `FileAccess.open(path, FileAccess.READ).get_as_text()`       |
| cartella dedicata del gioco (automatica)             | prefisso `"user://"`                                          |
| `dati.to_json`                                       | `JSON.stringify(dati)`                                        |
| `args.gtk.parse_json_file(path)`                    | `JSON.parse_string(testo)`                                    |
