---
title: 'Godot: gli array'
date: '2026-08-18T09:35:00+01:00'
author: Fabio Mattei
layout: page
---

## Una sequenza di elementi

Un **array** è una struttura dati che contiene una sequenza ordinata di elementi. A
differenza di una variabile, che conserva un solo valore, un array può conservarne
molti all'interno di un unico contenitore. Come in Ruby, gli elementi possono essere
di qualsiasi tipo e anche miscelati tra loro.

## Creare un array

Un array si crea con le parentesi quadre `[]`, esattamente come in Ruby.

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var vuoto = []
    var numeri = [0, 1, 2, 3]
    var lettere = ["a", "b", "c", "d", "e"]
    var misto = [42, "ciao", true, 3.14]

    print(numeri.size())    # 4
    print(lettere.size())   # 5
    print(misto)             # [42, ciao, true, 3.14]
{% endhighlight %}

---

## Accedere agli elementi

Ogni elemento ha un **indice** che parte da 0. Gli indici negativi contano dalla
fine: `-1` è l'ultimo elemento, `-2` il penultimo.

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var lettere = ["a", "b", "c", "d", "e"]
    print(lettere[0])     # a
    print(lettere[2])     # c
    print(lettere[-1])    # e  (ultimo)
    print(lettere[-2])    # d  (penultimo)
    print(lettere.front())  # a
    print(lettere.back())   # e
{% endhighlight %}

| indice          |  0  |  1  |  2  |  3  |  4  |
|-----------------|:---:|:---:|:---:|:---:|:---:|
| contenuto       | "a" | "b" | "c" | "d" | "e" |
| indice negativo | -5  | -4  | -3  | -2  | -1  |

---

## Modificare gli elementi

Si può cambiare il valore di un elemento assegnando direttamente tramite il suo
indice.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var lettere = ["a", "b", "c", "d", "e"]
    lettere[0] = "u"
    lettere[-2] = "k"
    print(lettere)    # [u, b, c, k, e]
{% endhighlight %}

---

## Aggiungere e rimuovere elementi

GDScript offre diversi metodi per aggiungere e togliere elementi da un array.

| Metodo               | Effetto                                          |
|-----------------------|----------------------------------------------------|
| `arr.append(x)`       | aggiunge `x` in fondo                              |
| `arr.push_back(x)`    | equivalente ad `append`                            |
| `arr.pop_back()`      | rimuove e restituisce l'ultimo elemento            |
| `arr.push_front(x)`   | aggiunge `x` all'inizio                            |
| `arr.pop_front()`     | rimuove e restituisce il primo elemento            |

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var classe = ["Gino", "Sandra"]
    print(classe)                    # [Gino, Sandra]

    classe.append("Mario")
    print(classe)                    # [Gino, Sandra, Mario]

    var rimosso = classe.pop_back()
    print(rimosso)                   # Mario
    print(classe)                    # [Gino, Sandra]

    classe.push_front("Anna")
    print(classe)                    # [Anna, Gino, Sandra]
{% endhighlight %}

A differenza di Ruby, GDScript non ha un operatore `<<` per aggiungere elementi:
si usa sempre `.append()`.

---

## Sottarray (slicing)

Con `.slice(inizio, fine)` si estrae una porzione dell'array: `inizio` incluso,
`fine` **escluso**, a differenza del range di Ruby.

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var lettere = ["a", "b", "c", "d", "e"]
    print(lettere.slice(1, 4))    # [b, c, d]
    print(lettere.slice(0, 3))    # [a, b, c]
{% endhighlight %}

---

## Verificare se un elemento è presente

L'operatore `in` (visto nella pagina sugli [operatori]({{ site.baseurl }}{% link _godot/operatori.md %}.html))
restituisce `true` se l'elemento cercato è contenuto nell'array.

#### Esercizio 6
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var lettere = ["a", "b", "c", "d", "e"]

    if "c" in lettere:
        print("La lettera c è nella lista")

    if not ("k" in lettere):
        print("La lettera k NON è nella lista")
{% endhighlight %}

---

## Visitare un array

Visitare un array significa **esaminare uno alla volta ciascun elemento** e
applicare delle operazioni a ognuno. In GDScript il modo più naturale è il ciclo
`for`, che itera direttamente sugli elementi (non sugli indici).

#### Esercizio 7
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var numeri = [1, 2, 3, 4, 5, 6]
    for n in numeri:
        if n % 2 == 0:
            print("%d è pari" % n)
{% endhighlight %}

#### Esercizio 8
Quando serve anche l'indice si può iterare su `range(array.size())`, oppure ottenere
coppie indice/valore con `.enumerate()` a partire da Godot 4.4 (nelle versioni
precedenti si usa `range`).

{% highlight gdscript %}
func _ready():
    var nomi = ["Alice", "Bruno", "Carla", "Davide"]
    for i in range(nomi.size()):
        print("%d: %s" % [i, nomi[i]])
{% endhighlight %}

Si può visitare un array anche con un ciclo `while` e un contatore esplicito, utile
quando si vuole controllare manualmente l'indice.

{% highlight gdscript %}
func _ready():
    var numeri = [10, 20, 30, 40, 50]
    var indice = 0
    while indice < numeri.size():
        print(numeri[indice])
        indice += 1
{% endhighlight %}

---

## Metodi utili

GDScript mette a disposizione molti metodi predefiniti che operano sull'intero
array.

| Metodo             | Significato                                      |
|---------------------|-----------------------------------------------------|
| `.size()`           | numero di elementi                                  |
| `.min()`            | elemento più piccolo                                |
| `.max()`            | elemento più grande                                 |
| `.sort()`           | ordina l'array **in-place** (lo modifica)           |
| `.reverse()`        | capovolge l'array **in-place**                      |
| `.has(x)`           | equivalente a `x in arr`                            |
| `.count(x)`         | conta le occorrenze di `x`                          |
| `.duplicate()`      | crea una copia indipendente dell'array              |
| `.erase(x)`         | rimuove la prima occorrenza di `x`                  |

#### Esercizio 9
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var numeri = [5, 3, 8, 1, 9, 2, 7, 4, 6]
    print(numeri.min())    # 1
    print(numeri.max())    # 9

    var ordinati = numeri.duplicate()
    ordinati.sort()
    print(ordinati)         # [1, 2, 3, 4, 5, 6, 7, 8, 9]
{% endhighlight %}

`.duplicate()` è importante: `.sort()` e `.reverse()` modificano l'array su cui sono
chiamati. Se si vuole conservare anche l'ordine originale, occorre lavorare su una
copia.

---

## Esercizi

#### Esercizio 10
Inizializza un array di sei nomi a tua scelta. Cambia il nome all'indice 3 e
stampa tutta la lista.

#### Esercizio 11
Inizializza un array di numeri interi a tua scelta e stampa solo i numeri dispari.

#### Esercizio 12
Scrivi uno script che, dati due array di numeri interi, trovi e stampi gli elementi
comuni a entrambi (usa l'operatore `in`).

#### Esercizio 13
Scrivi uno script che inizializzi un array con i numeri da 1 a 20 (con `range()`) e
calcoli la somma degli elementi in posizione pari (indici 0, 2, 4, …).

#### Esercizio 14
Scrivi uno script che calcoli la frequenza degli elementi di un array (quante volte
compare ciascun elemento) usando un `Dictionary`, e stampi ogni elemento con il suo
numero di occorrenze.
