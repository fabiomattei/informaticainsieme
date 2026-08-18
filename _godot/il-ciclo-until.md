---
title: 'Godot: un ciclo until (simulato)'
date: '2026-08-18T10:20:00+01:00'
author: Fabio Mattei
layout: page
---

## GDScript non ha until

Ruby dispone della parola chiave `until`, che esegue il blocco **finché la
condizione è falsa**, terminando non appena diventa vera — l'equivalente di
`while not`. **GDScript non ha `until`**: esiste solo `while`, e per ottenere lo
stesso comportamento leggibile bisogna negare esplicitamente la condizione con
`not`.

| Ruby                | GDScript equivalente         |
|----------------------|---------------------------------|
| `until cont >= 5`    | `while not (cont >= 5)`         |
| `until cont >= 5`    | `while cont < 5` (spesso più chiaro) |

Nella pratica, quasi sempre conviene **riscrivere la condizione al contrario**
piuttosto che usare `not`: `cont >= 5` diventa semplicemente `cont < 5`, che si
legge altrettanto bene senza bisogno di negazioni.

---

## Un ciclo until simulato

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire. Confronta le due forme:
sono equivalenti.

{% highlight gdscript %}
func _ready():
    var cont = 0
    while not (cont >= 5):    # "finché cont non raggiunge 5"
        cont += 1
        print(cont)
{% endhighlight %}

{% highlight gdscript %}
func _ready():
    var cont = 0
    while cont < 5:    # stesso identico risultato, più leggibile
        cont += 1
        print(cont)
{% endhighlight %}

Possiamo tracciare il valore di `cont` a ogni iterazione:

| Iterazione | cont (prima) | cont (dopo) | Condizione cont >= 5 |
|:----------:|:------------:|:-----------:|:----------------------:|
| 1          | 0            | 1           | falsa → continua        |
| 2          | 1            | 2           | falsa → continua        |
| 3          | 2            | 3           | falsa → continua        |
| 4          | 3            | 4           | falsa → continua        |
| 5          | 4            | 5           | falsa → continua        |
| —          | 5            | —           | **vera** → uscita       |

---

## Quando la forma negata è comunque utile

A volte la condizione naturale del problema è "fermati quando succede X", ed è
comunque più leggibile mantenere quella forma con `not`, invece di sforzarsi di
riscriverla al contrario.

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire. Il ciclo scorre un
array di risposte finché non trova la parola "stop".

{% highlight gdscript %}
func _ready():
    var risposte = ["ciao", "prova", "ancora", "stop", "extra"]
    var indice = 0

    while not (risposte[indice] == "stop"):
        print("Hai scritto: %s" % risposte[indice])
        indice += 1

    print("Uscita dal ciclo.")
{% endhighlight %}

Scrivere `while not (risposte[indice] == "stop")` si legge in modo molto simile a
come si leggerebbe `until risposte[indice] == "stop"` in Ruby: "continua finché non
arriva stop".

---

## Simulare begin...end until

Come visto nella pagina sul [ciclo while]({{ site.baseurl }}{% link _godot/il-ciclo-while.md %}.html),
GDScript non ha nemmeno la variante "esegui almeno una volta". Per simulare
`begin ... end until`, si esegue il corpo una prima volta fuori dal ciclo, poi lo si
ripete dentro un `while` con la condizione negata.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire. Il menu si ripresenta
finché l'utente (qui simulato con un array di scelte già decise) non arriva
all'opzione di uscita.

{% highlight gdscript %}
func _ready():
    var scelte = [1, 2, 1, 3]
    var indice = 0
    var scelta = scelte[indice]

    while not (scelta == 3):
        print("--- Menu ---")
        if scelta == 1:
            print("Ciao!")
        elif scelta == 2:
            for i in range(1, 6):
                printraw(str(i) + " ")
            print("")

        indice += 1
        scelta = scelte[indice]

    print("Arrivederci.")
{% endhighlight %}

---

## Esercizi

#### Esercizio 4
Scrivi uno script che, dati due numeri interi, calcoli il massimo comune divisore
(MCD) con l'algoritmo di Euclide, usando un `while` con condizione negata
(`while not (b == 0):`).

#### Esercizio 5
Scrivi uno script che simuli un conto alla rovescia: dato un numero N, stampa i
numeri da N fino a 1 (usa `while not (n == 0):`), poi scrive "Via!".

#### Esercizio 6
Scrivi uno script che, dato un array di numeri, sommi i valori uno alla volta
fermandosi (con `while` e condizione negata) non appena trova il primo numero
negativo, e stampi quanti numeri positivi ha sommato.
