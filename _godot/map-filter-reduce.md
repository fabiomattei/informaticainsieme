---
title: 'Godot: map, filter e reduce'
date: '2026-08-18T10:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Trasformare, filtrare, ridurre

Quando si lavora con gli array, tre operazioni tornano continuamente:

- **trasformare** ogni elemento in qualcos'altro (`map`)
- **filtrare** tenendo solo gli elementi che soddisfano una condizione (`filter`)
- **ridurre** tutti gli elementi a un unico valore (`reduce`)

GDScript (dalla versione 4 di Godot) offre questi tre metodi direttamente
sull'Array, proprio come Ruby. La differenza è come si scrive "una funzione da
passare a un altro metodo": GDScript usa le **funzioni lambda**, introdotte con la
parola chiave `func`.

---

## Funzioni lambda

Una lambda è una funzione senza nome, che può essere assegnata a una variabile o
passata direttamente come argomento.

{% highlight gdscript %}
func _ready():
    var doppio = func(x): return x * 2
    print(doppio.call(5))    # 10
{% endhighlight %}

`.call()` esegue la lambda. Quando la si passa a `map`, `filter` o `reduce`, è
Godot stesso a chiamarla, quindi normalmente non si scrive `.call()` esplicitamente.

---

## Il metodo map

`.map()` restituisce un **nuovo array** in cui ogni elemento è il risultato della
lambda applicata all'elemento originale. L'array di partenza non viene modificato.

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var numeri = [1, 2, 3, 4, 5]

    var doppi = numeri.map(func(n): return n * 2)
    print(doppi)        # [2, 4, 6, 8, 10]

    var quadrati = numeri.map(func(n): return pow(n, 2))
    print(quadrati)      # [1.0, 4.0, 9.0, 16.0, 25.0]

    print(numeri)         # [1, 2, 3, 4, 5] — invariato
{% endhighlight %}

#### Esercizio 2 — map su stringhe

{% highlight gdscript %}
func _ready():
    var parole = ["alice", "bob", "carla"]

    var maiuscole = parole.map(func(p): return p.to_upper())
    print(maiuscole)    # [ALICE, BOB, CARLA]

    var lunghezze = parole.map(func(p): return p.length())
    print(lunghezze)     # [5, 3, 5]
{% endhighlight %}

#### Esercizio 3 — lambda riutilizzabile

Assegnando la lambda a una variabile prima, si può riutilizzarla in più punti,
esattamente come una lambda di Ruby.

{% highlight gdscript %}
func _ready():
    var converti_celsius = func(c): return (c * 9.0 / 5) + 32

    var temperature_c = [0, 20, 37, 100]
    var temperature_f = temperature_c.map(converti_celsius)
    print(temperature_f)   # [32, 68, 98.6, 212]
{% endhighlight %}

---

## Il metodo filter

`.filter()` restituisce un **nuovo array** contenente solo gli elementi per cui la
lambda restituisce `true`.

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var numeri = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

    var pari = numeri.filter(func(n): return n % 2 == 0)
    print(pari)      # [2, 4, 6, 8, 10]

    var dispari = numeri.filter(func(n): return n % 2 != 0)
    print(dispari)    # [1, 3, 5, 7, 9]

    var grandi = numeri.filter(func(n): return n > 5)
    print(grandi)      # [6, 7, 8, 9, 10]
{% endhighlight %}

