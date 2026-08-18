---
title: 'Godot: output e debug'
date: '2026-08-18T09:05:00+01:00'
author: Fabio Mattei
layout: page
---

## Niente tastiera, per ora

In Ruby il ciclo naturale di un esercizio è `gets.chomp` per leggere e `puts` per
scrivere: il programma gira in un terminale. Godot invece è pensato per costruire
finestre grafiche interattive: non esiste un modo diretto per "fermare" il gioco e
aspettare che l'utente digiti una riga in una console, perché il gioco deve continuare
a disegnare 60 fotogrammi al secondo comunque.

Per questo, negli esempi di questa sezione dedicata al linguaggio, l'input arriverà
quasi sempre da **variabili già valorizzate nel codice**: si impara il linguaggio
cambiando i valori e osservando come cambia l'output. Il vero input del giocatore —
tastiera, mouse, testo digitato — è trattato più avanti nelle pagine su
[Input]({{ site.baseurl }}{% link _godot/input.md %}.html) e su
[come raccogliere il testo digitato]({{ site.baseurl }}{% link _godot/testo.md %}.html),
una volta che avremo una vera finestra di gioco su cui disegnare.

---

## Scrivere sulla console: print e affini

GDScript offre diverse funzioni per scrivere messaggi nel pannello **Output**.

| Funzione        | Comportamento                                              |
|-----------------|-------------------------------------------------------------|
| `print(x)`      | scrive il valore e va a capo automaticamente                |
| `prints(a, b)`  | scrive più valori separati da uno spazio                    |
| `printraw(x)`   | scrive il valore senza andare a capo                        |
| `printerr(x)`   | scrive il valore come messaggio di errore                   |
| `print_debug(x)`| scrive il valore insieme alla riga e alla funzione chiamante |

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    print("Ciao")
    print("Mondo")

    printraw("Ciao")
    printraw(" ")
    printraw("Mondo")
    print("")
{% endhighlight %}

`print` inserisce un ritorno a capo dopo ogni valore. `printraw` no: le parole
restano sulla stessa riga. Un `print("")` senza altro contenuto scrive solo una riga
vuota, utile per andare a capo dopo una sequenza di `printraw`.

#### Esercizio 2
`prints` è comodo per stampare più valori senza doverli concatenare a mano.

{% highlight gdscript %}
func _ready():
    var nome = "Mario"
    var eta = 25
    prints("Nome:", nome, "Età:", eta)
    # Nome: Mario Età: 25
{% endhighlight %}

---

## Costruire un messaggio con più valori: l'operatore %

GDScript non ha l'interpolazione `#{}` di Ruby dentro le stringhe. Al suo posto usa
l'**operatore di formattazione %**, che sostituisce dei segnaposto in una stringa con
i valori passati in un array.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var nome = "Alice"
    var eta = 17

    print("Mi chiamo " + nome + " e ho " + str(eta) + " anni.")
    print("Mi chiamo %s e ho %d anni." % [nome, eta])
{% endhighlight %}

La seconda riga è la forma preferita in GDScript: più leggibile, e converte
automaticamente i valori nel formato giusto.

| Segnaposto | Significato          |
|------------|-----------------------|
| `%s`       | qualsiasi valore, convertito a stringa |
| `%d`       | numero intero          |
| `%f`       | numero decimale        |

{% highlight gdscript %}
var lato = 5
print("Area del quadrato: %d" % (lato * lato))          # Area del quadrato: 25
print("Pi greco: %f" % 3.14159)                           # Pi greco: 3.140000
{% endhighlight %}

Quando il segnaposto è uno solo, non serve racchiudere il valore tra parentesi
quadre: `"Ciao %s" % nome` funziona esattamente come `"Ciao %s" % [nome]`.

---

## Debug rapido: stampare qualsiasi valore

`print()` accetta qualsiasi tipo di dato, non solo stringhe: array, dizionari, numeri,
oggetti. Questo lo rende comodo per il debug, un po' come `p` in Ruby.

{% highlight gdscript %}
func _ready():
    print(42)
    print("42")
    print(null)
    print([1, 2, 3])
    print({"a": 1, "b": 2})
{% endhighlight %}

---

## Riepilogo

| Ruby              | GDScript equivalente                     |
|-------------------|-------------------------------------------|
| `puts valore`     | `print(valore)`                           |
| `print valore`    | `printraw(valore)` (nessun ritorno a capo)|
| `p valore`        | `print(valore)` (funziona su ogni tipo)   |
| `"testo #{var}"`  | `"testo %s" % var`                        |
| `gets.chomp`      | non esiste: l'input arriva da UI o Input  |

---

## Esercizi

#### Esercizio 4
Scrivi uno script con due variabili `nome` ed `eta` e stampa la frase
`"Ciao NOME, l'anno prossimo avrai ETA+1 anni"` usando l'operatore `%`.

#### Esercizio 5
Scrivi uno script che stampi una tabellina con `prints`, allineando su ogni riga un
numero da 1 a 5 e il suo quadrato.

#### Esercizio 6
Scrivi uno script che, data una variabile `prezzo` e una variabile `sconto` (in
percentuale), stampi `"Prezzo scontato: %f €"` con il prezzo finale.
