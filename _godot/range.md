---
title: 'Godot: range()'
date: '2026-08-18T09:45:00+01:00'
author: Fabio Mattei
layout: page
---

## Una sequenza di numeri generata al volo

Ruby ha un vero **tipo Range** (`1..10`), che può essere assegnato a una variabile e
interrogato con metodi come `.sum` o `.include?`. GDScript non ha un tipo
equivalente: al suo posto c'è la **funzione** `range()`, pensata quasi
esclusivamente per generare la sequenza di valori su cui far girare un ciclo `for`.

{% highlight gdscript %}
range(5)         # 0, 1, 2, 3, 4       — da 0 (incluso) a 5 (escluso)
range(1, 5)      # 1, 2, 3, 4          — da 1 (incluso) a 5 (escluso)
range(1, 10, 2)  # 1, 3, 5, 7, 9       — con passo 2
{% endhighlight %}

Il limite superiore è **sempre escluso**, un po' come l'operatore `...` di Ruby: non
esiste una forma di `range()` con l'estremo superiore incluso.

---

## range() nei cicli

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    for i in range(1, 6):
        print(i)
{% endhighlight %}

Output: 1, 2, 3, 4, 5 — il 6 non compare mai, perché `range()` esclude sempre
l'estremo superiore.

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire. Osserva come si calcola
la somma dei numeri da 1 a 100 con una variabile accumulatore.

{% highlight gdscript %}
func _ready():
    var acc = 0
    for i in range(1, 101):
        acc += i
    print(acc)    # 5050
{% endhighlight %}

Da notare: per includere il 100 bisogna scrivere `range(1, 101)`, non
`range(1, 100)` — è un errore comune per chi viene da Ruby, dove `1..100` include
naturalmente il 100.

---

## range() con un solo argomento

Se si passa un solo valore, `range()` parte automaticamente da 0.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    for i in range(5):
        print(i)    # 0, 1, 2, 3, 4
{% endhighlight %}

Questa forma è la più comune quando serve solo "ripetere N volte", senza usare
davvero il valore di `i`.

{% highlight gdscript %}
func _ready():
    for i in range(3):
        print("ciao")    # stampato 3 volte
{% endhighlight %}

---

## range() con passo negativo

Per contare all'indietro si usa un passo negativo, ricordando che anche in questo
caso l'estremo superiore (qui il primo argomento, dato che si scende) resta
incluso e il secondo escluso.

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    for i in range(10, 0, -1):
        print(i)    # 10, 9, 8, ..., 1
    print("Via!")
{% endhighlight %}

---

## Verificare se un valore è in un intervallo

GDScript non ha un metodo `.include?()` su `range()` perché non esiste un vero
oggetto range da interrogare: la verifica va fatta con un confronto diretto, oppure
con `match` e le sue clausole di intervallo.

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var eta = 15

    match eta:
        0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12:
            print("Bambino")
        _:
            if eta <= 17:
                print("Adolescente")
            elif eta <= 64:
                print("Adulto")
            else:
                print("Anziano")
{% endhighlight %}

Per intervalli ampi come questo, in pratica è più leggibile una catena di
`if/elif`, vista nella pagina sulle [condizioni]({{ site.baseurl }}{% link _godot/condizioni.md %}.html):

{% highlight gdscript %}
func _ready():
    var eta = 15
    if eta <= 12:
        print("Bambino")
    elif eta <= 17:
        print("Adolescente")
    elif eta <= 64:
        print("Adulto")
    else:
        print("Anziano")
{% endhighlight %}

---

## Da range() ad array

Se serve davvero un array di numeri (per esempio per usare i metodi visti nella
pagina sugli [array]({{ site.baseurl }}{% link _godot/array.md %}.html)), si può
convertire il risultato di `range()` esplicitamente.

{% highlight gdscript %}
func _ready():
    var numeri = range(1, 6)
    print(numeri)          # [1, 2, 3, 4, 5]
    print(numeri.size())   # 5
    print(numeri.max())    # 5
{% endhighlight %}

In Godot 4 `range()` restituisce già un `Array`, quindi tutti i metodi degli array
funzionano direttamente su di esso, a differenza di Ruby dove un `Range` va prima
convertito con `.to_a`.

---

## Esercizi

#### Esercizio 6
Scrivi uno script che, dato un numero N, stampi tutti i numeri pari compresi tra 1 e
N usando `range()` con passo 2.

#### Esercizio 7
Scrivi uno script che generi la tabella della moltiplicazione di un numero N usando
`range()` e un ciclo `for`.

#### Esercizio 8
Scrivi uno script che calcoli la somma di tutti i numeri da 1 a N usando `range()` e
il metodo `.reduce()` sull'array risultante (vedi [map, filter e reduce]({{ site.baseurl }}{% link _godot/map-filter-reduce.md %}.html)).