GDScript non ha un metodo `.reject()` come Ruby: per l'effetto contrario basta
negare la condizione dentro la lambda (`n % 2 != 0` invece di cercare un "not
filter").

#### Esercizio 5 — filter su stringhe

{% highlight gdscript %}
func _ready():
    var parole = ["gatto", "cane", "elefante", "ape", "coccodrillo"]

    var lunghe = parole.filter(func(p): return p.length() > 4)
    print(lunghe)    # [gatto, elefante, coccodrillo]

    var con_a = parole.filter(func(p): return p.find("a") != -1)
    print(con_a)      # [gatto, cane, elefante, ape]
{% endhighlight %}

---

## Il metodo reduce

`.reduce()` **riduce** un array a un singolo valore applicando la lambda in modo
cumulativo. La lambda riceve due parametri: l'**accumulatore** e l'**elemento
corrente**; il secondo argomento di `.reduce()` è il valore iniziale
dell'accumulatore.

{% highlight gdscript %}
array.reduce(func(accumulatore, elemento): return ..., valore_iniziale)
{% endhighlight %}

A differenza di Ruby, in GDScript il valore iniziale **non è opzionale**: va sempre
indicato esplicitamente come secondo argomento.

#### Esercizio 6 — somma e prodotto

{% highlight gdscript %}
func _ready():
    var numeri = [1, 2, 3, 4, 5]

    var somma = numeri.reduce(func(acc, n): return acc + n, 0)
    print(somma)      # 15

    var prodotto = numeri.reduce(func(acc, n): return acc * n, 1)
    print(prodotto)    # 120
{% endhighlight %}

**Traccia di esecuzione per la somma:**

| Passo | acc (prima) | n | acc (dopo) |
|------:|:-----------:|:-:|:----------:|
| 1     | 0           | 1 | 1          |
| 2     | 1           | 2 | 3          |
| 3     | 3           | 3 | 6          |
| 4     | 6           | 4 | 10         |
| 5     | 10          | 5 | 15         |

#### Esercizio 7 — reduce per trovare il massimo

{% highlight gdscript %}
func _ready():
    var numeri = [3, 1, 8, 2, 7, 4]

    var massimo = numeri.reduce(func(acc, n): return acc if acc > n else n, numeri[0])
    print(massimo)    # 8

    # equivalente con il metodo .max():
    print(numeri.max())   # 8
{% endhighlight %}

---

## Concatenare map, filter e reduce

La vera potenza di questi metodi emerge quando si **incatenano**: il risultato di
uno diventa l'input del successivo.

#### Esercizio 8

{% highlight gdscript %}
func _ready():
    var numeri = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

    # somma dei quadrati dei numeri pari
    var risultato = numeri \
        .filter(func(n): return n % 2 == 0) \
        .map(func(n): return pow(n, 2)) \
        .reduce(func(acc, n): return acc + n, 0)

    print(risultato)    # 4 + 16 + 36 + 64 + 100 = 220
{% endhighlight %}

Il carattere `\` a fine riga permette di spezzare un'unica istruzione su più righe
per leggibilità: non è obbligatorio, si può scrivere tutto su una riga sola.

**Passo per passo:**
1. `.filter` → `[2, 4, 6, 8, 10]`
2. `.map`    → `[4, 16, 36, 64, 100]`
3. `.reduce` → `220`

---

## Riepilogo

| Ruby            | GDScript equivalente        |
|------------------|-------------------------------|
| `{ \|x\| ... }`  | `func(x): return ...`         |
| `.map`           | `.map()`                       |
| `.select`        | `.filter()`                    |
| `.reject`        | `.filter()` con condizione negata |
| `.reduce(0) { }` | `.reduce(func(...): ..., 0)`  |

---

## Esercizi

#### Esercizio 9
Dato un array di numeri interi, usa `.map()` per creare un array con il valore
assoluto di ciascun numero (funzione `abs()`).

#### Esercizio 10
Dato un array di stringhe, usa `.filter()` per ottenere solo le parole che iniziano
con la lettera "a" (usa `.begins_with()`).

#### Esercizio 11
Dato un array di numeri, usa `.reduce()` per trovare il minimo senza usare il
metodo `.min()`.

#### Esercizio 12
Dato un array di `Dictionary` `{"nome": ..., "voto": ...}`, usa `.filter()` per
ottenere solo gli studenti promossi (voto ≥ 6), poi `.map()` per estrarre solo i
nomi.

#### Esercizio 13
Dato un array di numeri interi da 1 a 20 (con `range()`), usa una catena
`.filter()` → `.map()` → `.reduce()` per calcolare la somma dei cubi dei numeri
dispari.
